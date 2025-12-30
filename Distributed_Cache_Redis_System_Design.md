# High-Level Design: Distributed Cache (Redis Cluster)

## 1. Requirements & Scope

### 1.1 Functional Requirements
*   **Operations:** `GET(key)`, `SET(key, value)`, `DELETE(key)`. Support for TTL (Time-To-Live).
*   **Distribution:** Automatically shard data across multiple nodes.
*   **Persistence:** Ability to restore state after a restart.
*   **Scalability:** Add/remove nodes without downtime.

### 1.2 Non-Functional Requirements
*   **Latency:** Ultra-low latency (**sub-millisecond** read/write).
*   **Availability:** High Availability (HA) via replication and automatic failover.
*   **Consistency:** Eventual consistency is acceptable during partitions, but Strong consistency preferred within a shard.
*   **Throughput:** Handle millions of QPS.

---

## 2. Capacity Estimation & Scale

### 2.1 Scale Estimates
*   **Total Cache Size:** 10 TB.
*   **Memory per Node:** 64 GB (Sweet spot for Redis performance/forking).
*   **Number of Nodes:** $10 \text{ TB} / 64 \text{ GB} \approx 160$ primary nodes. With 1 replica each -> **320 Nodes**.
*   **QPS:** 10 Million QPS.
*   **Load per Node:** $10\text{M} / 160 \approx 62,500$ QPS per node (Very comfortable for Redis).

---

## 3. High-Level Architecture

### 3.1 Logical Components
1.  **Client Library:** Smart client that knows the hashing algorithm and node topology.
2.  **Cache Nodes:** The storage engines (Shards).
3.  **Cluster Manager (Gossip Protocol):** Nodes talking to each other to detect failures.
4.  **Proxy (Optional):** Layer like **Twemproxy** or **Envoy** if we can't use smart clients.

### 3.2 Mermaid Architecture Diagram
```mermaid
graph TD
    Client([Smart Client / App])
    
    subgraph "Distributed Cache Cluster"
        Node1[Node A: Slots 0-5500]
        Node2[Node B: Slots 5501-11000]
        Node3[Node C: Slots 11001-16383]
        
        Rep1[Replica A]
        Rep2[Replica B]
        Rep3[Replica C]
    end
    
    subgraph "Persistence"
        Disk1[(AOF / RDB Disk)]
    end

    Client -->|HASH(key) -> Node A| Node1
    Client -->|HASH(key) -> Node B| Node2
    Client -->|HASH(key) -> Node C| Node3
    
    Node1 -.->|Replicate| Rep1
    Node2 -.->|Replicate| Rep2
    Node3 -.->|Replicate| Rep3
    
    Node1 <-->|Gossip / Heartbeat| Node2
    Node2 <-->|Gossip / Heartbeat| Node3
    Node3 <-->|Gossip / Heartbeat| Node1
    
    Node1 -->|fsync| Disk1
```

---

## 4. Data Sharding & Distribution

### 4.1 Consistent Hashing vs Hash Slots
*   **Consistent Hashing (Ring):** Used by Cassandra/Dynamo. Good for distribution, but complex to implement "exact" mapping without virtual nodes.
*   **Redis Cluster Approach (Hash Slots):**
    *   The key space is divided into fixed **16,384 Slots**.
    *   **Mapping:** `Slot = CRC16(key) % 16384`.
    *   **Assignment:** Every node is responsible for a subset of slots.
        *   Node A: [0 - 5500]
        *   Node B: [5501 - 11000]
        *   Node C: [11001 - 16383]

### 4.2 Client Routing
How does the client know where to go?
1.  **Moved Redirection:** Client sends `SET foo bar` to Node A.
    *   If `foo` belongs to Node B, Node A responds: `-MOVED 3999 127.0.0.2:6379`.
    *   Client updates its local "Slot Map" and redirects to Node B.
2.  **Smart Client:** The client periodically downloads the Slot Map so it hits the correct node on the first try (0-hop).

