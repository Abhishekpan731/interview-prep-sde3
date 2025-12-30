# 300 Common SDE3 Interview Questions (Comprehensive)

## 1. Advanced Java & Engineering Excellence (50 Questions)
1. How do you design a Java library for high-throughput performance?
2. Explain "Zero GC" coding techniques in Java.
3. How would you handle a production JVM crash? Steps to investigate.
4. Discuss the trade-offs between using internal Java libraries vs third-party ones (Guava, Apache Commons).
5. How do you ensure "Binary Compatibility" in a distributed Java system?
6. Explain "SemVer" and how you apply it to Java artifacts.
7. How does Java 21's Virtual Threads change your architectural decisions?
8. When would you prefer a "Reactive" stack (Spring WebFlux) over Virtual Threads?
9. How to optimize a Java application for a "Cold Start" in a serverless environment (AWS Lambda)?
10. Describe a complex memory leak you've solved in your career.
11. How do you implement "Circuit Breakers" without using external libraries?
12. Explain the "Double-Checked Locking" pattern and why it's considered an optimization.
13. How to achieve 99.999% availability in a Java microservice?
14. How do you manage "Technical Debt" in a large Java codebase?
15. Explain "Static Analysis" tools (SonarQube, Checkstyle) and their importance.
16. How to handle "Database Migrations" in a CI/CD pipeline (Flyway, Liquibase)?
17. What is "Mutation Testing" (PITest)?
18. How to perform "Load Testing" on a Java application? (JMeter, Gatling).
19. Explain "Sharding" at the application level vs database level.
20. How to implement "Global ID Generation" (Snowflake ID)?
21. Discuss "Content Negotiation" in REST APIs.
22. How to handle "Idempotency" in a distributed system?
23. What is the difference between "Optimistic" and "Pessimistic" locking in Hibernate?
24. How do you debug "Packet Loss" or "Network Latency" issues in Java?
25. Explain the "Clean Code" principles you follow religiously.
26. How to refactor a "God Class" safely?
27. Discuss "Domain Driven Design" (DDD) in Java.
28. What are "Aggregates" and "Value Objects" in DDD?
29. How to design an "Event-Driven" architecture using Kafka and Java?
30. Explain "Exactly-Once" semantics in Kafka.
31. How to implement "Poison Pill" handling in message queues?
32. What is "Backpressure" and how do you implement it in a custom stream?
33. How to secure a Java application against "SQL Injection" and "XSS"?
34. Explain "OAuth2" and "OIDC" flows.
35. How to handle "Secrets Management" in Spring Boot?
36. Discuss "Aspect-Oriented Programming" (AOP) use cases.
37. How to implement a "Custom Annotation Processor"?
38. Explain "Bytecode Manipulation" using ASM or ByteBuddy.
39. How do you ensure "Backward Compatibility" of your REST APIs?
40. What is "API Versioning" strategy (URI vs Header)?
41. How to monitor Java application performance using Prometheus and Grafana?
42. Explain "Distributed Tracing" with OpenTelemetry.
43. How to handle "CORS" issues in a distributed environment?
44. Discuss "Micro-frontends" architecture.
45. How to optimize "JSON Serialization" performance (Jackson vs Gson vs DSL-JSON)?
46. Explain "Graceful Shutdown" logic in a Spring Boot application.
47. How to manage "Thread Pool Exhaustion"?
48. What is the impact of "TCP Slow Start" on Java application latency?
49. How to handle "Large File Uploads" in a REST API?
50. Explain "Service Mesh" (Istio, Linkerd) and its role in Java microservices.

