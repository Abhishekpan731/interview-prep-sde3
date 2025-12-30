# 150 SQL Internals (PostgreSQL & MySQL) Interview Questions (SDE3)

## 1. Relational Database Architecture (20 Questions)
1. Explain the high-level architecture of **MySQL**. What is the role of the Server Layer vs. the Storage Engine Layer?
2. Explain the high-level architecture of **PostgreSQL**. How does its process-based model differ from MySQL's thread-based model?
3. What is a **Storage Engine**? Why does MySQL support multiple engines (InnoDB, MyISAM) while PostgreSQL uses a unified storage engine?
4. Describe the **InnoDB Architecture**. What are the Buffer Pool, Log Buffer, and Tablespaces?
5. Describe the **PostgreSQL Architecture**. What are the Postmaster, Backend processes, and Shared Buffers?
6. What is the **Buffer Pool**? How does the LRU (Least Recently Used) algorithm manage memory?
7. Explain the **Write-Ahead Log (WAL)**. Why is it essential for Durability?
8. What is the difference between **Redo Log** and **Undo Log** in MySQL?
9. Explain **Binary Log (Binlog)** in MySQL and how it differs from the Redo Log.
10. What is **Checkpointing**? Why is it needed to prevent long recovery times after a crash?
11. Explain the **Doublewrite Buffer** in InnoDB. Why is it used for data integrity?
12. What are **System Catalogs** or **Information Schema**?
13. Explain **Tablespace** management in PostgreSQL vs. MySQL.
14. What is the role of the **Background Writer** and **Checkpointer** in PostgreSQL?
15. How does MySQL handle **Connection Management** and Thread Pooling?
16. Explain **Autovacuum** in PostgreSQL. Why is it necessary?
17. What is **Bloat** in PostgreSQL? How does it affect performance?
18. Explain **TOAST (The Oversized-Attribute Storage Technique)** in PostgreSQL.
19. What is **Fill Factor**? How does it impact page splitting?
20. Explain the **On-disk representation** of a row in PostgreSQL vs. MySQL.

## 2. Indexing Internals & Data Structures (25 Questions)
21. Explain **B-Tree** vs. **B+ Tree** indexes. Why are B+ Trees preferred for disk-based databases?
22. What is a **Clustered Index**? Why can a table have only one clustered index?
23. What is a **Non-Clustered (Secondary) Index**? How does it point to the actual data?
24. Explain **Primary Key** vs. **Unique Key** implementation.
25. What is a **Composite Index** (Multi-column)? Explain the **Leftmost Prefix Rule**.
26. What is an **Index-Only Scan/Covering Index**? How does it improve performance?
27. Explain **Hash Indexes**. Why are they limited to equality comparisons?
28. What are **GIN (Generalized Inverted Index)** and **GiST (Generalized Search Tree)** indexes in PostgreSQL?
29. Explain **BRIN (Block Range Index)** in PostgreSQL. When is it better than a B-Tree?
30. What is a **Partial Index**? Give a use case.
31. What is an **Expression (Function) Index**?
32. Explain **LSM-Trees** (Log-Structured Merge-Trees). Why are they used in some NoSQL-like engines but not typically for primary SQL storage?
33. What is **Index Cardinality**? How does it affect the query optimizer?
34. Explain **Index Selectivity**.
35. What is **Index Fragmentation** and how to resolve it?
36. Describe the **Structure of a B-Tree Page/Node** (Header, Pointers, Keys, Data).
37. Explain **Sparse vs. Dense Indexes**.
38. What are **Bitmap Indexes**? Why are they rare in standard MySQL/PostgreSQL?
39. How does **Unique Constraint** checking work internally using an index?
40. Explain **Index Merging**.
41. What is **Descending Index** support?
42. How does the **Optimizer** choose whether to use an index or a full table scan?
43. What is **Full-Text Indexing** in SQL?
44. Explain **Spatial Indexes (R-Tree)**.
45. How does the database handle **Index Updates** during a write operation?

