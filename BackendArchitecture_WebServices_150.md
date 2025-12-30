# 150 Backend Architecture & Web Services Interview Questions (SDE3)

## 1. Three-Layer Architecture: Controller, Service, Repository (25 Questions)
1. Explain the **Three-Layer Architecture** in detail. What are the specific responsibilities of each layer?
2. Why is it important to keep the **Controller** layer "thin"? What should stay in the Controller vs. the Service?
3. How do you handle **Cross-Cutting Concerns** (Logging, Security, Transactions) across these layers?
4. What is the **Service Layer** pattern? Should it contain business logic or just orchestrate domain objects?
5. Explain the **Repository Pattern**. How does it abstract data access from the business logic?
6. Discuss the trade-offs of using **Anemic Domain Model** vs. **Rich Domain Model** in a Spring Boot application.
7. How do you handle **Circular Dependencies** between services? What are the architectural red flags when this happens?
8. Explain the **"Single Responsibility Principle" (SRP)** in the context of a 3-layer architecture.
9. How do you decide when to create a new Service class vs. adding methods to an existing one?
10. What is the role of **DTOs (Data Transfer Objects)**? At which layer should they be converted to/from Domain Entities?
11. Explain **Component Scanning** and how it wires these layers together in Spring Boot.
12. What is the impact of **Transaction Propagation** when a method in Service A calls a method in Service B?
13. How to implement **Multiple Data Sources** within the Repository layer?
14. Should the **Repository** layer ever throw business-specific exceptions?
15. Explain **Interface-based Services**. Why is it a common practice to have an `interface` and an `impl`?
16. How do you manage **Application State** in a stateless REST architecture?
17. What is the **Facade Pattern** and how does it simplify complex service interactions?
18. Discuss **Package-by-Feature** vs. **Package-by-Layer**. Which is better for large-scale microservices?
19. How do you handle **Async Processing** within the Service layer?
20. What is the purpose of **MapStruct** or **ModelMapper** in this architecture?
21. Explain **Proxy Objects** in Spring. How do they affect `@Transactional` and `@Cacheable`?
22. How do you implement **Caching** at the Service layer? When to use `@Cacheable`?
23. What are the risks of leaking **Persistence Entities** (e.g., Hibernate proxies) to the Controller?
24. How to design services for **Reusability** across different Controllers (e.g., REST vs. GraphQL vs. gRPC)?
25. Explain the **Command-Query Separation (CQS)** within the service layer.

## 2. RESTful API Design & Maturity (25 Questions)
26. What is the **Richardson Maturity Model**? Walk through levels 0 to 3.
27. What are the core constraints of **REST** (Stateless, Client-Server, Cacheable, Unified Interface, Layered System, Code-on-Demand)?
28. How do you design **Idempotent** APIs? Which HTTP methods are naturally idempotent?
29. Explain **HATEOAS (Hypermedia as the Engine of Application State)**. Why is it rarely fully implemented in industry?
30. How to choose between **Path Variables** and **Query Parameters** for filtering and resource identification?
31. What are the best practices for **API Versioning** (URI, Header, Media Type)?
32. How to design for **Pagination and Sorting** in a REST API?
33. What is the difference between **PUT** and **PATCH**? When to use **JSON Merge Patch** vs. **JSON Patch**?
34. How do you handle **Bulk Updates** or **Bulk Deletes** in a REST environment?
35. What is **Content Negotiation**? How does Spring Boot handle it via `produces` and `consumes`?
36. Discuss **REST vs. GraphQL**. When is GraphQL a better architectural choice?
37. How to design a **Search API** that handles complex filters (AND, OR, ranges)?
38. What is the significance of **HTTP Status Codes**? When to use 201 vs 202 vs 204?
39. Explain **POST vs. PUT** for resource creation.
40. How to handle **Client-side Caching** using ETags and `If-None-Match`?
41. What is the **BFF (Backend For Frontend)** pattern and why is it used in mobile vs. web clients?
42. How to document APIs using **OpenAPI/Swagger**? What are the benefits of Schema-first vs. Code-first?
43. What is **RESTful API Rate Limiting**? How to communicate limits via headers?
44. How do you design APIs for **Long-running Tasks**? (202 Accepted + Polling/Webhook).
45. Explain **Webhooks**. How do you ensure secure and reliable delivery?
46. What is **CORS (Cross-Origin Resource Sharing)**? How to configure it at the Controller level?
47. How to design **stateless** APIs while still providing a personalized experience?
48. What is the impact of **HTTP/2** and **gRPC** on traditional REST architectures?
49. How to handle **Binary Data** (Image uploads/downloads) in REST APIs efficiently?
50. Explain **Backward Compatibility** strategies in API design.

