# 150 Redis Interview Questions (SDE3)

## 1. Redis Fundamentals & Data Structures (25 Questions)
1. What is Redis and why is it categorized as a "Data Structures Server" rather than just a "Cache"?
2. Explain the core data types: **Strings, Lists, Sets, Hashes, and Sorted Sets**.
3. What is a **Sorted Set (ZSET)**? Explain the internal implementation using **Skip Lists** and **Hash Maps**.
4. How do **Bitmaps** work in Redis? Provide a real-world use case (e.g., tracking daily active users).
5. What is **HyperLogLog**? How does it achieve cardinality estimation with very low memory?
6. Explain **Geospatial** indexes in Redis.
7. What are **Redis Streams**? How do they differ from Kafka or Redis Pub/Sub?
8. What is the maximum size of a String, List, and Hash in Redis?
9. Explain the **LEDR (Log-structured, Event-driven, Distributed, RAM)** nature of Redis.
10. How does Redis handle **Expiration**? Explain Passive vs. Active expiration.
11. What is the difference between `EXPIRE` and `PEXPIRE`?
12. How does the **TTL (Time to Live)** affect memory management?
13. Explain **Redis Modules**. Name a few popular ones (e.g., RedisSearch, RedisJSON).
14. What are **Atomic Operations** in Redis?
15. Explain **Redis Transactions** (`MULTI`, `EXEC`, `DISCARD`, `WATCH`).
16. How does the `WATCH` command facilitate **Optimistic Locking**?
17. What is the difference between Redis and Memcached?
18. Explain **Pipelining** in Redis. How does it improve performance?
19. What is the role of the **Client Buffer**?
20. How to store complex JSON objects in Redis? (String vs. Hash vs. RedisJSON).
21. What are **Large Keys** and why are they dangerous for Redis performance?
22. Explain **Keyspace Notifications**.
23. What is the use of `SCAN` vs. `KEYS`? Why is `KEYS *` banned in production?
24. Explain the **In-place updates** feature in Redis.
25. How does Redis handle **Integer Encoding** for memory optimization?

## 2. Redis Internals & Concurrency (20 Questions)
26. Why is Redis **Single-threaded** (mostly)? How does it achieve high performance despite this?
27. Explain the **Event Loop** architecture in Redis.
28. What are the tasks that Redis performs **Multi-threaded** in recent versions (6.0+)? (I/O threads).
29. Explain the difference between **I/O Multiplexing** and standard blocking I/O.
30. What is **Epoll** and how does it relate to Redis?
31. How does Redis manage **Memory Allocation**? (Jemalloc vs. Libc).
32. What is **Memory Fragmentation**? How do you monitor and resolve it in Redis?
33. Explain the **Active Defrag** feature.
34. How does Redis handle **Copy-on-Write (CoW)** during background saves?
35. What is the impact of **Huge Pages** (Transparent Huge Pages) on Redis performance?
36. Deep dive into the **ZipList** and **QuickList** internal data structures.
37. What are **IntSets**?
38. Explain **Hashtable Rehashing** in Redis. How is it "Incremental"?
39. How does Redis handle **Object Sharing**?
40. What is the **Redis Protocol (RESP)**?
41. How does Redis handle **Context Switching** overhead?
42. Explain the **Client Object** structure in Redis source code.
43. What is the purpose of the `redis.conf` parameter `maxclients`?
44. How does Redis handle **Slow Logs**?
45. Explain the **Benchmark** tool provided with Redis.

## 3. Persistence: RDB & AOF (20 Questions)
46. Explain **RDB (Redis Database)** persistence. How does `BGSAVE` work?
47. Explain **AOF (Append Only File)** persistence.
48. Compare **RDB vs. AOF** in terms of data integrity, startup speed, and performance.
49. What is the **AOF Rewrite** (BGREWRITEAOF) process?
50. Explain the `fsync` policies: `always`, `everysec`, and `no`.
51. What is the **Hybrid Persistence** model (RDB-AOF mix) in Redis 4.0+?
52. How does Redis handle **Partial RDB** or **Corrupted AOF** files?
53. What is the performance impact of `fork()` during persistence?
54. How to perform a **Zero-downtime Backup** of a Redis instance?
55. Can you run Redis without any persistence? What are the use cases?
56. Explain the `SAVE` command vs. `BGSAVE`.
57. What is the impact of **Disk I/O Latency** on Redis while AOF is enabled?
58. Explain the **AOF Preamble**.
59. How does Redis decide when to trigger an automatic AOF rewrite?
60. What is the **RDB checksum**?
61. How to recover data if the persistence file is lost?
62. Explain the relationship between Persistence and **Replication**.
63. What is the `no-appendfsync-on-rewrite` setting?
64. How does Redis handle **Write Amplification** in AOF?
65. Discuss the trade-offs of using **Diskless Replication**.