## 3. Transactions, ACID & MVCC (25 Questions)
46. Define **ACID** properties in detail.
47. How is **Atomicity** guaranteed using logs?
48. What is **Consistency** in the context of ACID?
49. Explain **Isolation** levels: Read Uncommitted, Read Committed, Repeatable Read, and Serializable.
50. What is **Dirty Read**, **Non-Repeatable Read**, and **Phantom Read**?
51. How does **InnoDB** implement **Repeatable Read** to prevent Phantom Reads?
52. Explain **MVCC (Multi-Version Concurrency Control)**. How does a database maintain multiple versions of a row?
53. What is a **Transaction ID (XID)** and how is it used in visibility checks?
54. Explain **Snapshot Isolation**.
55. How does PostgreSQL handle **Visibility Checks** (xmin, xmax)?
56. What is the **Transaction Log (WAL/Redo)** role in crash recovery?
57. Explain the **Two-Phase Commit (2PC)** for distributed transactions.
58. What is a **Savepoint**?
59. Explain the **Serializable Snapshot Isolation (SSI)** in PostgreSQL.
60. What is the impact of **Long-Running Transactions** on MVCC and log files?
61. Explain **Locking** vs. **Versioning**.
62. What is a **Write-Ahead Log (WAL)** segment?
63. Explain **Transaction Wraparound** in PostgreSQL.
64. What is the **Commit Log (CLOG)** in PostgreSQL?
65. Explain **Autocommit** mode.
66. How to implementation **Pessimistic Locking** (`SELECT ... FOR UPDATE`)?
67. What is **Optimistic Locking** using a version column?
68. Explain **Implicit vs. Explicit Transactions**.
69. How does the database handle **Deadlocks** in transactions?
70. What are **Inter-transaction coordination** mechanisms?

## 4. Query Optimization & Execution (25 Questions)
71. Describe the **Query Life Cycle**: Parser -> Analyzer -> Rewriter -> Optimizer -> Executor.
72. What is the **Query Execution Plan**? How do you read an `EXPLAIN` output?
73. Compare **Index Scan** vs. **Index Only Scan** vs. **Bitmap Index Scan**.
74. Explain **Sequential Scan (Full Table Scan)**. When is it faster than an index scan?
75. What are the different **Join Algorithms**: Nested Loop Join, Hash Join, and Merge Join?
76. Explain **Hash Join** internals. Why is it efficient for large datasets?
77. Explain **Merge Join**. When is it preferred?
78. What are **Database Statistics**? How does `ANALYZE` affect the optimizer?
79. Explain **Cost-based Optimizer (CBO)** vs. **Rule-based Optimizer (RBO)**.
80. What are **Histograms** in query optimization?
81. Explain **Join Ordering**. Why is the order of tables in a join critical?
82. What is a **Correlated Subquery**? Why are they often slow?
83. Explain **Common Table Expressions (CTEs)** and their performance impact.
84. What are **Materialized Views**? How do they differ from standard Views?
85. Explain **Window Functions** execution.
86. What is **Query Parallelism**? How does PostgreSQL use multiple cores for a single query?
87. Explain **Prepared Statements**. How do they improve performance and security?
88. What is the **Plan Cache**?
89. How to optimize a query with **OR** conditions?
90. Explain **Limit and Offset** performance pitfalls.
91. What is **Sort Merge** vs. **QuickSort** in database memory?
92. Explain **External Sorting** for datasets larger than RAM.
93. What is **Bloom Filter** usage in Join optimization?
94. Explain **Adaptive Query Execution**.
95. How to handle **Function calls in WHERE clauses** (Index suppression)?