## 3. Robust Error Handling & Exception Management (25 Questions)
51. How do you implement **Global Exception Handling** in Spring Boot?
52. Compare `@ControllerAdvice` vs. `@ExceptionHandler` at the class level.
53. What is the **Problem Details for HTTP APIs (RFC 7807)**? How to implement it in Spring?
54. How much information should an Error Response contain? (Security implications of exposing stack traces).
55. How do you differentiate between **Client Errors (4xx)** and **Server Errors (5xx)** logically?
56. What is your strategy for **Custom Exceptions**? Should you have many specific ones or a few generic ones with codes?
57. How do you handle **Database Exceptions** without leaking DB-specific details to the client?
58. Explain the role of **ResponseStatusException** vs. custom exception classes.
59. How to handle **Validation Errors**? What is the ideal JSON structure for nested field errors?
60. What is a **Circuit Breaker**'s role in error handling for cascading failures?
61. How to implement **Retry Logic** for transient errors (e.g., using `@Retryable`)?
62. How to handle **Timeout Exceptions** in a distributed service call?
63. Explain **Error Masking** in production environments.
64. How do you log exceptions? (What levels to use for which errors: WARN vs ERROR).
65. What is the impact of **Checked vs. Unchecked Exceptions** on Spring transactions?
66. How to handle **Async Exception Handling** (`@Async` methods)?
67. What is the **DefaultErrorAttributes** in Spring Boot and how to customize it?
68. How to handle **InterruptedExceptions** properly?
69. How to design a **Resilient API** that returns partial data if one service fails?
70. What are **Dead Letter Queues (DLQ)** and how do they relate to backend error handling?
71. How to map **Spring Security** exceptions (e.g., AccessDeniedException) to custom JSON errors?
72. How to implement **RequestId/CorrelationId** for tracing errors across logs?
73. What is **Fail-fast** vs **Fail-safe** in the context of backend processing?
74. How to handle **Resource Not Found** exceptions for non-existent IDs?
75. Discuss **Error Messaging** for Internationalization (i18n).

## 4. Input Validation & Data Integrity (20 Questions)
76. Explain **Java Bean Validation (JSR 380)** and its implementation (Hibernate Validator).
77. How to use `@Valid` vs. `@Validated`? When is group validation necessary?
78. How do you perform **Custom Validation**? Walk through creating a custom Annotation and Validator.
79. What is the difference between **Syntactic Validation** and **Semantic (Business) Validation**?
80. Where should validation happen? (Controller vs. Service vs. DB).
81. How to handle **Cross-field Validation** (e.g., `startDate` before `endDate`)?
82. Explain **Manual Validation** using the `Validator` object for complex scenarios.
83. How to prevent **SQL Injection** through input validation and parameterized queries?
84. What is **Input Sanitization**? How does it differ from Validation?
85. How to handle **XSS (Cross-Site Scripting)** prevention at the API layer?
86. What is **Data Integrity** in a multi-user, multi-threaded environment?
87. How to implementation **Optimistic Locking** (`@Version`) for data integrity?
88. What is **Pessimistic Locking**? When is it necessary despite the performance cost?
89. How to ensure **Idempotency** for "Create" operations to prevent duplicate data?
90. Explain **Referential Integrity** at the application level vs. database level.
91. How to handle **Large Payload validation** without consuming excessive memory?
92. What are the security risks of **Indirect Object References**?
93. How to validate **Enums** in request bodies correctly?
94. Explain **In-place updates** and partial updates integrity challenges.
95. How to handle **Concurrent Modification** of the same resource?

