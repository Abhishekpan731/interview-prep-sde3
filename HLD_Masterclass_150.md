# 150 High-Level Design (HLD) Interview Questions (SDE3)

## 1. HLD Fundamentals & System Components (25 Questions)
1. Explain the **CAP Theorem**. Why is it often misunderstood, and how does it apply to modern distributed databases?
2. What is the **PACELC Theorem**? How does it extend the CAP theorem?
3. Explain **Scalability** (Vertical vs. Horizontal) and the trade-offs involved.
4. What is **Availability** (High Availability) and how do you calculate uptime (the "nines")?
5. Explain **Consistency Models**: Strong, Eventual, Causal, and Read-After-Write consistency.
6. What is **Latency vs. Throughput**? How can one be optimized without affecting the other?
7. Explain the **Load Balancer** types (L4 vs. L7). Where would you use each in a massive architecture?
8. What is **Consistent Hashing**? How does it solve the problem of re-sharding in a distributed cache?
9. Explain **Database Sharding**. What are the different sharding keys (Range, Hash, Directory) and their pros/cons?
10. What is **Database Replication** (Master-Slave, Multi-Master, Quorum-based)?
11. Explain the **Gossip Protocol**. How is it used for failure detection in systems like Cassandra?
12. What is a **Reverse Proxy**? How does it differ from a standard Proxy?
13. Explain **Content Delivery Networks (CDN)**. What are Push vs. Pull CDNs?
14. What is **Caching** at different levels (Client, CDN, Web Server, App Server, Database)?
15. Explain **Microservices vs. Monolith**. What are the "hidden costs" of microservices?
16. What is **Service Discovery** (Client-side vs. Server-side discovery)?
17. Explain the **API Gateway** pattern. What are its cross-cutting concerns?
18. What is **Message Queuing** and why is it used for asynchronous processing?
19. Explain **Event-Driven Architecture (EDA)**.
20. What is **Pub/Sub** mechanism?
21. Explain **Heartbeat** and **Lease** mechanisms in distributed systems.
22. What is a **Distributed Unique ID Generator** (e.g., Snowflake ID)? Why is it needed?
23. Explain **Backpressure** and how it prevents system collapse under heavy load.
24. What are **SLA, SLO, and SLI**?
25. Explain the **Fan-out** pattern in social media notification systems.

## 2. Distributed Data & Storage Design (25 Questions)
26. Compare **SQL vs. NoSQL**. In which scenarios would you strictly prefer one over the other?
27. Explain **NewSQL** (e.g., CockroachDB, Google Spanner). How do they achieve ACID across regions?
28. What is **Index-based Search** vs. **Full-Text Search** (Elasticsearch)?
29. Explain **LSM Trees** (Log-Structured Merge-Trees) vs. **B-Trees**. Why are LSM Trees better for write-heavy workloads?
30. What is a **Write-Ahead Log (WAL)**?
31. Explain **Leader Election** algorithms (Paxos, Raft, Zab).
32. What is **Quorum** in distributed systems (R + W > N)?
33. Explain **Two-Phase Commit (2PC)** vs. **Three-Phase Commit (3PC)**.
34. What is the **Saga Pattern** for distributed transactions? (Choreography vs. Orchestration).
35. Explain **CQRS** (Command Query Responsibility Segregation).
36. What is **Event Sourcing**? How does it differ from traditional CRUD?
37. Explain **Data Partitioning** strategies for 100TB+ datasets.
38. What is **Secondary Indexing** in NoSQL?
39. Explain **Object Storage** (S3) vs. **Block Storage** (EBS) vs. **File Storage** (EFS).
40. How to handle **Hot Keys** ( Celebrity problem) in a distributed cache or database?
41. What is **Read-Your-Writes** consistency and how to implement it?
42. Explain **Vector Clocks** and **Lamport Timestamps**.
43. What is **Bloom Filter**? How to use it to reduce expensive database lookups?
44. Explain **Data Denormalization**. When is it a good idea in HLD?
45. What is **Conflict-Free Replicated Data Types (CRDTs)**?
46. Explain **In-Memory Databases** (Redis) and their persistence modes.
47. How to design for **Data Corruption** and **Data Integrity** in large systems?
48. What is **Cold vs. Warm vs. Hot** storage?
49. Explain **Change Data Capture (CDC)**.
50. How to design a **Data Lake** vs. **Data Warehouse**?