---

## 5. Persistence (Durability)

**Problem:** RAM is volatile. If power fails, data is lost.

### 5.1 Snapshotting (RDB - Redis Database)
*   **Mechanism:** Fork the process and write a point-in-time snapshot to disk every X minutes.
*   **Pros:** Compact file, fast restart.
*   **Cons:** Data loss of the last X minutes. High CPU/Memory usage during fork.

### 5.2 Append Only File (AOF)
*   **Mechanism:** Log every write operation (`SET`, `INCR`) to a file.
*   **sync Strategies:**
    *   `always`: Sync every write. Slow but safest.
    *   `everysec`: Buffer and sync every 1 second. (Best trade-off).
    *   `no`: Let OS decide.
*   **Pros:** Minimal data loss (1 sec).
*   **Cons:** File grows huge. Needs **Rewrite** (Compaction) to remove redundant ops (`INCR x`, `INCR x` -> `SET x 2`).

### 5.3 Hybrid Persistence
Use RDB for the base image and AOF for recent changes. Fast reload + Reliability.

---

## 6. High Availability & Failover

### 6.1 Replication
*   **Master-Slave:** Every Master has 1+ Replicas.
*   **Async Replication:** Master acknowledges write to client *before* waiting for Replica.
    *   *Trade-off:* Small window of data loss if Master crashes immediately.

### 6.2 Failure Detection (Gossip Protocol)
*   Nodes exchange `PING`/`PONG` messages.
*   **PFAIL (Possible Fail):** Node A sends Ping to Node B. Timeout. Node A flags B as PFAIL.
*   **FAIL:** Node A gossips to C, D, E. If the majority also mark B as PFAIL, it becomes FAIL.

### 6.3 Automatic Failover
1.  Cluster detects Master B is DOWN.
2.  Replica B1 initiates an election.
3.  Masters A and C vote for B1 (based on replication offset freshness).
4.  B1 promotes itself to Master.
5.  Cluster Map updates; clients are redirected to new Master.

---

## 7. Scalability: Resharding

How do we add Node D?
1.  **Setup:** Start Node D (Empty).
2.  **Slot Migration:** Move slots from A, B, C to D.
    *   Migration is done **Slot by Slot**.
    *   It does **not** stop the world.
3.  **Migration Flow (Key Extraction):**
    *   Mark slot as `MIGRATING` on Source, `IMPORTING` on Target.
    *   Move keys atomically.
    *   If client asks Source for a key that moved: `-ASK 127.0.0.4:6379`. Client goes to Target for *just this query*.
    *   Once slot is empty on Source, update Cluster Map.

---

## 8. Eviction Policies (Memory Management)

When RAM is full, what happens?
*   **noeviction:** Return error on write.
*   **allkeys-lru:** Evict Least Recently Used key (any key).
*   **volatile-lru:** Evict LRU key *that has a TTL set*.
*   **allkeys-lfu:** Evict Least Frequently Used (better for non-temporal patterns).

**Approximated LRU:** Redis doesn't keep a perfect LRU list (too much memory). It samples 5 random keys and kills the oldest.

---

## 9. Consistency Model: The "Split Brain" scenarios

*   **Redis is not CP.** It does not guarantee strong consistency under partition.
*   **Scenario:** Network partition cuts Master A from Replicas and Majority.
    *   Clients connected to A continue writing (Data divergence).
    *   Major partition elects A2 as new master.
    *   When partition heals, A becomes a Replica of A2. **Writes to A are lost.**
    *   *Mitigation:* `min-replicas-to-write`. Master refuses writes if it can't see X replicas.

---

## 10. Summary

*   **Sharding:** Hash Slots (16k) allow flexible rebalancing.
*   **Performance:** Single-threaded event loop avoids Lock contention. (Multi-threaded I/O added in Redis 6.0).
*   **Availability:** Gossip protocol + Raft-like election for failover.
*   **Persistence:** AOF `everysec` is the industry standard balance.
