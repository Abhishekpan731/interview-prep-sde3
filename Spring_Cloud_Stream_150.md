# 150 Spring Cloud Stream Interview Questions (SDE3)

## 1. Fundamentals & Architecture (25 Questions)
1. What is **Spring Cloud Stream**? How does it simplify event-driven microservices?
2. Explain the core concept: **Source, Sink, and Processor** (Legacy) vs. **Functional Programming Model**.
3. What is the **Binder**? How does it abstract away specific messaging middleware (Kafka, RabbitMQ, Solace)?
4. Explain the **Programming Model** transition from `@EnableBinding` to Functional interfaces (`java.util.function`).
5. What are the benefits of using a **Functional Model** in Spring Cloud Stream 3.x+?
6. Describe the **Binding Hierarchy**: Application -> Binding -> Binder -> Middleware.
7. What is a **Destination**? How is it mapped to an actual topic or queue?
8. Explain the role of **Content-Type Negotiation** in Spring Cloud Stream.
9. What is **Message Conversion**? How does Spring Cloud Stream handle JSON, Avro, and Protobuf?
10. Explain **Binder Isolation**. Can you use multiple binders (e.g., Kafka and RabbitMQ) in the same application?
11. What is the **StreamBridge**? When should you use it instead of functional bindings?
12. Explain the **Health Indicator** for binders in Spring Boot Actuator.
13. What is the significance of the `spring.cloud.stream.function.definition` property?
14. How does Spring Cloud Stream handle **Application Startup**? (Lazy vs. Eager binding).
15. What is the difference between a **Message** and a **Payload**?
16. Explain **Message Headers** in Spring Cloud Stream. How are they mapped to native headers (Kafka headers)?
17. What is **Partitioning** in Spring Cloud Stream? Why is it handled at the application level?
18. Explain **Instance Index** and **Instance Count** for scaling.
19. What is **Consumer Group** management?
20. How does Spring Cloud Stream handle **Auto-provisioning** of topics/queues?
21. What is the **Reactive Model** support (Project Reactor) in SCSt?
22. Explain **Batch Listening** mode.
23. What is the **Binder SPI**?
24. How to perform **Polled Consumption**?
25. Explain the impact of **Spring Boot 3 / Spring Framework 6** on Spring Cloud Stream.

## 2. Bindings, Channels & Functions (25 Questions)
26. How to define a **Producer** using `Supplier<T>`?
27. How to define a **Consumer** using `Consumer<T>`?
28. How to define a **Processor** using `Function<T, R>`?
29. Explain **Multi-In/Multi-Out** bindings. How to handle multiple input or output streams in one function?
30. What is **Binding Names** convention (e.g., `functionName-in-0`, `functionName-out-0`)?
31. How to customize **Destination Mapping** for specific functions?
32. Explain **Input/Output Message Conversion** customization.
33. How to use **Custom Message Converters**?
34. Explain the usage of **Generic types** in functional bindings.
35. How to handle **Reactive Streams** (`Flux` and `Mono`) as functional inputs?
36. What is the difference between `Consumer<T>` and `Consumer<Message<T>>`?
37. How to access **Message Headers** inside a functional consumer?
38. Explain **Dynamic Destinations** using `StreamBridge`.
39. How to handle **Blocking I/O** inside a reactive stream handler?
40. Explain **Concurrency** within a single consumer binding.
41. What is **Polled Consumer** and how is it different from event-driven consumption?
42. How to configure **Max Messages per Poll**?
43. Explain **Composition** of functions (`function1|function2`).
44. How to use **Routing Functions**?
45. What is the **Target Protocol** header and how it facilitates routing?
46. Explain **Message Interceptors** in SCSt.
47. How to use **Local Transactions** with Spring Cloud Stream?
48. What is **Message Versioning**?
49. How to handle **Large Payloads** (Serialization overhead)?
50. Explain **Backpressure** between the binder and the functional code.

## 3. Kafka Binder Internals (30 Questions)
51. What is the **Spring Cloud Stream Kafka Binder**?
52. How does the binder map **Consumer Groups** to Kafka groups?
53. Explain **Offset Management** in SCSt Kafka. Difference between `auto-commit` and manual commits.
54. What is the **Binding-specific Kafka configuration**? (e.g., `producer.configuration`, `consumer.configuration`).
55. Explain **Provisioning** of Kafka topics (Replication factor, partitions).
56. How does SCSt handle **Native Decoding** vs. **Binder Decoding**?
57. What is **Kafka Streams** support in Spring Cloud Stream?
58. Explain **KStream, KTable, and GlobalKTable** bindings.
59. How to perform **Stateful Transformations** using SCSt Kafka Streams?
60. Explain **Windowing** (Tumbling, Hopping, Session) in the Kafka Streams binder.
61. What is **Interactive Queries** in Kafka Streams?
62. How to handle **Exactly-once Semantics (EOS)** with Kafka Binder?
63. Explain **DLQ (Dead Letter Queue)** implementation for Kafka.
64. What is **Custom Deserialization/Serialization** (Serde) in Kafka Streams context?
65. Explain **Partition Key Expression** and **Partition Selector Strategy**.
66. How to achieve **Key-based Partitioning** in Kafka?
67. What is **Kafka Binder Health Indicator** configuration?
68. Explain **Batch Mode** processing in Kafka Binder.
69. How to handle **Headers Mapping** (JSON vs. Native)?
70. What is the **Retry** mechanism at the Kafka Binder level?
71. Explain the **Pause/Resume** functionality for Kafka consumers.
72. How to use **KStream join** in Spring Cloud Stream?
73. What is the impact of **Kafka Transactional Producer** on SCSt?
74. Explain **Topology** visualization for Kafka Streams bindings.
75. How to handle **Consumer Side Multi-threading** in Kafka binder?
76. What is **Message Timestamp** handling in Kafka?
77. Explain **Compact Topic** support.
78. How to use **Zstd/Snappy** compression via Kafka binder?
79. What is **Standardize Header** mapping?
80. How to troubleshooting **Rebalancing** issues in a SCSt-Kafka app?

