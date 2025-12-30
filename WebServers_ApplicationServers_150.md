# 150 Web & Application Servers Interview Questions (SDE3)

## 1. Fundamentals: Web Servers vs. Application Servers (20 Questions)
1. Explain the fundamental difference between a **Web Server** and an **Application Server**.
2. How does a web server handle **Static Content** vs. **Dynamic Content**?
3. What is the **Common Gateway Interface (CGI)**? How does it differ from a Servlet Container?
4. Explain the **Event-driven (Async)** architecture vs. **Thread-per-request (Sync)** model.
5. What is a **Reverse Proxy**? Why is it crucial in a production environment?
6. Explain **HTTP/1.1 vs. HTTP/2 vs. HTTP/3** from a server-side perspective.
7. What is **SSL Termination**? Should it happen at the Load Balancer, Web Server, or App Server?
8. Explain the role of a **Servlet Container** (e.g., Tomcat) within the Java ecosystem.
9. What is a **Connector**? (HTTP, AJP).
10. Explain **Keep-Alive** and its impact on server resource utilization.
11. What is **Content Negotiation** at the web server level?
12. Explain **Gzip/Brotli Compression**. Where is it more efficient to perform: Web server or App server?
13. What is **Sticky Session (Session Affinity)** and how is it implemented at the server level?
14. Explain **Virtual Hosting** (Name-based vs. IP-based).
15. What are **Server-Sent Events (SSE)** and **WebSockets** support in modern servers?
16. Explain **Caching Headers** (Etag, Cache-Control, Expires).
17. What is **Hot Deployment** vs. **Cold Deployment**?
18. Explain the impact of the **Operating System's File Descriptor limits** on a high-traffic web server.
19. What is **Zero Copy** transfer and which servers support it?
20. Compare **Edge Computing** (Cloudflare Workers/Lambda@Edge) vs. traditional Web Servers.

## 2. Apache Tomcat (30 Questions)
21. Describe the internal architecture of **Apache Tomcat** (Server, Service, Connector, Engine, Host, Context).
22. What is **Catalina**? What is **Jasper**?
23. Explain the **Tomcat Thread Pool** (`maxThreads`, `minSpareThreads`, `acceptCount`).
24. What is the difference between the **HTTP/1.1 Connector** and the **AJP (Apache JServ Protocol) Connector**?
25. When would you use **Apache HTTPD + Tomcat (mod_jk/mod_proxy_ajp)** instead of just standalone Tomcat?
26. Explain **Tomcat Classloading Hierarchy**. How does it ensure isolation between different web apps?
27. What is the purpose of `server.xml`, `web.xml`, and `context.xml`?
28. How do you configure **JNDI Resources** (e.g., DataSources) in Tomcat?
29. Explain **Tomcat Valves**. How do they differ from Servlet Filters?
30. What is **Tomcat Jasper** and how does it handle JSP compilation?
31. How to enable **SSL/TLS** in Tomcat? (JSSE vs. APR).
32. What is the **Apache Portable Runtime (APR)** and how does it improve Tomcat performance?
33. How to perform **Session Replication** in a Tomcat Cluster? (DeltaManager vs. BackupManager).
34. Explain **Manager App** and **Host Manager** security best practices.
35. How to debug a **Memory Leak** in a Tomcat-hosted application? (Heap dumps, `jmap`).
36. What is the **"PermGen" (legacy) / "Metaspace" (modern)** issue in Tomcat during frequent redeployments?
37. How to configure **JVM Options** (`-Xms`, `-Xmx`, `-XX:MaxMetaspaceSize`) for Tomcat?
38. Explain **Context Fragment** files.
39. How does Tomcat handle **Virtual Threading** (Project Loom) in recent versions?
40. What is the **Shutdown Port** in Tomcat and why is it often disabled in production?
41. How to implement **Custom Error Pages** globally in Tomcat?
42. Explain **Embedded Tomcat** (used in Spring Boot) vs. **Standalone Tomcat**.
43. How to optimize **Tomcat startup time**?
44. What is the role of the `work` directory in a Tomcat installation?
45. Explain **Tomcat Logging** (JULI vs. SLF4J/Logback).
46. How to handle **Database Connection Pooling** using `Tomcat JDBC Pool`?
47. What is **Tomcat Rewrite Valve**?
48. How to prevent **Session Fixation** in Tomcat?
49. Explain the impact of **large WAR files** on Tomcat performance.
50. How to automate Tomcat deployment using Jenkins/Ansible?