## 5. Persistence, Mapping & DTOs (20 Questions)
96. Why should you **Never** return JPA Entities directly from a Controller?
97. Explain the **DTO Pattern**. What are its variants (Request DTO vs. Response DTO)?
98. What is **LazyInitializationException**? How does the 3-layer architecture help or hinder this?
99. Explain the **Open Session in View (OSIV)** anti-pattern.
100. How to use **MapStruct** for high-performance mapping?
101. What is **Projections** in Spring Data JPA? How do they improve performance?
102. Explain the **Deep Copy vs. Shallow Copy** in the context of persistence.
103. How to handle **JsonView** for dynamic field filtering based on the role?
104. What is **Auto-mapping** vs. **Manual-mapping**? Which is better for maintainability?
105. How to handle **Polymorphic DTOs** (mapping inheritance)?
106. Explain **DTO flattening** and why it's useful for UI-heavy APIs.
107. How to handle **Audit fields** (`createdAt`, `updatedAt`) across layers?
108. What is the impact of **Lombok** on the readability and stability of DTOs?
109. How to handle **BigDecimals** and currency accurately in Web Services?
110. Explain the **Entity-DTO-Service** flow for a "Update" operation.
111. How to handle **Circular references** during JSON serialization?
112. What is **Jackson's JsonIgnore** vs. **JsonProperties** for controlling visibility?
113. How to implementation **Versioning of DTOs**?
114. Explain **BeanUtils.copyProperties** and why it's often considered a bad practice in SDE3 roles.
115. How to design for **JSON-LD** or other semantic web formats?

## 6. Performance, Stability & Scaling (20 Questions)
116. How to optimize **Response Time** for high-traffic endpoints?
117. What is **HTTP Connection Pooling**? How does it affect backend performance?
118. Explain **N+1 Query Problem** in the context of the Service-Repository layer.
119. How to implement **Response Caching** (HTTP level vs Application level)?
120. What is **Etag** and how it saves bandwidth?
121. Explain **Gzip/Brotli Compression** in Spring Boot.
122. How to handle **Heavy Database Queries**? (Async results, streaming).
123. What is **Thread Pool Tuning** for tomcat/netty?
124. Explain **Backpressure** implementation using Project Reactor's Flux/Mono.
125. How to implement **Bulkhead isolation** using `@Bulkhead` or specialized thread pools?
126. What is the impact of **Frequent GC** on API latency?
127. How to monitor **API Performance** using Actuator and Micrometer?
128. What is **Load Shedding**? How to implement it in a Spring Boot application?
129. Explain **Pre-fetching** and **Lazy-loading** strategies for web services.
130. How to handle **Sudden Spikes in Traffic**? (Autoscaling vs Quotas).
131. What is the performance impact of **Large JSON Payloads**?
132. How to use **Externalized Configuration** to tune performance at runtime?
133. What is **Zero Copy** transfer for file services?
134. How to implement **Rate Limiting** using Redis?
135. Discuss the impact of **Security Overheads** (JWT signature validation, DB checks) on latency.

## 7. Advanced Patterns & Production Troubleshooting (15 Questions)
136. How do you perform **Contract Testing** (Spring Cloud Contract) for stable integrations?
137. What is **Semantic Versioning** for internal APIs?
138. How to handle **Feature Flags** in a microservices architecture?
139. Explain the **Service Mesh** role in modern backend architectures.
140. How to debug a **Memory Leak** in a Spring Boot application?
141. What are the common causes of **High CPU Usage** in a REST service?
142. How do you perform **Canary Deployment** for a backend service?
143. What is **Distributed Tracing** (Span and Trace IDs)?
144. How to handle **Log Aggregation** and search across a cluster?
145. What is the **API First Design** approach?
146. How to implementation **Health Checks** (Liveness/Readiness)?
147. Discuss **Backward Compatibility** testing.
148. What is **Chaos Engineering** (Chaos Monkey) and why use it for backend stability?
149. How to design for **Observability** (Logs, Metrics, Traces)?
150. Summary: What is the most complex architectural challenge you've faced while developing RESTful APIs, and how did you overcome it?

---
*Generated by Antigravity for SDE3 Interview Prep.*