## 2. Low-Level Design (LLD) (50 Questions)
51. Design an "LUR Cache" with O(1) complexity.
52. Design a "Rate Limiter" (Token Bucket vs Leaky Bucket).
53. Design a "Parking Lot" system.
54. Design a "Vending Machine" using the State Pattern.
55. Design an "Elevator System" for a 100-story building.
56. Design a "Library Management System."
57. Design a "Snake and Ladder" game.
58. Design a "Chess Game" with move validation.
59. Design a "Movie Booking System" (BookMyShow).
60. Design a "Splitwise" application.
61. Design a "Traffic Signal" system.
62. Design a "File System" (files, directories, permissions).
63. Design a "Logger Library" that supports different sinks and levels.
64. Design an "Auction System" (eBay).
65. Design a "URL Shortener" (LLD focus: class structure, DB schema).
66. Design a "Hotel Management System."
67. Design an "Online Food Delivery" (Zomato/Swiggy) LLD.
68. Design a "Cab Booking" system (Uber/Ola) LLD.
69. Design a "Stock Brokerage" system (Zerodha).
70. Design a "Notification Service" (Email, SMS, Push).
71. Design a "Rule Engine" for insurance premiums.
72. Design a "Job Scheduler" (Quartz/Hangfire style).
73. Design a "Web Crawler."
74. Design a "Payment Gateway" integration wrapper.
75. Design a "Shopping Cart" with discount rules.
76. Design a "Meeting Scheduler" (Google Calendar).
77. Design a "Tic-Tac-Toe" for N players.
78. Design a "Distributed Cache" client wrapper.
79. Design a "Pub-Sub" system locally.
80. Design a "Bloom Filter" for large datasets.
81. Design an "Expression Parser" (Reverse Polish Notation).
82. Design a "Dependency Injection" container from scratch.
83. Design a "Unit Testing Framework."
84. Design a "Message Queue" (simplified version of SQS).
85. Design a "Social Media Feed" (LLD).
86. Design a "Dictionary" (Trie-based).
87. Design an "In-memory Database" with ACID properties.
88. Design a "Recommendation System" (collaborative filtering placeholder).
89. Design a "Video Streaming" client (HLS/DASH considerations).
90. Design a "Command Line Interface" (CLI) tool framework.
91. Design a "Task Management System" (Trello/Jira).
100. Design an "Inventory Management" system for e-commerce.

## 3. High-Level Design (HLD) (50 Questions)
101. Design **Twitter** (Timeline, Tweet, Follow).
102. Design **Facebook/Instagram** (News Feed, Image upload).
103. Design **WhatsApp/Messenger** (Real-time chat, last seen).
104. Design **Netflix/YouTube** (Video streaming at scale).
105. Design **Uber/Lyft** (Driver matching, location tracking).
106. Design **Airbnb** (Search, Booking engine).
107. Design **Amazon** (E-commerce, Flash sales).
108. Design **Google Search** (Crawler, Indexer, PageRank).
109. Design **TinyURL** (Read/Write intensive).
110. Design **Google Drive/Dropbox** (Syncing, Deduplication).
111. Design **Reddit/HackerNews** (Upvoting, Threading).
112. Design **Ticketmaster** (High concurrency booking).
113. Design **Pinterest** (Image pinning, feed).
114. Design **Discord/Slack** (Group messaging, channels).
115. Design a **Distributed ID Generator**.
116. Design a **Distributed Cache** (Redis/Memcached style).
117. Design a **Web Crawler** (Scaling, Politeness).
118. Design an **Ad Click Aggregator**.
119. Design a **Monitoring System** (Prometheus style).
120. Design a **Logging System** (ELK/Splunk style).
121. Design a **Global Rate Limiter**.
122. Design a **Distributed Lock Service**.
123. Design a **Data Migration Tool** (Zero downtime).
124. Design a **Video Transcoding Service**.
125. Design a **Metrics Dashboard**.
126. Design a **Proximity Service** (Yelp/Google Maps).
127. Design a **Flash Sale** system (Handling 1M requests/sec).
128. Design a **Payment System** (Compliance, Reconciliation).
129. Design a **Notification Service** at scale.
130. Design a **Metadata Service** for S3.
131. Design a **Ride-sharing Pricing Engine**.
132. Design an **Autocomplete Service**.
133. Design a **Content Delivery Network (CDN)**.
134. Design a **Digital Wallet** (PayPal/Stripe).
135. Design a **News Feed Aggregator**.
136. Design a **Stock Exchange Matching Engine**.
137. Design highly available **Configuration Management**.
138. Design a **Multi-tenant SaaS** architecture.
139. Design an **Identity Management System** (Auth0 style).
140. Design a **Backup and Recovery** system for 1PB of data.
141. Discuss "Microservices" vs "Monolith" (When to switch).
142. Explain "Database Sharding" and "Consistent Hashing."
143. Discuss "Availability" vs "Consistency" (CAP Theorem).
144. Explain "PACELC" Theorem.
145. What is "Read After Write Consistency"?
146. How to handle "Hot Keys" in a distributed cache?
147. Explain "Service Discovery" and "Circuit Breakers."
148. Discuss "Database Indexing" internals (B-Trees, LSM-Trees).
149. Explain "Write-Ahead Logging" (WAL).
150. How to choose between "Relational" and "NoSQL" databases?

