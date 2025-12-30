# High-Level Design: Distributed Web Crawler (Googlebot)

## 1. Requirements & Scope

### 1.1 Functional Requirements
*   **Crawl:** Given a set of seed URLs, download content and extract links.
*   **Discovery:** Continuously discover new URL links from downloaded pages.
*   **Update:** Periodically recrawl pages to detect changes (freshness).
*   **Politeness:** Respect `robots.txt` and do not overwhelm target servers.

### 1.2 Non-Functional Requirements
*   **Scalability:** Must handle billions of pages.
*   **Performance:** High throughput (download thousands of pages/sec).
*   **Extensibility:** Support new content types (Images, PDFs) easily.
*   **Robustness:** Handle malformed HTML, unresponsive servers, and crawler traps (infinite loops).

---

## 2. Capacity Estimation & Scale

### 2.1 Scale Estimates
*   **Target:** 1 Billion webpages/month.
*   **QPS:** 1B / 30 days / 24 hrs / 3600s ≈ **400 pages/sec**.
*   **With Updates:** If we want to recrawl high-value pages frequently, we target maybe **2,000 pages/sec** peak.
*   **Storage:**
    *   Avg page size: 100KB (Text only).
    *   1B * 100KB = **100 TB / month**.
    *   5 Years = **6 Petabytes**.

---

## 3. High-Level Architecture

### 3.1 Logical Components
1.  **URL Frontier:** The "ToDo" list. Manages priority and scheduling.
2.  **DNS Resolver:** Custom high-performance caching DNS server.
3.  **HTML Downloader (Worker):** Fetches the raw content.
4.  **Content Parser:** Extracts links and validates content.
5.  **Deduplication (Seen URLs):** Checks if URL or Content has already been processed.
6.  **Storage:** BigTable / HDFS for content.

### 3.2 Mermaid Architecture Diagram
```mermaid
graph TD
    Seed[Seed URLs] --> Frontier
    
    subgraph "URL Frontier (The Brain)"
        Frontier[Frontier / Scheduler]
        Prioritizer[Prioritizer Queue]
        Politeness[Politeness / Host Queue]
    end
    
    subgraph "Worker Cluster"
        Resolver[DNS Resolver Cache]
        Downloader[HTML Downloader]
        Parser[Link Extractor / Parser]
    end
    
    subgraph "Optimization & Dedupe"
        DedupURL[URL Filter (Bloom Filter)]
        DedupContent[Content Fingerprinter (SimHash)]
        Robots[Robots.txt Cache]
    end
    
    subgraph "Data Storage"
        DB_Content[(Document Store: BigTable)]
        DB_Index[(Search Index)]
    end

    Frontier -->|Get Batch| Downloader
    Downloader -->|Resolve IP| Resolver
    Downloader -->|Check Allowed| Robots
    
    Downloader -->|Fetch HTTP| Internet((The Web))
    Internet -->|HTML| Downloader
    
    Downloader -->|Raw Content| DB_Content
    Downloader -->|Raw Content| Parser
    
    Parser --> DedupContent
    DedupContent -->|Unique Content| DB_Index
    
    Parser -->|Extracted URLs| DedupURL
    DedupURL --"New URL"--> Prioritizer
    Prioritizer --> Politeness
    Politeness --> Frontier
```

---

## 4. The URL Frontier (Core Component)

The Frontier makes two critical decisions:
1.  **Which** URL to crawl next? (Prioritization)
2.  **When** to crawl it? (Politeness)

### 4.1 Prioritization (Business Logic)
We don't want to crawl 1 million spam pages.
*   **Input:** URL.
*   **Logic:** Assign a `score` based on PageRank, update frequency, or domain authority.
*   **Queueing:** Use biased queues (High Priority Queue vs Low Priority Queue). The Scheduler reads from High Priority 70% of the time.

### 4.2 Politeness (Infrastructure Logic)
If we send 1,000 requests/sec to `wikipedia.org`, we are a DDoS attacker.
*   **Mapping:** Map `hostname` -> `Queue`.
*   **Constraint:** "Max 1 thread per host" or "Max 1 request per 2 seconds per host".
*   **Implementation:** A separate FIFO queue for each domain (`google.com`, `cnn.com`). A "Heap" manages which queue is ready to be popped based on the `NextAvailableTime` timestamp.

---

## 5. Deduplication Strategies

### 5.1 URL Deduplication (Have we seen this link?)
*   **Problem:** `google.com` vs `www.google.com` vs `google.com/` are the same.
*   **Solution:**
    1.  **Canonicalization:** Normalize string (lowercase, remove hash fragments, sort params).
    2.  **Bloom Filter:** A probabilistic data structure.
        *   Fast and memory efficient (bits, not strings).
        *   If it says "No", the URL is definitely new.
        *   If it says "Yes", it *might* be seen (False Positive). We check a disk-based Key-Value store to confirm.

### 5.2 Content Deduplication (Is this a copy?)
*   **Problem:** Mirror sites or identical news articles on different subdomains. Crawling 100 copies wastes resources.
*   **Solution:** **SimHash (Locality Sensitive Hashing)**.
    *   Traditional Hash (MD5): `hash("cat")` vs `hash("cats")` are totally different.
    *   **SimHash:** Logic ensures that similar documents have similar hash bits.
    *   We calculate the Hamming Distance between the new page's SimHash and existing hashes.
    *   If distance < $K$, we mark it as a duplicate and discard.

---

## 6. Core Workflows (The Loop)

1.  **Select:** Frontier gives Worker a list of URLs (e.g., all for `cnn.com`).
2.  **DNS:** Worker checks local DNS cache. If miss, resolves and caches.
3.  **Robots:** Worker checks `robots.txt` cache. Is `/admin` allowed? No -> Skip.
4.  **Fetch:** Worker sends HTTP GET.
    *   *Timeout:* If > 10s, abort.
    *   *Status:* If 301 Redirect, add new URL to Frontier. If 200, process.
5.  **Parse:** Extract all `<a href="...">` tags.
6.  **Filter:** Pass extracted links through Bloom Filter.
7.  **Loop:** Add new unique links back to Frontier.

---

## 7. Storage & Robustness

### 7.1 Data Storage (BigTable)
*   **Row Key:** `Reverse Domain` (e.g., `com.google.news:http`).
    *   *Why?* Keeps all pages from the same domain physically close on disk, improving compression and read locality.
*   **Columns:** Content, Headers, Timestamp, SimHash.

### 7.2 Handling "Spider Traps"
*   **Infinite Deep Paths:** `example.com/a/b/c/d/e/...`
    *   *Fix:* Max URL length limit and Max Path Depth limit.
*   **Infinite Dynamic Calendars:** `example.com/calendar?year=3000`
    *   *Fix:* Detection of query parameters that don't change content significantly (SimHash helps here).

---

## 8. Scalability & Performance

*   **Distributed Architecture:** Workers are stateless. We can spin up 10,000 Docker containers.
*   **Checkpointing:** The Frontier must save its state to disk (Redis/Cassandra) periodically. If the system crashes, we reload the crawling queues from the last checkpoint.
*   **DNS is the BottleNeck:** Standard DNS servers hate high traffic.
    *   *Solution:* We build our own specialized DNS servers that cache explicitly and respect TTLs aggressively.

---

## 9. Evolution

*   **Rendering (The Modern Web):** React/Angular apps don't return HTML content; they return JS.
    *   *Upgrade:* The Downloader needs a **Headless Chrome** (Puppeteer) renderer.
    *   *Cost:* Rendering JS takes 10x more CPU/Time. We only render for high-value domains or when the raw HTML is suspiciously empty.
