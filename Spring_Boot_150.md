# 150 Spring Boot Interview Questions (SDE3)

## 1. Core Spring Boot & Auto-configuration (25 Questions)
1. Explain the internal working of `@SpringBootApplication`. What are the three annotations it encapsulates?
2. How does Spring Boot's **Auto-configuration** work internally? Explain the role of `META-INF/spring.factories` or `org.springframework.boot.autoconfigure.AutoConfiguration.imports`.
3. Explain the `@Conditional` annotation and its variants like `@ConditionalOnClass`, `@ConditionalOnProperty`, and `@ConditionalOnMissingBean`.
4. How would you create a **Custom Spring Boot Starter**? Walk through the steps and internal structure.
5. What is the difference between `@Component`, `@Service`, `@Repository`, and `@Controller`? When does it matter?
6. Explain the **Bean Lifecycle** in Spring. Where do `postProcessBeforeInitialization` and `postProcessAfterInitialization` fit in?
7. What is the difference between **Constructor Injection** and **Field Injection**? Why is Constructor Injection preferred for SDE3 level designs?
8. How does `@Value` work? Explain the role of `PropertySourcesPlaceholderConfigurer`.
9. What is the **Spring Expression Language (SpEL)**? Provide a complex use case.
10. How do you handle multiple implementations of the same interface? Explain `@Primary`, `@Qualifier`, and `@Ordered`.
11. Explain **Profiles** in Spring Boot. How do you implement environment-specific configuration?
12. What is **Externalized Configuration**? Explain the precedence order of configuration (Properties, YAML, Environment variables, Command-line args).
13. How does Spring Boot handle **YAML vs Properties** files internally?
14. What is the purpose of `SpringApplicationRunListener`?
15. Explain the concept of **Component Scanning**. How can you exclude specific classes or packages?
16. What is the difference between `BeanFactory` and `ApplicationContext`?
17. How does the **Deferred Import Selector** work in Spring Boot?
18. Explain **Lazy Initialization** (`@Lazy`) and its impact on startup time vs. memory.
19. How to use `@ConfigurationProperties` and why is it better than `@Value` for structured config?
20. Explain **Relaxed Binding** in Spring Boot.
21. What is the purpose of `ApplicationRunner` and `CommandLineRunner`?
22. How does Spring Boot determine the **Embedded Web Server** (Tomcat vs Jetty vs Undertow)?
23. Explain the **Context Hierarchy** in Spring (Parent-Child contexts).
24. How to debug auto-configuration issues using `--debug` or `ConditionEvaluationReport`?
25. What is **ObjectProvider** and when would you use it instead of direct autowiring?

## 2. Spring Web, MVC & REST (25 Questions)
26. Explain the life cycle of a request in **DispatcherServlet**.
27. What is the difference between `@RestController` and `@Controller`?
28. How do you implement **Global Exception Handling**? Explain `@ControllerAdvice` and `@ExceptionHandler`.
29. What is **Content Negotiation**? How does Spring Boot decide between JSON and XML?
30. Explain **HATEOAS** and its importance in mature REST APIs.
31. How to implement **Async REST Controllers**? Describe `Callable`, `DeferredResult`, and `WebAsyncTask`.
32. What is the difference between `RestTemplate` and `WebClient`? Why is `RestTemplate` being moved to maintenance mode?
33. Explain **Filters vs Interceptors** in Spring MVC.
34. How to handle **Binary Data** (File upload/download) in a Spring Boot REST API?
35. What is `@RequestBody` and `@ResponseBody`? How does `HttpMessageConverter` work?
36. How to implement **Versioning** in REST APIs (URI, Header, Media Type)?
37. Explain **Cross-Origin Resource Sharing (CORS)** configuration in Spring Boot.
38. What is the role of `ViewResolver`?
39. How do you implement **Request Validation**? Explain `@Valid`, `@Validated`, and custom `ConstraintValidator`.
40. What is **Server-Sent Events (SSE)** and how to implement it in Spring Boot?
41. Explain the difference between **Path Variables** and **Request Parameters**.
42. How to implement **Rate Limiting** at the controller level?
43. What is **Jackson Serialization/Deserialization**? How to customize it using `@JsonView` or `@JsonProperty`?
44. Explain **Message Converters** internals.
45. How to handle **ETags** and conditional requests in Spring Boot?
46. What is **Spring WebFlux**? When should you choose Reactive over MVC?
47. Explain `Mono` and `Flux` in the context of Spring WebFlux.
48. How does **Netty** differ from **Tomcat** in the Spring Boot ecosystem?
49. What is **Functional Routing** in WebFlux?
50. How to handle **Context Propagation** in a Reactive stream (e.g., passing trace IDs)?

