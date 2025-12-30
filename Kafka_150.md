# 150 Apache Kafka Interview Questions (SDE3)

## 1. Kafka Fundamentals & Architecture (25 Questions)
1. What is Apache Kafka and how does it differ from traditional message brokers like RabbitMQ or ActiveMQ?
2. Explain the fundamental architectural components: Producer, Consumer, Broker, Cluster, Topic, Partition, and Offset.
3. What is the role of **Zookeeper** in Kafka (legacy) vs. the new **KRaft (KIP-500)** mode?
4. Explain the concept of **Partitions**. Why are they the unit of parallelism and scalability in Kafka?
5. How does Kafka achieve high throughput for writes? (Sequential I/O, Page Cache, Zero Copy).
6. What is the role of the **Controller** broker in a cluster?
7. How does Kafka handle partition leader election?
8. Explain the concept of **Replication Factor**. What are the trade-offs of setting it high?
9. What are **ISR (In-Sync Replicas)**?
10. What is a "Committed Message" in Kafka?
11. Explain **Log Segments**. How are logs physically stored on disk?
12. What is the **Index file** and **Time Index file** in a log segment?
13. Describe the **End-to-End Latency** in a Kafka pipeline. What factors influence it?
14. How does Kafka ensure **Message Ordering**? Within a partition vs. across the topic.
15. What are **Bootstrap Servers**?
16. Explain the **Kafka Protocol**. Is it binary or text-based?
17. How does Kafka manage **Metadata**?
18. What is the difference between a **Topic** and a **Partition** logical vs. physical representation?
19. Describe the **Controller's responsibility** during a broker failure.
20. Explain **Preferred Leader Election**.
21. What is **Unclean Leader Election**? When would you enable it?
22. How does Kafka handle **Multi-tenancy**? (Quotas, Namespaces).
23. Explain the concept of **Compact Topics** (Log Compaction). When would you use it?
24. What is the purpose of the `__consumer_offsets` topic?
25. Describe the internal structure of a Kafka record (Key, Value, Timestamp, Headers).

## 2. Producer Internals (20 Questions)
26. Describe the internal workflow of the **Kafka Producer (`send()` method)**.
27. Explain the role of the **Serializer** and **Partitioner** in the Producer.
28. What is the **RecordAccumulator**? How does it help in batching?
29. Explain `batch.size` and `linger.ms`. How do they affect throughput and latency?
30. What is the purpose of `acks=0`, `acks=1`, and `acks=all` (or `-1`)?
31. How does the Producer handle **Retries**? What is `retries` vs. `delivery.timeout.ms`?
32. What is the **Idempotent Producer**? How does it prevent duplicate messages during retries?
33. Explain the role of **Producer ID (PID)** and **Sequence Number**.
34. What is `max.in.flight.requests.per.connection`? How does it affect ordering and throughput?
35. Explain **Message Compression** (Gzip, Snappy, Lz4, Zstd). Where does it happen?
36. What is the **Custom Partitioner**? Provide a use case where you would implement one.
37. How does the producer find the leader of a partition? (Metadata caching).
38. What is `buffer.memory` and what happens when the producer fills this buffer?
39. Explain the impact of **Batching** on CPU vs. Network utilization.
40. How to handle **Serialization errors** in the producer?
41. What is the `max.request.size`?
42. How does the producer handle **Network partitions**?
43. Explain the `request.timeout.ms`.
44. How do you implement a **Sync vs. Async** producer send?
45. What is the **Producer Interceptor**?

## 3. Consumer Internals & Group Management (25 Questions)
46. Explain the **Consumer Group** concept. How does it enable load balancing?
47. What is a **Group Coordinator** and how is it selected?
48. What is a **Consumer Coordinator**?
49. Describe the **Consumer Rebalance** process. What triggers it?
50. What is **Incremental Cooperative Rebalancing**? How is it better than "Eager" rebalancing?
51. Explain **Static Group Membership**. Why is it useful in Kubernetes/StatefulSet environments?
52. How does a consumer discover which partitions to read from?
53. What is the **Offset Commit** process? Automatic vs. Manual.
54. Difference between `commitSync()` and `commitAsync()`.
55. What is the **Consumer Heartbeat**? Explain `heartbeat.interval.ms` and `session.timeout.ms`.
56. What is `max.poll.interval.ms`? What happens if the consumer logic takes longer than this?
57. Explain `fetch.min.bytes` and `fetch.max.wait.ms`.
58. What is the **Consumer Offset Reset Policy** (`auto.offset.reset`)?
59. How to achieve **Exactly-once consumption** (conceptually)?
60. What is **Consumer Lag**? How do you monitor and resolve it?
61. Explain the **Partition Assignment Strategy** (Range, RoundRobin, Sticky, CooperativeSticky).
62. How to handle **Poison Pill** messages in the consumer?
63. Explain **Parallel Consumer** (processing messages in parallel from the same partition).
64. How to implement a **Dead Letter Queue (DLQ)** in Kafka?
65. What is `max.partition.fetch.bytes`?
66. How does a consumer handle **Schema evolution**?
67. What is the impact of a high number of partitions on consumer rebalancing?
68. Explain the "Thundering Herd" problem in rebalancing.
69. How do you manually assign partitions to a consumer (`assign` vs `subscribe`)?
70. What is the impact of **Message Headers** on consumer performance?