## 3. Designing Popular Systems (Case Studies) (30 Questions)
51. Design a **URL Shortener** (e.g., Bitly). Focus on: ID generation, redirection latency, and storage.
52. Design **Twitter** (Timeline, Tweet, Follow). Focus on: Fan-out, Pull vs. Push for feed generation.
53. Design **Facebook Messenger/WhatsApp**. Focus on: Websockets, Message delivery status, and Offline messages.
54. Design **Netflix/YouTube**. Focus on: Video transcoding, CDN distribution, and Adaptive bitrate streaming (HLS).
55. Design **Uber/Lyft**. Focus on: Geospatial indexing (GeoHash/QuadTree), Driver matching, and Location tracking.
56. Design **Instagram**. Focus on: Image upload, Feed generation, and Storage of billions of media files.
57. Design **Dropbox/Google Drive**. Focus on: Block-level syncing, Data deduplication, and Conflict resolution.
58. Design **Amazon/E-commerce**. Focus on: Inventory management, Shopping cart, and Payment processing.
59. Design **Ticketmaster** (Flash Sales). Focus on: High concurrency, Locking mechanisms, and Queueing.
60. Design a **Web Crawler** (Scaling to billions of pages). Focus on: Politeness, Deduplication, and Frontier management.
61. Design a **Notification Service** (Email, SMS, Push). Focus on: Priority, Retries, and Template management.
62. Design a **Typeahead Suggestion (Autocomplete)**. Focus on: Trie data structure, Caching, and Frequency updates.
63. Design a **Metrics Monitoring System** (e.g., Prometheus). Focus on: Time-series DB, Data retention, and Alerting.
64. Design a **Logging System** (e.g., ELK stack). Focus on: Log collection, Aggregation, and Search latency.
65. Design a **Rate Limiter**. Focus on: Different algorithms (Token Bucket, Leaky Bucket) and Distributed implementation.
66. Design a **Distributed Cache** (e.g., Redis Cluster). Focus on: Data sharding, Persistence, and High Availability.
67. Design **Google Maps/Proximity Service** (Yelp). Focus on: Spatial search and Map rendering.
68. Design an **Ad Click Aggregator**. Focus on: High throughput, Windowing, and Accuracy.
69. Design a **Digital Wallet** (e.g., PayPal/Stripe). Focus on: ACID transactions, Reconciliation, and Fraud detection.
70. Design a **News Feed Aggregator**. Focus on: Ranking algorithms and Personalization.
71. Design a **Ride-sharing Matching Engine**.
72. Design a **Multi-player Game Backend** (e.g., Tic-Tac-Toe or Chess).
73. Design a **Voting System** (High concurrency like Eurovision).
74. Design an **Online Judge** (e.g., LeetCode/Codeforces).
75. Design a **Stocks/Crypto Exchange** (Matching Engine). Focus on: Low latency and Sequential processing.
76. Design a **Health Monitoring Dashboard** for 1 million IoT devices.
77. Design a **Content Delivery Network (CDN)** yourself.
78. Design a **Collaborative Document Editor** (e.g., Google Docs). Focus on: OT (Operational Transformation) or CRDTs.
79. Design an **Anti-Spam/Abuse System**.
80. Design a **Distributed Job Scheduler**.

