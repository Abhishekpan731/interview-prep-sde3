# 150 NoSQL (MongoDB & Cassandra) Interview Questions (SDE3)

## 1. NoSQL Fundamentals & The CAP Theorem (20 Questions)
1. What is **NoSQL**? In what scenarios is it preferred over a traditional RDBMS?
2. Explain the **CAP Theorem** (Consistency, Availability, Partition Tolerance). Why can you only pick two?
3. What is the difference between **BASE** (Basically Available, Soft state, Eventual consistency) and **ACID**?
4. Explain **Consistency Models**: Strong Consistency vs. Eventual Consistency vs. Causal Consistency.
5. What is the **PACELC Theorem** and how does it extend CAP?
6. Categorize the four types of NoSQL databases: **Key-Value**, **Document**, **Column-family**, and **Graph**.
7. In a **Network Partition** scenario, why is Partition Tolerance mandatory for distributed NoSQL systems?
8. Explain **Horizontal Scaling (Scaling Out)** vs. **Vertical Scaling (Scaling Up)**.
9. What is **Shared-Nothing Architecture**?
10. Explain **Data Denormalization**. Why is it a core principle in NoSQL?
11. What is the **N+1 Query Problem** in NoSQL context?
12. Explain **Polyglot Persistence**.
13. How do NoSQL databases handle **Schema Flexibility**?
14. What is the impact of **Secondary Indexes** on NoSQL performance?
15. Explain **Consistency Level** toggling at the request level.
16. What is **Read-Your-Writes** consistency?
17. Explain **Vector Clocks** for versioning in distributed NoSQL.
18. What is a **Conflict-Free Replicated Data Type (CRDT)**?
19. Explain **Quorum-based Consistency** (R + W > N).
20. When would you choose **Document DB (MongoDB)** over **Column-Family DB (Cassandra)**?

## 2. MongoDB Internals & Architecture (35 Questions)
21. What is **MongoDB**? Explain its **Document-oriented** model using **BSON**.
22. What is the difference between **JSON** and **BSON**?
23. Explain the **WiredTiger Storage Engine**. What are its key features (Checkpointing, Compression, Locking)?
24. How does MongoDB handle **Locking** (Global vs. Database vs. Collection level)?
25. Explain **Replica Sets**. What are Primary, Secondary, and Arbiter nodes?
26. How does the **Election Process** work in a Replica Set when a Primary fails?
27. What is the **Oplog (Operations Log)**? How is it used for replication?
28. Explain **Read Preference** (Primary, PrimaryPreferred, Secondary, etc.).
29. What is **Write Concern** (`w: 1`, `w: "majority"`, `j: true`)?
30. Explain **Sharding** in MongoDB. What are the components: **Shard**, **Mongos**, and **Config Servers**?
31. What is a **Shard Key**? Explain **Hashed Sharding** vs. **Ranged Sharding**.
32. What is the **Celebrity Problem (Hot Shard)** and how to pick a good shard key?
33. Explain **Chunk Splitting** and **Balancer** process.
34. What are **Jumbo Chunks**?
35. Explain **Capping Collections**.
36. How does MongoDB handle **Indexing**? (B-Tree).
37. Explain **Multikey Indexes** (for arrays).
38. What is a **TTL (Time to Live) Index**?
39. Explain **Text Search** and **Geospatial Indexes** in MongoDB.
40. What is the **Aggregation Framework**? Explain the pipeline stages ($match, $group, $lookup).
41. Explain **$lookup** (Join-like operation). What are its performance limitations?
42. How to use the **Explain Plan** in MongoDB to optimize queries?
43. What is **Covered Query** in MongoDB?
44. Explain **GridFS** for storing large files.
45. How does MongoDB handle **Atomic Operations** at the document level?
46. Explain **Multi-document Transactions** introduced in MongoDB 4.0+.
47. What is **Change Streams**?
48. Explain **Read Isolation** (Read Concern: "local", "majority", "snapshot").
49. How to monitor MongoDB performance? (mongostat, mongotop).
50. What is **Index Intersection**?
51. Explain **In-Memory Storage Engine** in MongoDB.
52. How to handle **Schema Design** (Relationship Modeling): **Embedding** vs. **Referencing**.
53. When to use **Manual Referencing** vs. **DBRefs**?
54. Explain **Partial Indexes**.
55. What is the impact of **Large Document Sizes (16MB limit)** on performance?