## 3. persistence: Spring Data JPA & Hibernate (25 Questions)
51. Explain the difference between `save()`, `persist()`, `merge()`, and `saveAndFlush()` in JPA.
52. How does **Hibernate First Level Cache** work? How does it differ from the **Second Level Cache**?
53. What is the **N+1 Selection Problem**? How do you solve it using `JOIN FETCH` or `@EntityGraph`?
54. Explain `@Transactional`. What happens when you call a transactional method from another method within the same class?
55. Describe **Propagation Levels** in Spring Transactions (`REQUIRED`, `REQUIRES_NEW`, etc.).
56. What are **Isolation Levels**? How do they map to database isolation levels?
57. Explain **Pessimistic vs Optimistic Locking** in JPA.
58. What is the purpose of `@Modifying` annotation?
59. How does **Dirty Checking** work in Hibernate?
60. Explain **Auditing** in Spring Data JPA (`@CreatedBy`, `@LastModifiedDate`).
61. What is the difference between `JpaRepository`, `PagingAndSortingRepository`, and `CrudRepository`?
62. How to implement **Dynamic Queries**? Explain `Specification` and `QueryDSL`.
63. What is the role of `PersistenceContext` and `EntityManager`?
64. How do you handle **Large Result Sets**? Explain `Stream` vs `List` vs `Page` in repository methods.
65. Explain **Lazy vs Eager Loading**. What is `LazyInitializationException` and how to fix it correctly?
66. How to map **Inheritance** in JPA? (Single Table, Joined, Table Per Class).
67. What is **Composite Primary Key**? Explain `@IdClass` vs `@EmbeddedId`.
68. How to use **Projections** (Interface-based vs DTO-based)?
69. Explain **Batch Processing** in Spring Data JPA.
70. How to handle **Soft Deletes** using Hibernate `@SQLDelete` and `@Where`?
71. What is the significance of the `flush()` operation?
72. How do you implement **Multi-tenancy** at the JPA level?
73. Explain **Connection Pooling** (HikariCP). How to tune pool size?
74. How to integrate **NoSQL** (MongoDB/Cassandra) with Spring Boot?
75. What is **Entity Lifecycle Hooks**? (`@PrePersist`, `@PostLoad`, etc.).

## 4. Spring Security & OAuth2 (25 Questions)
76. Explain the **Security Filter Chain** internals.
77. What is the difference between `Authentication` and `Authorization`?
78. How does `SecurityContextHolder` work? Explain the default ThreadLocal strategy.
79. What is **CSRF**? Why is it usually disabled in Stateless/REST APIs?
80. Explain **Method Level Security** (`@PreAuthorize`, `@PostAuthorize`, `@Secured`).
81. How to implement a custom **AuthenticationProvider**?
82. Difference between `UserDetailsService` and `UserDetailsManager`.
83. Explain **CORS** vs **CSRF**.
84. How to handle **JWT (JSON Web Tokens)** in Spring Security?
85. What is **OAuth2**? Explain the Authorization Code Grant vs Client Credentials Grant.
86. How to implement **Single Sign-On (SSO)** using Spring Security?
87. What is an **Access Token** vs a **Refresh Token**?
88. Explain the role of **Resource Server** and **Authorization Server** in OAuth2.
89. How to customize the **Success and Failure Handlers** in Spring Security?
90. What is **Principle of Least Privilege** in the context of Spring Security?
91. How to handle **Session Management** (Stateless vs Session-based)?
92. Explain **BCryptPasswordEncoder** and why it's better than MD5/SHA.
93. What is **SAML** and does Spring Security support it?
94. How to protect against **Brute Force Attacks** at the security layer?
95. What is **Spring Security ACL (Access Control List)**?
96. Explain the **DelegatingFilterProxy**.
97. How to share Security Context in **Async/Multi-threaded** environments?
98. What is **OAuth2 Login** vs **OAuth2 Client**?
99. How to implement **Identity Provider (IdP)** selection in Spring Security?
100. Explain **Content Security Policy (CSP)** headers in Spring Security.