## 4. Databases & Distributed Systems (50 Questions)
151. How do "B-Trees" differ from "LSM-Trees"?
152. Explain "MySQL" vs "PostgreSQL" performance trade-offs.
153. What is "Database Isolation Levels" (Read Uncommitted to Serializable)?
154. Explain "MVCC" (Multi-Version Concurrency Control).
155. What is "Index-Only Scan"?
156. Explain "Deadlocks" in databases and how to resolve them.
157. How do "Primary-Secondary" and "Multi-Primary" replications work?
158. Explain "Quorum" in distributed databases (Cassandra/DynamoDB).
159. What is "Gossip Protocol"?
160. Explain "Paxos" and "Raft" consensus algorithms.
161. Difference between "Optimistic Locking" and "Pessimistic Locking" in DB.
162. How to handle "N+1 Query Problem"?
163. What is "Normalization" vs "Denormalization"?
164. Explain "ACID" vs "BASE."
165. How does "Redis" persist data? (RDB vs AOF).
166. Explain "Redis Cluster" and its sharding.
167. What is "MongoDB" WiredTiger engine?
168. How to perform "Pagination" in a massive database? (Offset vs Keyset).
169. Explain "Full Text Search" (Elasticsearch/Solr).
170. What is "Inverted Index"?
171. How to handle "Skewed Data" in distributed systems?
172. Explain "Two Phase Commit" (2PC) vs "Saga."
173. What is "Time to Live" (TTL) in caching?
174. Explain "Write-through", "Write-back", and "Write-around" caching strategies.
175. What is "Cache Stampede" (Thundering Herd)?
176. Explain "Consistent Hashing."
177. How does "Zookeeper" manage distributed coordination?
178. What is "Vector Clocks"?
179. Explain "Lamport Timestamps."
180. How to handle "Clock Drift" in distributed systems?
181. What is "Partition Tolerance"?
182. Explain "Fallacy of Distributed Computing."
183. How to design for "Scalability" (Vertical vs Horizontal)?
184. What is "Load Balancing" (L4 vs L7)?
185. Explain "Weighted Round Robin" vs "Least Connections."
186. What is "Anycast" routing?
187. Explain "Database Mirroring" vs "Clustering."
188. How to optimize "Join" operations on large datasets?
189. Explain "Bitmap Indexes."
190. What is a "Covering Index"?
191. How to scale "WebSockets"?
192. Explain "Server-Sent Events" (SSE).
193. Difference between "gRPC" and "REST."
194. What is "Protocol Buffers" (Protobuf)?
195. Explain "Message Overload" handling in Kafka.
196. How to monitor "Consumer Lag" in Kafka?
197. Explain "Exactly-once" processing in Spark/Flink.
198. What is "Stream Processing" vs "Batch Processing"?
199. How to design a "Data Warehouse" (Snowflake/Redshift).
200. Explain "Data Lake" vs "Data Warehouse."

