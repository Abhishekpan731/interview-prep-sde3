# High-Level Design: Metrics Monitoring System (Prometheus / Datadog)

## 1. Requirements & Scope

### 1.1 Functional Requirements
*   **Data Ingestion:** Collect metrics from thousands of services (CPU, Memory, Request Counts).
*   **Querying:** Support a powerful query language (like PromQL) for aggregation and analysis.
*   **Alerting:** Evaluate rules (e.g., `cpu > 90%`) and trigger notifications.
*   **Visualization:** Efficient retrieval for dashboards (Grafana).
*   **Data Retention:** Keep high-resolution data for X days and downsampled data for Y months.

### 1.2 Non-Functional Requirements
*   **Write Heavy:** The system must handle millions of writes per second.
*   **Availability:** Alerting is critical. Ingestion should prioritize availability (AP).
*   **High Throughput:** Scrape targets in parallel without blocking.
*   **Latency:** Query latency < 100ms for recent data.
*   **Scalability:** Horizontally scalable ingestion and storage.

---

## 2. Capacity Estimation & Scale

### 2.1 Traffic Estimates
*   **Hosts/Services:** 50,000 active instances.
*   **Metrics per Host:** 1,000 unique metrics (Time Series).
*   **Scrape Interval:** Every 10 seconds.
*   **Data Points per Second (DPS):** $50,000 \times 1,000 / 10 = \mathbf{5,000,000 \text{ DPS}}$.

### 2.2 Storage Estimates
*   **Data Point Size:** Using **Gorilla Compression**, a timestamp + float64 value compresses to **~1.3 bytes**. Let's be conservative and say **2 bytes**.
*   **Daily Storage:** $5M \text{ DPS} \times 86400 \text{ sec} \times 2 \text{ bytes} \approx \mathbf{864 \text{ GB / Day}}$.
*   **Yearly Storage (Raw):** ~315 TB.
*   **With Downsampling:** We reduce resolution for older data (1m -> 1h), reducing specific retention costs by 90%.

---

## 3. High-Level Architecture

### 3.1 Logical Components
1.  **Scraper (Retrieval):** Discover targets (Consult Service Discovery) and Pull metrics.
2.  **Storage Engine (TSDB):** In-memory buffer + Disk storage with compaction.
3.  **Compactor / Downsampler:** Optimizes storage and creates lower-resolution aggregates.
4.  **Query Engine:** Executes PromQL queries.
5.  **Alert Manager:** Deduplicates and sends alerts.
6.  **Service Discovery:** Kubernetes/Consul integration to find dynamic pods.

### 3.2 Mermaid Architecture Diagram
```mermaid
graph TD
    Target([Microservice / Pod])
    
    subgraph "Scraping & Ingestion"
        SD[Service Discovery (K8s/Consul)]
        Scraper[Scraper / Collector]
        LB[Load Balancer]
    end
    
    subgraph "Time Series Database (TSDB)"
        Head[Head Block (In-Memory)]
        WAL[Write Ahead Log]
        DiskBlocks[(Persistent Blocks)]
        Compactor[Compactor / Downsampler]
    end
    
    subgraph "Read & Alert Path"
        QueryEng[Query Engine]
        RuleEng[Rule Evaluator]
        AlertMgr[Alert Manager]
        Grafana[Dashboard / UI]
    end
    
    subgraph "External"
        PagerDuty[PagerDuty / Email]
    end

    Target -.->|Expose /metrics| Scraper
    Scraper -->|Get Targets| SD
    
    Scraper -->|Write Samples| Head
    Scraper -->|Write WAL| WAL
    
    Head -->|Flush (2hrs)| DiskBlocks
    DiskBlocks --> Compactor
    
    Grafana --> QueryEng
    QueryEng -->|Fetch Data| Head
    QueryEng -->|Fetch Data| DiskBlocks
    
    RuleEng -->|Evaluate Rules| QueryEng
    RuleEng -->|Fire Alert| AlertMgr
    AlertMgr -->|Notify| PagerDuty
```

---

## 4. Data Model & TSDB Design

### 4.1 Data Model
*   **Metric Name:** `http_requests_total`
*   **Labels (Dimensions):** `{method="POST", handler="/api/tracks", status="200"}`
*   **ID:** Only the unique combination of Name + Labels forms a **Series ID**.
*   **Sample:** `(Timestamp, Value)`

### 4.2 TSDB Internals (The "Prometheus" Model)
1.  **Head Block (In-Memory):**
    *   Holds the last 2 hours of data.
    *   Data structure: `Map<SeriesID, List<Sample>>`.
    *   **WAL (Write Ahead Log):** Every incoming sample is appended to a distinct WAL file on disk to survive crashes.