## 3. Cassandra Internals & Architecture (35 Questions)
56. What is **Apache Cassandra**? Explain its **Wide-column** peer-to-peer architecture.
57. Explain **Gossip Protocol**. How do nodes discover each other?
58. What is the **Ring Architecture** and **Consistent Hashing**?
59. Explain the **Partition Key** vs. **Clustering Key**. Why is the Partition Key the most important design choice?
60. What is a **Primary Key** in Cassandra (Partition Key + Clustering Columns)?
61. Explain **SSTables (Sorted String Tables)** and **Memtables**. Describe the **Write Path** in Cassandra.
62. Explain the **Read Path** in Cassandra. Why is it more complex than the write path?
63. What is a **Bloom Filter**? How does it speed up reads?
64. Explain **Compaction**. What is the difference between **SizeTieredCompactionStrategy (STCS)** and **LeveledCompactionStrategy (LCS)**?
65. What is **Read Repair** (Sync vs. Async)?
66. Explain **Anti-Entropy Node Repair** using **Merkle Trees**.
67. What is **Hinted Handoff**? How does it help in temporary node failures?
68. Explain **Tombstones**. Why are they problematic and how are they cleaned up (GC Grace Seconds)?
69. What is the **Commit Log**?
70. Explain **Replication Factor (RF)** and **Consistency Level (CL)**.
71. What is **QUORUM**? Calculate it for RF=3.
72. Explain **LOCAL_QUORUM** vs. **EACH_QUORUM** in a multi-DC setup.
73. What is **Snitch** (PropertyFileSnitch, GossipingPropertyFileSnitch)?
74. Explain **Lightweight Transactions (LWT)** using Paxos.
75. What is **Cassandra Query Language (CQL)**?
76. Why doesn't Cassandra support **Joins**? How do you model data to avoid them?
77. Explain **Query-Driven Data Modeling** in Cassandra.
78. What is **Denormalization** in Cassandra? (Creating one table per query).
79. What are **Secondary Indexes** in Cassandra? Why are they often discouraged?
80. Explain **SASI (SSTable Attached Secondary Index)**.
81. What is **Materialized Views** in Cassandra?
82. How does Cassandra handle **High Availability** across Data Centers (DC)?
83. Explain **VNodes (Virtual Nodes)**. What problems do they solve?
84. What is the **Partition Summary** and **Key Cache**?
85. Explain the **Write Amplification** and **Read Amplification** in Cassandra.
86. How to monitor Cassandra? (nodetool, JMX).
87. Explain **JVM Tuning** for Cassandra (G1GC settings).
88. What is **Speculative Retry**?
89. Explain **Batching** in Cassandra (Logged vs. Unlogged batches).
90. What is **Data Compression** in SSTables?