## 4. Kafka Internals: Storage & Replication (20 Questions)
71. Deep dive: How does Kafka use **Zero-copy** (`sendfile`)?
72. What is the **Page Cache** and how does Kafka leverage it for performance?
73. Explain the log storage structure: **Indices, Time Indices, and Log files**.
74. How does Kafka perform **Log Compaction**? Describe the "Cleaner" thread.
75. What is the **high watermark** vs. **log end offset (LEO)**?
76. How is data replicated from Leader to Follower?
77. What is a **Fenced Replica**?
78. Explain the **Fetch Request** from a follower to a leader.
79. How does Kafka handle **Disk Failure**? (JBOD vs. RAID configurations).
80. What is the impact of the `flush.interval` and `flush.ms` settings?
81. Explain the **Request Queue** and **Response Queue** in a Kafka Broker.
82. What are **Network Threads** and **I/O Threads**? How to tune them?
83. How does Kafka handle **Backpressure** between Producer and Broker?
84. Explain the **Purgatory** in Kafka.
85. What is the significance of the `min.insync.replicas` setting?
86. How to recover a corrupted index file?
87. What is **Delayed Operations**?
88. Describe the **Metadata Cache** on the broker.
89. How is the **Leader Epoch** used during replication to prevent data loss?
90. Explain **Quota Management** for Produce and Fetch requests.

## 5. Reliability: Exactly-Once Semantics (EOS) (15 Questions)
91. What are the three delivery semantics: At-most-once, At-least-once, and Exactly-once?
92. How does the **Transactional Producer** work?
93. Explain the **Transaction Coordinator**.
94. What is the **Transaction Log** (`__transaction_state`)?
95. Describe the **2-Phase Commit (2PC)** in Kafka transactions.
96. What is the **Transactional ID**?
97. How do you enable **Idempotence**? Why is it a prerequisite for Transactions?
98. What is the **Isolation Level** (`read_committed` vs. `read_uncommitted`) in the Consumer?
99. Explain the **Marker Records** (Commit/Abort markers) in the partition log.
100. How does Kafka handle **Zombie Producers**? (Epoch-based fencing).
101. What is the impact of Transactions on **End-to-End Latency**?
102. Can you have a transaction across multiple clusters?
103. How to implement an **Atomic "Read-Process-Write"** loop?
104. What happens to a transaction if a broker fails mid-way?
105. Performance trade-offs of enabling Exactly-once.

## 6. Kafka Streams & Kafka Connect (15 Questions)
106. What is **Kafka Streams**? How does it differ from Flink or Spark Streaming?
107. Explain **KStream** vs. **KTable**.
108. What is a **GlobalKTable**?
109. Describe the **State Store** in Kafka Streams. How is it backed by Kafka?
110. Explain **Windowing** in Kafka Streams (Tumbling, Hopping, Sliding, Session).
111. What is **Duality of Stream and Table**?
112. Explain **RocksDB** usage in Kafka Streams.
113. How does Kafka Streams handle **Late-arriving data**?
114. What is **Kafka Connect**? Source vs. Sink Connectors.
115. Explain **Connect Distributed Mode** vs. Standalone Mode.
116. What is the **Schema Registry**? Why is it crucial for Kafka Connect?
117. How does **Single Message Transforms (SMT)** work in Connect?
118. What is **Converter** in Kafka Connect?
119. How to handle **Offset management** in a Source Connector?
120. Scaling Kafka Connect: How are "Tasks" distributed across "Workers"?

## 7. Performance Tuning, Operations & Scaling (30 Questions)
121. How to tune Kafka for **Maximum Throughput**?
122. How to tune Kafka for **Lowest Latency**?
123. What are the key **JVM tuning** recommendations for Kafka Brokers?
124. How to size the **Heap Memory** for a broker? (Why is a large heap often bad?).
125. Monitoring Kafka: List the top 5 **JMX Metrics** to watch.
126. How to handle **Rebalancing Storms** in a large cluster?
127. Explain **Rack Awareness** and why it's important for high availability.
128. How to perform a **Zero-Downtime Upgrade** of a Kafka cluster?
129. How to **Expand a Cluster**? (Adding brokers and reassigning partitions).
130. What is **Cruise Control**? How does it help in cluster management?
131. How to handle **Disk Pressure**? (Retention policies: Time-based vs. Size-based).
132. What is the impact of **Thin vs. Thick Clients**?
133. Explain **Tiered Storage** (KIP-405). Why is it revolutionary for Kafka?
134. How to secure Kafka: **SSL/TLS, SASL (Plain, SCRAM, GSSAPI/Kerberos), and ACLs**.
135. What is the **Inter-broker Protocol version** vs. **Log message format version**?
136. Challenges of running Kafka on **Kubernetes**.
137. How to manage **Large Message Sizes** (>1MB) in Kafka?
138. Troubleshooting: "Broker is not available" (Metadata issues).
139. Troubleshooting: "Consumer Fetch Session Timeout."
140. How to design a **Multi-Region/DR** strategy for Kafka? (MirrorMaker 2.0).
141. What is **Confluent Replicator** vs. Open Source MirrorMaker?
142. Impact of **OS-level TCP tuning** on Kafka.
143. How to detect and fix **Under-replicated Partitions**?
144. What is **Leader Skew** and how to fix it?
145. How to handle **Offline Partitions**?
146. Explain the **Controller Mutation Rate**.
147. Impact of **File Descriptor limits** on high-partition brokers.
148. How to optimize **Garbage Collection** for Low Pause Times (G1/ZGC in Kafka context).
149. Scaling Producers: Connection pooling and management.
150. Future of Kafka: Discussion on **Queues vs. Streams** and the role of Kafka in **Modern Data Stack**.

---
*Generated by Antigravity for SDE3 Interview Prep.*