## 5. Security, DevOps & CI/CD (50 Questions)
201. Explain "HTTPS" handshake in detail.
202. What is "TLS/SSL" offloading?
203. How to prevent "Man-in-the-Middle" (MITM) attacks?
204. Explain "JWT" and its security pitfalls (Signing vs Encryption).
205. What is "CSRF" and how to prevent it?
206. Explain "Content Security Policy" (CSP).
207. How to manage "Symmetric" and "Asymmetric" keys securely?
208. What is "KMS" (Key Management Service)?
209. Explain "RBAC" vs "ABAC" access control.
210. What is "Penetration Testing"?
211. Explain "SQL Injection" prevention (Parameterized queries).
212. How to secure "Microservices Communication" (mTLS).
213. Explain "Docker" architecture and container security.
214. What is the difference between "Container" and "Virtual Machine"?
215. Explain "Kubernetes" (Pod, Deployment, Service, Ingress).
216. How to manage "Helm Charts"?
217. Explain "Infrastructure as Code" (Terraform, CloudFormation).
218. What is "Immutable Infrastructure"?
219. Explain "CI/CD Pipeline" components.
220. What is "Canary Release" vs "Blue-Green Deployment"?
221. Explain "Zero-Downtime Deployment."
222. How to handle "Rollbacks" in production?
223. What is "Chaos Engineering" (Chaos Monkey)?
224. Explain "Observability" (Logs, Metrics, Traces).
225. What is "SRE" (Site Reliability Engineering)?
226. Explain "Error Budget" and "SLO/SLI."
227. How to implement "Automated Testing" in CI/CD?
228. Explain "Integration Testing" vs "End-to-End Testing."
229. What is "Performance Regression"?
230. How to manage "Configuration" across environments (Twelve-Factor App)?
231. Explain "Service Mesh" benefits.
232. What is "Log Aggregation" (Fluentd/Logstash)?
233. How to perform "Static Code Analysis" (SonarQube).
234. What is "Vulnerability Scanning" (Snyk/Trivy)?
235. Explain "Dependency Management" (Security updates).
236. What is "GitOps"?
237. Explain "Trunk Based Development" vs "GitFlow."
238. How to handle "Secrets" in CI/CD (GitHub Secrets, Vault).
239. What is "Compliance as Code"?
240. Explain "Serverless" security.
241. How to optimize "Docker Image Size"?
242. What is "Multi-stage Build"?
243. Explain "Sidecar" pattern in Kubernetes.
244. How to debug "OOMKilled" pods?
245. What is "Autoscaling" (HPA/VPA)?
246. Explain "Pod Anti-affinity."
247. What is "Liveness" and "Readiness" probes?
248. How to perform "Database Backups" in the cloud?
249. Explain "Disaster Recovery" strategies (Active-Active vs Active-Passive).
250. How to choose a "Cloud Provider" (AWS vs Azure vs GCP)?

## 6. Behavioral & Leadership (50 Questions)
251. Tell me about a time you handled a "Production Outage."
252. Describe a "Conflict" you had with a peer/manager and how you resolved it.
253. How do you "Mentor" junior engineers?
254. Tell me about a "Technical Decision" you made that failed. What did you learn?
255. How do you manage "Changing Requirements"?
256. Describe a time you had to "Persuade" stakeholders about a technical choice.
257. How do you balance "Feature Delivery" vs "Code Quality"?
258. Tell me about a time you went "Above and Beyond."
259. How do you stay "Up-to-date" with technology?
260. Describe your "Ideal Development Process."
261. How do you handle "Tight Deadlines"?
262. Tell me about a "Creative Solution" you implemented for a hard problem.
263. How do you conduct "Code Reviews"? What do you look for?
264. Describe a time you had to "Optimize" a slow system.
265. How do you handle "Imposter Syndrome" or team burnout?
266. What is your "Leadership Style"?
267. How do you prioritize "Technical Debt" in the product roadmap?
268. Tell me about a time you disagreed with a "Product Requirement."
269. How do you handle "Difficult Conversations" with team members?
270. Describe a time you had to "Lead" a project from scratch.
271. How do you evaluate a "New Tool or Framework" before adoption?
272. Tell me about a time you simplified a "Complex System."
273. How do you handle "Feedback" from peers?
274. What "Mistake" have you made that taught you the most?
275. How do you build "Trust" in a new team?
276. Describe a time you improved "Team Productivity."
277. How do you approach "Hiring" technical talent?
278. Tell me about a time you had to work with a "Legacy Codebase."
279. How do you define "Success" for a software project?
280. Describe your "Onboarding" process for new hires.
281. How do you handle "Stressful" work situations?
282. Tell me about a time you had to "Learn" a new technology quickly.
283. How do you ensure "Alignment" between business goals and engineering?
284. Describe a time you advocated for "Diversity and Inclusion."
285. How do you handle "Scope Creep"?
286. Tell me about a time you resolved a "Performance Bottleneck."
287. How do you approach "Post-mortems"?
288. Describe a time you took "calculated risk."
289. How do you balance "Work-Life Balance" for your team?
290. Tell me about a time you worked on a "Cross-functional" project.
291. How do you handle "Client Interactions" directly?
292. Describe a time you had to "Publicly Admit" a mistake.
293. How do you encourage "Open Communication" in your team?
294. Tell me about a time you "Influenced" the company's technical direction.
295. How do you handle "Remote Work" challenges?
296. Describe a time you had to "Scale" yourself as the company grew.
297. What is the "Most Proud" achievement in your software career?
298. How do you handle "Ambiguity" in project requirements?
299. Tell me about a time you had to "Say No" to a feature request.
300. Summary: What makes a "Great" SDE3 compared to a "Good" SDE2?

---
*Generated by Antigravity for SDE3 Interview Prep.*