## 5. Microservices, Cloud & Actuator (25 Questions)
101. What is **Spring Cloud Config**? How to handle dynamic configuration refreshes?
102. Explain **Service Discovery** (Eureka/Consul). Why is it needed in microservices?
103. What is a **Feign Client**? How does it simplify inter-service communication?
104. Explain the **API Gateway** (Spring Cloud Gateway). What are Filters and Predicates?
105. What is **Circuit Breaker** (Resilience4j)? Explain the states (Open, Closed, Half-Open).
106. Explain **Distributed Tracing** (Sleuth/Micrometer Tracing) and **Zipkin**.
107. How to handle **Distributed Transactions** in microservices (Saga Pattern)?
108. What is **Spring Cloud Bus**?
109. Explain **Client-side Load Balancing** (LoadBalancer/Ribbon).
110. What is **Spring Boot Actuator**? List common endpoints (`/health`, `/metrics`, `/info`).
111. How to create a **Custom Actuator Endpoint**?
112. What is **Micrometer**? How does it integrate with Prometheus/Grafana?
113. Explain **Log Aggregation** in a microservices environment (Sidecar vs centralized).
114. How to handle **Service-to-Service Security**?
115. What is the **Sidecar Pattern**?
116. Explain **Health Check vs Readiness Check vs Liveness Check**.
117. What is **Spring Cloud Stream**? How does it abstract message brokers like Kafka/RabbitMQ?
118. Explain **Consumer Groups** and **Partitions** in a Spring-Kafka integration.
119. What is **Polyglot Persistence** in microservices?
120. How to implement **Backpressure** in an event-driven microservice?
121. What is **Spring Cloud OpenFeign** interceptors?
122. Explain **Fault Tolerance** strategies besides Circuit Breakers (Retry, Bulkhead, RateLimiter).
123. How to perform **Zero-Downtime Deployment** for a Spring Boot app?
124. Explain **Graceful Shutdown** in Spring Boot 2.3+.
125. What is **Spring Cloud Sleuth**'s replacement in Spring Boot 3? (Micrometer Tracing).

## 6. Testing, Performance & Production (25 Questions)
126. What is `@SpringBootTest`? Difference between `DEFINED_PORT` vs `RANDOM_PORT`.
127. Explain `@WebMvcTest` and `@DataJpaTest`. Why are they called "Slices"?
128. How to use **MockMvc** to test controllers?
129. What is **Mockito**? Explain `@Mock` vs `@MockBean`.
130. How to use **Testcontainers** for integration testing?
131. What is **Contract Testing** (Spring Cloud Contract)?
132. How to test **Asynchronous Methods** (`@Async`)?
133. What is **AOP (Aspect Oriented Programming)** and how to use it for logging/metrics?
134. Explain the difference between `@Before`, `@After`, `@Around`, and `@Pointcut`.
135. How to optimize **Spring Boot Startup Time**? (Lazy init, CDS, AOT).
136. What is **Spring Native** and **GraalVM**?
137. How to handle **Memory Leaks** in a Spring Boot application?
138. Explain **Thread Pool Tuning** for specialized tasks.
139. What is **Caching** in Spring Boot (`@Cacheable`, `@CacheEvict`)?
140. Which **Cache Managers** does Spring Boot support (Redis, Caffeine, Ehcache)?
141. How to monitor **Database Connection Pool** health?
142. What is **Heap Dump** and how to capture it from a running Spring Boot app?
143. Explain **JMX (Java Management Extensions)** support in Spring Boot.
144. How to use **Spring Boot Admin** for monitoring?
145. What is **Flyway/Liquibase**? Why use it for database schema migrations?
146. How to handle **Big Data Exports** without OOM?
147. What is `@Scheduled` task? How to handle it in a multi-instance (clustered) environment? (ShedLock).
148. Explain the **Bill of Materials (BOM)** and its role in dependency management.
149. How to implement **Correlation IDs** for logging across microservices?
150. Future of Spring: Discussion on **Spring Framework 6** and **Spring Boot 3** (Java 17 baseline, Jakarta EE).

---
*Generated by Antigravity for SDE3 Interview Prep.*