## 3. NGINX (30 Questions)
51. What makes **NGINX** much faster than Apache HTTPD for concurrent connections? (Master/Worker process model).
52. Explain the **Event-driven, Non-blocking** architecture of NGINX.
53. What is the **Master Process** and **Worker Process** roles?
54. How to configure NGINX as a **Load Balancer**? List common algorithms (Round Robin, Least Conn, IP Hash).
55. Explain **NGINX Reverse Proxy** directives: `proxy_pass`, `proxy_set_header`.
56. What is **FastCGI**? How does NGINX communicate with PHP-FPM or other backends?
57. Explain **NGINX Caching** (`proxy_cache`, `proxy_cache_path`, `proxy_cache_key`).
58. What is **Micro-caching**?
59. Explain the **Location Block** matching priority in NGINX.
60. What is the difference between `rewrite` and `return`?
61. How to implement **SSL Termination** in NGINX?
62. Explain **OCSP Stapling** and why it's used in NGINX.
63. What is **Upstream** in NGINX and how to handle health checks (`max_fails`, `fail_timeout`)?
64. Explain **NGINX Buffering** (`proxy_buffers`). When to disable it?
65. What is **NGINX Streaming** (`proxy_buffering off`) used for?
66. How to handle **CORS** at the NGINX level?
67. Explain **NGINX Variables** (e.g., `$host`, `$remote_addr`, `$request_uri`).
68. What is the **Stub Status** module?
69. How to limit **Rate/Connections** using `limit_req` and `limit_conn`?
70. Explain **NGINX Rewrite Rules** (Flag types: `last`, `break`, `redirect`, `permanent`).
71. What is **NGINX Open Source** vs. **NGINX Plus**?
72. How to perform **Blue-Green deployment** using NGINX?
73. Explain **njs** (NGINX JavaScript) and when to use it over Lua.
74. What is the **NGINX Ingress Controller** in Kubernetes?
75. How to optimize **Static File Serving** ( `sendfile`, `tcp_nopush`, `tcp_nodelay`)?
76. What is **Gzip Static** module?
77. Explain **NGINX Auth Basic** and **Auth Request** modules.
78. How to hide **NGINX version** for security?
79. What is **NGINX Proxy Protocol** support?
80. How to troubleshoot "502 Bad Gateway" vs. "504 Gateway Timeout"?

## 4. Oracle WebLogic (20 Questions)
81. Describe the **WebLogic Domain** architecture (Admin Server + Managed Servers).
82. What is the **Node Manager**? Why is it essential for remote server management?
83. Explain **WebLogic Clusters**. How does it handle failover and load balancing?
84. What is the **T3 Protocol**?
85. Explain **WebLogic Server States** (SHUTDOWN, STARTING, RUNNING, SUSPENDING, etc.).
86. What is **WLST (WebLogic Scripting Tool)**?
87. How to manage **JDBC DataSources** (Generic vs. GridLink) in WebLogic?
88. Explain **WebLogic JMS** (Java Message Service) architecture: Modules, Sub-deployments, Persistent Stores.
89. What is the **Work Manager**? How to use it for thread management and priority indexing?
90. Explain **Production Redeployment** (Side-by-side deployment) in WebLogic.
91. What is the **Configuration Wizard** vs. **Domain Template**?
102. How to configure **LDAP** or an **Active Directory** as an Authentication Provider in WebLogic?
103. Explain **Staging Modes** (Stage, No-stage, External-stage) for application deployment.
104. What is the **Administration Console** and how to secure it?
105. How to troubleshoot **Stuck Threads** in WebLogic?
106. What is **Managed Server Independence (MSI)** mode?
107. Explain **WebLogic Zero Downtime Patching (ZDT)**.
108. How to perform **Backup and Recovery** of a WebLogic Domain?
109. What is **WebLogic Server Diagnostic Framework (WLDF)**?
110. Explain **Coherence** integration with WebLogic for in-memory data grid.