## 5. High Availability, Replication & Scaling (20 Questions)
96. Explain **Master-Slave (Leader-Follower) Replication**.
97. What is **Synchronous vs. Asynchronous vs. Semi-synchronous** replication?
98. Explain **Logical Replication** vs. **Physical (Streaming) Replication**.
99. What is **Write-Ahead Log (WAL) Shipping**?
100. Explain **Read Replicas** and how they help in horizontal scaling.
101. What is **Replication Lag**? How do you monitor and mitigate it?
102. Explain **Multi-Master Replication**. What are the conflict resolution challenges?
103. What is **Cascading Replication**?
104. Explain **Database Sharding**. What is a **Sharding Key**?
105. Compare **Horizontal Sharding** vs. **Vertical Partitioning**.
106. What is **Table Partitioning** (Range, List, Hash)? How does **Partition Pruning** work?
107. Explain **Citrus / Citus** (PostgreSQL extension for sharding).
108. What is **Vitess** (Scaling MySQL)?
109. Explain **Always On / Patroni** for PostgreSQL High Availability.
110. How to handle **Failover** and **Promoting a Follower**?
111. What is the **Quorum** in a database cluster?
112. Explain **PostgreSQL BDR (Bi-Directional Replication)**.
113. What are **Global ID** generation strategies in a sharded environment?
114. Explain **Data Locality** in sharded databases.
115. How to perform **Online Schema Changes** in large clusters?

## 6. Locking, Contention & Tuning (20 Questions)
116. Explain **Row-Level Locking** vs. **Page-Level Locking** vs. **Table-Level Locking**.
117. What are **Shared (S)** locks and **Exclusive (X)** locks?
118. Explain **Intention Locks (IS, IX)**. Why are they needed?
119. What is **Metadata Locking (MDL)** in MySQL?
120. Explain **Predicate Locking**.
121. What is a **Deadlock**? How does the database detect and resolve it?
122. Explain **Lock Contention**. How to identify it?
123. What are **Spinlocks** vs. **Mutexes** in database internals?
124. Explain **Latches** vs. **Locks**.
125. What is the **InnoDB Buffer Pool Hit Ratio**?
126. How to tune **MySQL `innodb_buffer_pool_size`** and **PostgreSQL `shared_buffers`**?
127. What is **Work Mem** in PostgreSQL? How does it affect joins and sorts?
128. Explain **Maintenance Work Mem**.
129. How to tune **WAL/Redo Log settings** for high-write workloads?
130. What is **Connection Overload** and how do **Connection Poolers** (PGBouncer) help?
131. Explain **Huge Pages** in Linux and their impact on database performance.
132. How to monitor **Database I/O Wait**?
133. What is **Query Profiling**?
134. Explain the impact of **Virtualization/Containers** on database performance.
135. How to scale a database for **Writes** (beyond Sharding)?

## 7. Advanced Topics & Modern Trends (15 Questions)
136. Explain **NewSQL** (Google Spanner, CockroachDB). How do they handle ACID globally?
137. What are **Vector Databases** in the context of SQL (e.g., pgvector)?
138. Explain **JSONB** in PostgreSQL. How does it provide NoSQL capabilities?
139. What is **Logical Decoding**?
140. Explain **PostgreSQL Extensions Architecture**.
141. What is **Serverless SQL** (e.g., Aurora Serverless)?
142. Explain **Storage-Compute Separation** in modern cloud databases.
143. What is **Columnar Storage** (e.g., ClickHouse, DuckDB)? How does it differ from Row-based?
144. Explain **Hybrid Transactional/Analytical Processing (HTAP)**.
145. What is **Database Proxy** (e.g., ProxySQL, MaxScale)?
146. How to handle **Database Compliance and Security (GDPR/PII)**?
147. Explain **Data Masking** and **Transparent Data Encryption (TDE)**.
148. What is **Zero-Downtime Migration**?
149. Explain **Point-In-Time Recovery (PITR)**.
150. Future of Databases: Discussion on **AI-driven tuning** and **Auto-indexing**.

---
*Generated by Antigravity for SDE3 Interview Prep.*