## 4. High Availability: Replication & Sentinel (25 Questions)
66. How does **Master-Slave (Leader-Follower) Replication** work in Redis?
67. Explain **Partial Resynchronization (PSYNC)**. What are Repl-ID and Offset?
68. What is the **Replication Backlog Buffer**?
69. Explain **Chained Replication** (Sub-replicas).
70. What are **Read-only Replicas**?
71. How to handle **Stale Reads** from replicas?
72. What is **Redis Sentinel**? What are its primary functions (Monitoring, Notification, Failover, Configuration Provider)?
73. How does Sentinel perform **Automatic Failover**?
74. Explain the **Quorum** in Sentinel.
75. What is the **Sentinel Leader Election** (Raft-like protocol)?
76. How does a client discover the current Master in a Sentinel-managed setup?
77. Explain **Split Brain** scenario in Redis Sentinel. How to mitigate it using `min-replicas-to-write`?
78. What is the **Subjective Down (SDOWN)** vs. **Objective Down (ODOWN)**?
79. How to perform a **Manual Failover**?
80. What is the impact of **Network Partition** on a Sentinel cluster?
81. Explain **Epoch** in Sentinel.
82. How many Sentinel instances are recommended for a production cluster?
83. What is the **TILT Mode** in Sentinel?
84. How to secure Sentinel communication?
85. Explain the **Slave Priority** setting.
86. What happens when a dead Master comes back online in a Sentinel setup?
87. How does Sentinel handle **Replicas Promotion**?
88. What is the `down-after-milliseconds` setting?
89. Explain **Pub/Sub in Sentinel** for discovery.
90. Discuss the limitations of Redis Sentinel.

## 5. Scalability: Redis Cluster (20 Questions)
91. What is **Redis Cluster**? How does it differ from Sentinel?
92. Explain **Hash Slots**. Why exactly 16384 slots?
93. How is a Key mapped to a Hash Slot?
94. What is **Hash Tags** (`{tag}key`) and why are they important for multi-key operations?
95. Explain **Cluster Sharding**.
96. How does **Gossip Protocol** work in a Redis Cluster?
97. What is a **MOVED** redirection vs. an **ASK** redirection?
98. How to perform **Resharding (Slot Migration)** without downtime?
99. Explain **Cluster Failover**. How is a new master elected in a cluster?
100. What is **Client-side Side Loading** (Cluster-aware clients)?
101. Can a Redis Cluster have multiple replicas for each shard?
102. Explain **Replica Migration** in Redis Cluster.
103. What are the limitations of multi-key operations (like `MSET`, `SUNION`) in a cluster?
104. How to handle **Large Clusters** (>100 nodes)?
105. Explain the **Cluster Bus** and its port (usually 10000 + redis port).
106. What is the `cluster-node-timeout`?
107. How does a cluster handle **Partial Failures**?
108. Explain **Slave-only nodes** in Cluster mode.
109. What happens if more than half of the masters in a cluster go down?
110. Compare **Client-side Partitioning** vs. **Proxy-based (Codis/Twemproxy)** vs. **Redis Cluster**.

## 6. Advanced Use Cases & Patterns (20 Questions)
111. How to implement a **Distributed Lock** in Redis? Explain **Redlock** algorithm.
112. Why is a simple `SETNX` not enough for a production-grade distributed lock?
113. How to implement a **Rate Limiter**? (Fixed Window, Sliding Window, Token Bucket).
114. How to implement a **Task Queue** using Redis? (List vs. Stream).
115. Explain **Pub/Sub**. What are its limitations (At-most-once)?
116. How to use **Sorted Sets for Leaderboards**?
117. How to implement **Autocomplete/Search** using Redis?
118. Explain **Session Management** using Redis in a distributed web app.
119. How to implement **Idempotency** using Redis?
120. Explain **Redis LUA Scripting**. Why are LuA scripts atomic?
121. What is the impact of a **Long-running LUA script** on Redis?
122. How to handle **Cache Penentration, Cache Breakdown, and Cache Avalanche**?
123. What is **Cache Warming**?
124. Explain **Bloom Filters** and their usage in preventing cache penetration.
125. How to use Redis for **Real-time Analytics**?
126. Explain **Side-caching (Lazy Loading)** vs. **Write-through** vs. **Write-behind**.
127. How to manage **Cache Invalidation** strategies?
128. What is **Multi-level Caching**?
129. How to use **Redis Bitmaps for Retention Analysis**?
130. Explain **Redis Streams Consumer Groups**. How do they compare to Kafka Consumer Groups?

## 7. Performance Tuning & Best Practices (20 Questions)
131. What are the top 5 **Redis Metrics** to monitor in production?
132. Explain **Eviction Policies** (`noeviction`, `allkeys-lru`, `volatile-lru`, `allkeys-lfu`, etc.).
133. What is the difference between **LRU (Least Recently Used)** and **LFU (Least Frequently Used)**?
134. How does Redis implement **Approximate LRU**?
135. What is the **Maxmemory** setting? What happens when it's reached?
136. How to diagnose **High Latency** in Redis?
137. Explain the **Latency Monitor** and **Intrinsic Latency**.
138. What is the impact of **Swapping** on Redis?
139. How to find and fix **Big Keys** (`--bigkeys` command)?
140. How to find **Hot Keys**?
141. Explain **TCP Backlog** and its tuning for Redis.
142. What is the **Slow Log** depth and threshold?
143. How to use **Redis-cli --stat** and **--latency**?
144. Performance trade-offs of **Encryption (TLS)** in Redis.
145. How to optimize **Memory Usage** (using Hashes for small object sets)?
146. Discuss the impact of **Pipeline Size** on throughput.
147. How to handle **Redis Benchmarking** correctly?
148. Best practices for **Naming Keys**.
149. How to secure Redis? (Password, renaming dangerous commands, restricted networking).
150. Future of Redis: Discussion on **Redis 7.0+ features** (Functions, Sharded Pub/Sub).

---
*Generated by Antigravity for SDE3 Interview Prep.*