## 5. IBM WebSphere (20 Questions)
111. Explain **WebSphere Application Server (WAS)** architecture (Cell, Node, Node Agent, Server).
112. What is the **Deployment Manager (Dmgr)**?
113. Compare **WebSphere Traditional** vs. **WebSphere Liberty**.
114. What are the benefits of **WebSphere Liberty's composable kernel**?
115. Explain the **Profile** concept in WebSphere.
116. What is **wsadmin**? (JACL vs. Jython).
117. How to manage **WebSphere Clusters** (Vertical vs. Horizontal scaling)?
118. What is the **Service Integration Bus (SIBus)** and for what is it used?
119. Explain the **Plugin-cfg.xml** file in the context of IBM HTTP Server + WebSphere.
120. How to configure **Global Security** and **LTPA Tokens** in WebSphere?
121. What is **High Availability Manager (HAManager)** in WebSphere?
122. Explain **PMI (Performance Monitoring Infrastructure)**.
123. How to handle **Resource Adapters (J2C)** in WebSphere?
124. What is **WebSphere Proxy Server**?
125. Explain the **binary deployment process** using `.ear` files in WebSphere.
126. How to perform a **Silent Installation** of WebSphere?
127. What is **Application Edition Management** in WAS ND?
128. How to troubleshoot **Hung Threads** vs. **Memory Leaks** in WebSphere? (Thread analyzer, Memory analyzer).
129. Explain **Syncing Nodes** in a WebSphere cell.
130. What is **WebSphere’s Intelligent Management** (Elasticity, Health Management)?

## 6. Security, Tuning & Distributed Operations (30 Questions)
131. How to perform **Hardening** of Web and Application Servers? (Removing banners, disabling unused modules).
132. Explain **SSL/TLS Cipher Suites**. How to select the most secure ones?
133. What is the impact of **OCSP** and **CRL** on server startup/handshake performance?
134. How to manage **Certificate Expiry** across a large server fleet?
135. Explain **Cross-Site Scripting (XSS)** and **SQL Injection** mitigation at the Web Server layer (WAF/Modules).
136. What is **ModSecurity**?
137. How to handle **Log Rotation** (e.g., `logrotate`) and **Centralized Logging** (ELK/Graylog) for server logs?
138. Explain **Thread Dumps** and **Heap Dumps** analysis to solve performance bottlenecks.
139. How to tune **TCP parameters** (e.g., `net.ipv4.tcp_tw_reuse`, `tcp_max_syn_backlog`) on the server OS?
140. What is **Benchmarking**? (using `ab`, `wrk`, or `jmeter`) for servers.
141. Explain **Zero-Downtime Patching** strategies for clusters.
142. How to handle **Session Data** in a stateless world using Redis/DB instead of server-side replication?
143. What is the **Proxy Protocol** and why is it needed for preserving client IPs through multiple load balancers?
144. Explain **Application Performance Monitoring (APM)** integration with servers (Dynatrace, AppDynamics).
145. How to handle **File Upload limits** (`client_max_body_size`, `max-file-size`) across layers?
146. Discuss **Cold Start** issues in Java Application Servers vs. Lightweight Containers.
147. How to implement **Mutual TLS (mTLS)** for server-to-server communication?
148. What is **Virtualization overhead** on web server performance?
149. Explain **Containerization (Docker)** of Tomcat vs. Traditional installation.
150. Summary: If a server is responding with "503 Service Unavailable," walk me through your diagnostic process as a Senior Engineer.

---
*Generated by Antigravity for SDE3 Interview Prep.*