## 4. Resilience, Security & Performance (30 Questions)
81. Explain the **Circuit Breaker** pattern. What are the states (Open, Closed, Half-Open)?
82. What is the **Retry with Exponential Backoff** strategy?
83. Explain **Bulkhead Isolation**.
84. How to perform a **Graceful Degradation** of a system under heavy load?
85. What is the **Sidecar Pattern**? How is it used in Service Mesh?
86. Explain **Distributed Tracing**. Why is it crucial for microservices?
87. How to handle **Cascading Failures** in a distributed system?
88. What is **Throttling**? How it differs from Rate Limiting?
89. Explain **Zero-Downtime Deployment** (Blue-Green vs. Canary vs. Rolling).
90. How to design for **Disaster Recovery** (RTO and RPO goals)?
91. What is **Multi-Region Deployment**? Active-Active vs. Active-Passive.
92. Explain **Chaos Engineering**. Why do companies like Netflix use it?
93. How to secure **Service-to-Service Communication**? (mTLS).
94. Explain **HSTS, CSP, and CORS** in the context of system security.
95. What is **Data Encryption at Rest and in Transit**?
96. How to handle **Database Credentials and Secrets** in HLD?
97. Explain **Rate Limiting vs. WAF (Web Application Firewall)**.
98. How to prevent **DDoS Attacks** in your design?
99. Explain **Database Connection Pooling**. Why can it be a bottleneck?
100. How to optimize **Database Migration** for 1 Billion+ rows without downtime?
101. What is **TCP/IP Tuning** for low latency systems?
102. Explain **HTTP/2 vs. HTTP/1.1 vs. gRPC**.
103. What are **Websockets** and when to use them over REST/Polling?
104. How to optimize **Frontend Assets** (Bundling, Minification, Pre-fetching)?
105. Explain the importance of **Log Rotation and Retention Policies**.
106. How to handle **Large File Uploads** in a distributed system?
107. Explain **Zero-Copy** I/O.
108. How to design a **Stateless** application?
109. Explain **Cloud-Native Design** principles.
110. How to minimize **Communication Overhead** in microservices?

## 5. Architectural Patterns & Decision Making (25 Questions)
111. When would you choose **Synchronous** vs. **Asynchronous** communication?
112. Explain **Orchestration vs. Choreography** in microservices interaction.
113. What is the **Strangler Fig Pattern**? (Migration from Monolith to Microservices).
114. Explain **Hexagonal Architecture** (Ports and Adapters).
115. What is **Clean Architecture**?
116. Explain **Domain-Driven Design (DDD)** concepts: Bounded Context, Aggregates, Entities, and Value Objects.
117. How to design a **Multi-Tenant SaaS** architecture? (Shared DB vs. Shared Schema vs. Isolated DB).
118. What is **Serverless Architecture**? When is it NOT a good choice?
119. Explain **BFF (Backend For Frontend)** pattern.
120. What is **Micro-Frontends**?
121. Explain **Eventual Consistency vs. Strong Consistency** trade-offs in HLD.
122. How to handle **Date/Time/Timezone** issues in a global distributed system?
123. What is **Read Amplification** and **Write Amplification**?
124. Explain **Shared Nothing Architecture**.
125. What is **Infrastructure as Code (IaC)**?
126. How to choose a **Database** for a system? (Matrix of requirements: Consistency, Read Load, Write Load, Latency).
127. Explain **Polyglot Persistence**.
128. What is **Data Normalization** vs. **Denormalization** trade-off?
129. How to design a **Plug-and-Play Architecture**?
130. What is **Service Mesh** and why do you need it?
131. Explain **Distributed Locking** without external dependencies.
132. What is **Cache Stampede**?
133. How to handle **Version Incompatibility** in distributed systems?
134. Explain **Schema Evolution** in NoSQL and message formats (Avro/Protobuf).
135. Summary: What are the **Top 5 Anti-patterns** in HLD you've seen in your experience?

## 6. Advanced/Modern Design (15 Questions)
136. Explain **Edge Computing** and its design benefits.
137. What is **Web3** from a system design perspective?
138. How to design for **Observability** (Logs, Metrics, Traces)?
139. Explain **Real-time Data Processing** (Apache Flink/Spark Streaming).
140. How to design a **Privacy-first System** (GDPR/CCPA compliance)?
141. What is **Cold Start** in Lambda/Functions? How to design around it?
142. Explain **Sidecar injection** in Kubernetes for service mesh.
143. How to design for **1 Billion active users**? (Horizontal scaling, Sharding, Global Load Balancing).
144. Explain **Database Mesh**.
145. What is **FinOps** in the context of Cloud Design?
146. How to design an **Auto-scaling** engine?
147. Explain **Distributed Machine Learning Infrastructure** design.
148. What is **Zero-Trust Security** at the design level?
149. Explain **Network Partition** (Split Brain) handling in HLD.
150. Future of Architecture: Discussion on **Sustainability and Green Computing** in system design.

---
*Generated by Antigravity for SDE3 Interview Prep.*