## 4. RabbitMQ Binder Internals (20 Questions)
81. What is the **Spring Cloud Stream RabbitMQ Binder**?
82. Explain the mapping of **Exchange, Queue, and Routing Key**.
83. What is the default **Exchange Type** used by SCSt?
84. How to handle **Quorum Queues** and **Streams (RabbitMQ 3.9+)**?
85. Explain **Publisher Confirms and Returns**.
86. What is **Dead Lettering** in RabbitMQ? How does SCSt configure the `x-dead-letter-exchange`?
87. How to implement **Delayed Message Exchange** with SCSt?
88. Explain **Tuning Performance**: Prefetch count, concurrency, and batching.
89. What is **Anonymous Groups** vs. Named Groups?
90. How to handle **Multiplexing**?
91. Explain **DLQ Reprocessing** patterns for RabbitMQ.
92. What is the **RepublishToDlq** option?
93. How does SCSt handle **Virtual Hosts**?
94. Explain **Connection Factory** configuration for the binder.
95. What is the **Spring AMQP** relationship with SCSt RabbitMQ binder?
96. How to use **Custom Exchange/Queue Provisioning**?
97. Explain **Container Type** (Direct vs. Simple).
98. How to handle **Unbound Bindings**?
99. Explain **Lazy Queues** configuration.
100. How to monitor **RabbitMQ Binder Metrics**?

## 5. Error Handling & Resilience (20 Questions)
101. What are the three levels of **Error Handling** in Spring Cloud Stream? (Application, Binder, Middleware).
102. Explain the **Retry Policy** (`max-attempts`, `back-off`).
103. What is the **Default Service Activator** for errors?
104. How to use **Error Channels** (e.g., `destination.group.errors`)?
105. What is the **ErrorMessageStrategy**?
106. Explain **DLQ (Dead Letter Queue)** strategies. When to send to DLQ?
107. How to implement **Manual Error Handlers**?
108. Explain **Global Error Channels**.
109. What is the difference between **Binder-specific retry** and **Spring Retry**?
110. How to handle **Serialization errors**?
111. What is the **ListenerContainerCustomizer** and how to use it for error handling?
112. Explain **Stateful Retry** vs. **Stateless Retry**.
113. How to perform **Alerting** on stream failures?
114. How to implement **Circuit Breakers** (Resilience4j) with Spring Cloud Stream?
115. What is the **Drop** strategy for erroneous messages?
116. How to handle **Duplicate Messages** (Idempotency)?
117. Explain **Poison Pill** handling.
118. How to **Reprocess** messages from a DLQ?
119. What is **RecoveryCallback**?
120. How to handle **Network Outages** between the binder and middleware?

## 6. Testing, Operations & Best Practices (30 Questions)
121. How to **Unit Test** functional handlers?
122. What is the **Test Binder**? How to use `InputDestination` and `OutputDestination` for integration testing?
123. Explain **Integration Testing** using **Testcontainers** (Kafka/RabbitMQ).
124. How to verify **Message Headers** in tests?
125. What is **Schema Registry** integration? Why is it crucial for Avro?
126. Explain **Confluent Schema Registry** vs. **Spring Cloud Schema Registry**.
127. How to manage **Schema Evolution** (Backward, Forward, Full compatibility)?
128. Explain **Monitoring** with Micrometer and Actuator.
129. What are the key **JMX Metrics** for binders?
130. How to implement **Distributed Tracing** (Sleuth/Micrometer Tracing) for event flows?
131. Explain **Cloud Native Buildpacks** for SCSt applications.
132. How to handle **Multi-version** co-existence of streams?
133. What is **Service Discovery** role in Spring Cloud Stream?
134. Explain the **Security** aspects (SSL/SASL) for producers and consumers.
135. How to handle **Sensitive Data** in message payloads (Encryption)?
136. What is **CDC (Change Data Capture)** integration with Spring Cloud Stream (Debezium)?
137. Explain **Zero-Downtime Migration** from legacy `@EnableBinding` to the functional model.
138. How to optimize **CPU and Memory** for stream-heavy apps?
139. What is **Batch Size vs. Linger** tuning for Kafka?
140. How to handle **Large Messages** (Claim Check Pattern)?
141. Explain **Event Sourcing** with Spring Cloud Stream.
142. What is **CQRS** with Spring Cloud Stream?
143. How to perform **Stream Joins** across different binders?
144. Explain **Graceful Shutdown** of stream applications.
145. What are the common **Performance Bottlenecks** in SCSt?
146. How to use **Spring Cloud Data Flow (SCDF)** to manage streams?
147. What is **Stream Partitioning** and why is it important for ordering?
148. Explain **Ordered Delivery** vs **Parallel Processing**.
149. How to handle **Backlog** situations?
150. Summary: What is the most complex event-driven architecture you've implemented using Spring Cloud Stream, and how did you handle data consistency?

---
*Generated by Antigravity for SDE3 Interview Prep.*