## 4. Scaling, Replication & High Availability (20 Questions)
91. Compare **MongoDB Sharding** vs. **Cassandra Partitioning**.
92. How do you handle **Database Migration** for millions of NoSQL records?
93. Explain **Hot Spot** detection in a distributed cluster.
94. How to perform **Zero-Downtime Scaling** (Adding nodes to a cluster)?
95. Discuss **Multi-Region Disaster Recovery** strategies in NoSQL.
96. Compare **Master-Slave (MongoDB)** vs. **Master-Master (Cassandra)**.
97. Explain **Cross-Region Replication** latency challenges.
98. What is **Data Locality** and why is it important in NoSQL?
99. How to handle **Clock Skew** in a geographically distributed NoSQL database?
100. Explain **Eventual Consistency Resolution** (Last Write Wins vs. Causal).
101. What is **Active-Active** vs. **Active-Passive** deployment?
102. Explain **Backup and Point-in-time Recovery** for NoSQL.
103. What is **Storage Mapping** in Cloud (EBS vs. Local SSD) for NoSQL?
104. How to perform a **Security Audit** on NoSQL clusters (RBAC, Encryption)?
105. Explain **Connection Overload** in MongoDB (the 65k limit).
106. How to use a **Database Proxy** with NoSQL?
107. Explain **Warm Standby** in NoSQL.
108. What is **Network Partitioning (Split Brain)** handling in NoSQL?
109. Discuss the role of **Zookeeper** or **Etcd** in some NoSQL architectures.
110. How to implementation **Rate Limiting** at the database layer?

## 5. Performance Tuning & Best Practices (20 Questions)
111. How to optimize **MongoDB Aggregation Pipeline**?
112. How to handle **Deep Pagination** in MongoDB?
113. Explain **Indexing Strategies** for high-write NoSQL workloads.
114. How to tune **Cassandra Compaction** for SSDs?
115. What is the impact of **Large Partitions** (>100MB) in Cassandra?
116. How to optimize **BSON Serialization** overhead?
117. What are the best practices for **Shard Key selection**?
118. How to handle **Massive Delete** operations in NoSQL?
119. Explain **Caching** strategies (Redis in front of NoSQL).
120. How to tune **Thread Pools** for database drivers?
121. What is **Query Timeout** management in NoSQL?
122. How to handle **Database Connections** in Serverless (Lambda) environments when using NoSQL?
123. Explain **Compression Algorithms** (Snappy, Zstd) impact on I/O.
124. How to identify **Slow Queries**?
125. What is **Profilers** usage in MongoDB?
126. Explain **Memory Mapping (Mmap)** vs. **Direct I/O**.
127. How to tune **Linux OS** for Cassandra (Swappiness, Read-ahead)?
128. What is the impact of **Data Model changes** on existing NoSQL data?
129. How to handle **Version Incompatibility** in NoSQL drivers?
130. Best practices for **Key Naming/Structuring**.

## 6. Advanced Topics & Modern Trends (20 Questions)
131. Explain **Vector Search** in NoSQL (e.g., MongoDB Vector Search).
132. How do NoSQL databases handle **Time Series Data**? (e.g., MongoDB Time Series collections).
133. What is **Distributed Ledger** vs. NoSQL?
134. Explain **Serverless NoSQL** (e.g., DynamoDB, CosmosDB, MongoDB Atlas Serverless).
135. What is **NewSQL** and how does it bridge the gap?
136. Explain **In-database Analytics**.
137. How to handle **Data Privacy and Sovereignty** in NoSQL?
138. Explain **Zero-knowledge Proofs** in NoSQL context.
139. What is **AI-driven Data Modeling**?
140. Discuss **Blockchain-based NoSQL** databases.
141. What is **JSON/Document support in SQL** vs. native NoSQL? (The Convergence).
142. Explain **Edge NoSQL** (databases running on mobile/IOT devices).
143. What is **Data Mesh** and NoSQL's role?
144. How to perform **Hybrid-Cloud NoSQL** deployment?
145. Explain **Cold-Hot Data Tiering**.
146. What is **Change Data Capture (CDC)** in NoSQL?
147. Discuss **Open Source vs. Managed (SaaS) NoSQL** trade-offs.
148. What is **GraphQL on top of NoSQL**?
149. How to handle **Compliance (HIPAA/GDPR)** in NoSQL architectures?
150. Future of Databases: Is **Schema-on-Read** or **Schema-on-Write** winning?

---
*Generated by Antigravity for SDE3 Interview Prep.*