2.  **Persistent Blocks (Disk):**
    *   Every 2 hours, the Head block is flushed to an immutable **Block** on disk.
    *   **Inverted Index:** Maps `Label -> SeriesID`. Allows fast lookup like `method="POST"`.
    *   **Chunking:** Samples are compressed into chunks (XOR compression).
3.  **Compaction:**
    *   Merges smaller blocks (2h) into larger blocks (10h, 1d, etc.) to reduce fragmentation and index size.

### 4.3 Compression (Facebook Gorilla Algorithm)
*   **Timestamps:** We don't store `1609459200, 1609459210, 1609459220`. We store the **Delta-of-Delta**.
    *   Delta: `10, 10`. Delta-of-Delta: `0`. Compresses to nearly 1 bit.
*   **Values (Float64):** XOR the current value with the previous value. If the values are close (CPU goes 50.1 -> 50.2), the XOR result has many leading zeros, which compress heavily.

---

## 5. Storage Scale: Data Retention & Downsampling

**Problem:** Keeping 5M DPS raw data for 1 year is too expensive.

**Solution: Downsampling (Rollups)**
We maintain retention tiers:
1.  **Raw (10s resolution):** Keep for **14 Days**. Used for deep debugging.
2.  **Medium (5m resolution):** Keep for **90 Days**. Used for weekly trends.
3.  **Low (1h resolution):** Keep for **1 Year**. Used for capacity planning.

**How Downsampling Works:**
The **Compactor** reads a block of raw samples and computes aggregates: `Min`, `Max`, `Sum`, `Count` for each 5-minute window. We must calculate these 4 statistics because you cannot average averages accurately later.

---

## 6. Core Workflows

### 6.1 Data Collection: Pull vs Push
*   **Pull Model (Prometheus):**
    *   **Pros:** Better control. The monitoring system decides rate. Easier to detect "Service Down" (if scrape fails, service is down).
    *   **Cons:** Hard to traverse NAT/Firewalls.
*   **Push Model (Graphite/Datadog agent):**
    *   **Pros:** Good for short-lived jobs (batch jobs) that die before a scrape occurs.
    *   **Cons:** Easier to DDoS the monitoring server.
*   **Decision:** **Pull** for long-running services. **Push Gateway** for batch jobs.

### 6.2 Alerting Flow
1.  **Rule Evaluator:** A background loop runs every 1 minute.
2.  **Query:** Executes `avg(cpu_usage) > 90` over the last 5m.
3.  **State:**
    *   `Pending`: Condition met, but waiting for `for: 5m` duration.
    *   `Firing`: Condition met for enough time.
4.  **Send:** `Firing` event sent to **Alertmanager**.
5.  **Alertmanager:**
    *   **Grouping:** Group 100 alerts from 100 pods into 1 email: "High CPU on Cluster X".
    *   **Inhibition:** If "Datacenter Down" is firing, suppress "High Latency" alerts (Root cause suppression).
    *   **Routing:** Send P1 to PagerDuty, P3 to Slack.

---

## 7. Scalability & Federation

### 7.1 Federation (Hierarchical Scaling)
A single Prometheus server usually caps at ~10M active series. For 50k hosts, we need more.
*   **Sharding (Functional):** Server A scrapes SQL DBs; Server B scrapes Web Servers.
*   **Federation:** A Global Prometheus scrapes **aggregated** data from Server A and Server B.
    *   *Edge:* Store full raw data.
    *   *Global:* Store only aggregated service-level SLIs (Service Level Indicators).

### 7.2 Remote Read/Write (Cortex / Thanos)
*   Prometheus local storage is not clustered.
*   **Remote Write:** Configure Prometheus to forward all samples to a centralized clustered store (like **Thanos** or **Cortex**) which uses S3 as the backend.
*   **Thanos:** Sidecar approach. It sits next to Prometheus and uploads Block files to S3 Object Storage. Queries are fanned out to both S3 and local Prometheus instances.

---

## 8. Reliability & Fault Tolerance

*   **HA Pairs:** Run two identical Prometheus servers scraping the same targets.
    *   *Problem:* Duplicate alerts.
    *   *Solution:* Alertmanager Cluster handles deduplication. If `Prometheus-A` sends Alert X and `Prometheus-B` sends Alert X, Alertmanager merges them.
*   **Scrape Failure:** If a scrape fails/timeouts, record a `up=0` metric. This allows alerting on `up == 0`.

---

## 9. Evolution

*   **Metric Churn:** If a developer adds a `request_id` label to a metric, cardinality explodes to Infinity.
    *   *Protection:* Scraper limits the max number of unique series per target (e.g., 5000). Drop metrics exceeding limit.
*   **Exemplars:** Attaching a Trace ID to a Metric sample. "Latency is high (Metric), here is the Trace ID (a23f) that caused it." This bridges Metrics and Tracing.
