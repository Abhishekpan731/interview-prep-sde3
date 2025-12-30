# High-Level Design: Typeahead Suggestion System (Autocomplete)

## 1. Requirements & Scope

### 1.1 Functional Requirements
*   **Autocomplete:** As the user types, suggest top $K$ (e.g., 5) query completions.
*   **Ranking:** Suggestions should be ranked by popularity (frequency) or relevance.
*   **Data Freshness:** New trending queries (e.g., "World Cup Final") should appear in suggestions reasonably fast.

### 1.2 Non-Functional Requirements
*   **Low Latency:** **Critical**. The system must respond within **100ms** (before the user types the next character).
*   **High Availability:** 99.99%.
*   **Scalability:** Support billions of queries per day.
*   **Consistency:** Eventual consistency is acceptable. It's okay if a new query takes a few hours to appear.

---

## 2. Capacity Estimation & Scale

### 2.1 Traffic Estimates
*   **DAU:** 10 Million.
*   **Avg Searches:** 10 per user/day.
*   **Avg Query Length:** 20 characters = 20 partial requests per search.
*   **Total QPS:** 10M * 10 * 20 / 86400 ≈ **23,000 QPS**. Peak QPS: ~50k.

### 2.2 Storage Estimates
*   **Unique Queries:** Assume 100 Million unique queries in the database.
*   **Avg Query Size:** 30 bytes.
*   **Total Data Size:** 100M * 30 bytes ≈ **3 GB**.
    *   *Insight:* The entire active dataset fits in **RAM**! This is key for performance.

---

## 3. High-Level Architecture

### 3.1 Logical Components
1.  **Client:** Sends prefix requests (utilizing debounce/throttling).
2.  **Load Balancer:** Distributes traffic.
3.  **Suggestion Service:** Read-only service serving from in-memory Cache/Trie.
4.  **Query Assembler / Analytics:** Collects logs and aggregates frequencies.
5.  **Trie Builder:** Background worker that rebuilds the optimal data structure.

### 3.2 Mermaid Architecture Diagram
```mermaid
graph TD
    Client([User Browser])
    
    subgraph "Online Real-time Path"
        LB[Load Balancer]
        AppSvc[Suggestion Service (API)]
        TrieCache[(In-Memory Trie Cluster)]
    end
    
    subgraph "Offline / Async Path"
        Analytics[Log Collector]
        Aggregator[Stream Aggregator (Flink)]
        DB[(Frequency DB: Cassandra)]
        Builder[Trie Builder Worker]
        Snapshot[(Object Store: S3)]
    end

    Client -->|GET /prefix=g| LB
    LB --> AppSvc
    AppSvc -->|Lookup| TrieCache
    
    Client -.->|Log Query| Analytics
    Analytics --> Aggregator
    Aggregator -->|Update Frequencies| DB
    
    Builder -->|Fetch Top Queries| DB
    Builder -->|Build/Serialize| Snapshot
    
    Snapshot -->|Reload| TrieCache
```

---

## 4. Data Structure: The Trie (Prefix Tree)

### 4.1 Basic Trie
*   **Structure:** Tree where each edge is a character. A path from Root to Node represents a prefix.
*   **Problem:** Searching the subtree for the "Top 5" is $O(V)$ where $V$ is total nodes in the subtree. This is **too slow** for latency requirements.

### 4.2 Optimized Trie (Cache Top-K at Node)
*   **Optimization:** Store the top 5 most popular queries **directly at each node**.
*   **Example:**
    *   Node `b`: Stores Top 5 for "b..." -> ["best buy", "bee movie", "baseball"].
    *   Node `be`: Stores Top 5 for "be..." -> ["best buy", "beer", "bee movie"].
    *   Node `bes`: Stores Top 5 for "bes..." -> ["best buy", "best western"].
*   **Read Time:** $O(1)$. Just return the pre-computed list stored at the node matching the prefix.
*   **Write Time:** Increases complexity, but we build offline.

---

## 5. Core Workflows

### 5.1 Read Flow (Autocomplete)
1.  User types "am".
2.  Client sends `GET /suggest?q=am`.
3.  Load Balancer routes to **Suggestion Service**.
4.  Service looks up `am` key in the In-Memory Trie.
5.  Returns the cached list `["amazon", "amazon prime", "amd"]`.
6.  Total Latency: ~5ms (Network) + ~0.1ms (Memory Lookup).

### 5.2 Write/Update Flow (Frequency Updates)
1.  User enters "amazing spiderman" and hits enter.
2.  **Log Collector** captures the query.
3.  **Aggregator:**
    *   Naive: Update DB for every query. (Too slow).
    *   Optimized: Sample traffic (1 in 1000) logging OR aggregate in memory buffers (MapReduce/Flink) for 5 minutes.
4.  **Update DB:** Increment count for "amazing spiderman" in Cassandra.

### 5.3 Trie Rebuild Flow (The "Epoch" Approach)
How do we update the live Trie? Modifying a locked tree in-memory under 50k QPS is dangerous.
*   **Strategy:** **Snapshot & Replace**.
    1.  **Weekly/Daily:** Scan the DB for all queries with freq > threshold.
    2.  **Builder:** Builds a completely new serialized Trie structure.
    3.  **Deploy:** Upload to S3.
    4.  **Server:** Suggestion Services download the new file and hot-swap the reference in memory.

---

## 6. Scalability & Caching

### 6.1 Browser Caching
*   If user types "amaz", we return suggestions.
*   If user types "amazo", and we already cached the result for "amaz", does it help? Not directly, but...
*   **Browser Cache:** `Cache-Control: public, max-age=3600`. If the user backspaces and types "am" again, don't hit the server.

### 6.2 Server-Side Sharding (If Data Grows)
If the Trie exceeds RAM (e.g., we want to support 50 languages):
*   **Shard by Prefix:**
    *   Server A: `a-m`
    *   Server B: `n-z`
*   **Load Balancing:** The Load Balancer looks at the first char of query `q` and routes to the correct shard.
*   **Result:** Can hold massive datasets while keeping operations purely in-memory.

---

## 7. Data Freshness (Real-Time Trends)

**Problem:** The weekly rebuild misses "Breaking News" happening right now.

**Solution:** **Hybrid Architecture**.
1.  **Main Trie:** Built weekly (Historical data).
2.  **Trending Trie:** Built every 15 minutes (Recent logs).
3.  **Merge:**
    *   Service queries both.
    *   Relevance Formula: `FinalScore = (HistoricalFreq * w1) + (TrendingFreq * w2)`.
    *   If a query is in Trending, it boosts its rank significantly.

---

## 8. Frequency Update & Sampling

*   **Sampling:** Storing every single query search is wasteful.
    *   We log only 1 out of N requests (e.g., N=100).
    *   We assume that if a query is truly popular, it will statistically appear in the sample often enough.
*   **Privacy:** Strip identifying information. We only care about the *string query*, not *who* searched it.

---

## 9. Fault Tolerance

*   **Trie Builder Failure:** If the builder job fails, the system continues serving stale data (yesterday's suggestions). This is a **Soft Failure**.
*   **Service Crash:** Since the Trie is stateless (read-only in memory), we just spin up a new container. Ideally, use ZooKeeper/Etcd to manage shard assignments if sharding.

---

## 10. Summary of Optimizations

1.  **Store Top-K at Nodes:** Reduces lookup from $O(V)$ to $O(1)$.
2.  **In-Memory Storage:** Eliminates disk I/O latency.
3.  **Sampling:** Reduces analytics WRITE load by 99%.
4.  **Hybrid Tries:** Solves the usage pattern of Historical (Stable) vs Trending (Volatile) queries.
