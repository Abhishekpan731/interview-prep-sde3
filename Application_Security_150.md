# 150 Application Security Interview Questions (SDE3)

## 1. Core Security Concepts & Cryptography (25 Questions)
1. Explain the **CIA Triad** (Confidentiality, Integrity, Availability) with examples in a web application.
2. What is the difference between **Symmetric** and **Asymmetric Encryption**? When would you use each?
3. Explain **Hashing vs. Encryption vs. Encoding**. Give use cases for each.
4. What is a **Salt** and **Pepper** in the context of password hashing? Why are they needed?
5. How does **TLS/SSL Handshake** work? Explain the role of Certificates and CAs.
6. What is **Forward Secrecy** in TLS?
7. Explain **Digital Signatures** and how they ensure non-repudiation.
8. What is a **Replay Attack**? How do you prevent it using Nonce or Timestamps?
9. Explain **Message Authentication Codes (MAC)** and **HMAC**.
10. What are the security risks of using **Base64** for "hiding" sensitive data?
11. Discuss the transition from **SHA-1** to **SHA-256/3**. Why are collisions a problem?
12. What is **Key Stretching** (e.g., PBKDF2, BCrypt, Argon2)?
13. Explain the **Principle of Least Privilege (PoLP)**.
14. What is **Defense in Depth**? Provide a layered security strategy for a Java microservice.
15. Explain **Zero Trust Architecture**.
16. What is a **Man-in-the-Middle (MITM)** attack?
17. How do you securely store **Secret Keys** (KMS, Vault, Environment Variables)?
18. What is **Entropy** in the context of random number generation? Why use `SecureRandom` over `Random`?
19. Explain **Identity and Access Management (IAM)**.
20. What is **Multi-Factor Authentication (MFA)** and its various factors (Knowledge, Possession, Inherence)?
21. Discuss **Biometric Security** risks.
22. What is **Side-Channel Attack** (e.g., Timing attacks)?
23. Explain **Public Key Infrastructure (PKI)** components.
24. What is **Certificate Pinning**? Is it still recommended?
25. Explain **Perfect Forward Secrecy (PFS)**.

## 2. OWASP Top 10 & Common Vulnerabilities (30 Questions)
26. Walk through the **OWASP Top 10 (2021)**. Which ones are most relevant to Java developers?
27. What is **SQL Injection (SQLi)**? Explain the difference between Error-based, Blind, and Time-based SQLi.
28. How do **Parameterized Queries** and **ORMs** prevent SQLi?
29. What is **Cross-Site Scripting (XSS)**? Explain Stored, Reflected, and DOM-based XSS.
30. How do **Content Security Policy (CSP)** headers mitigate XSS?
31. What is **Cross-Site Request Forgery (CSRF)**? How do Synchronizer Tokens work?
32. Explain **Broken Access Control**. How to test for IDOR (Insecure Direct Object Reference)?
33. What is **Insecure Deserialization**? Why is it extremely dangerous in Java?
34. Explain **XML External Entity (XXE)** injection. How to disable DTD processing in Java?
35. What is **Security Misconfiguration**? Give examples in a cloud environment.
36. Discuss **Vulnerable and Outdated Components**. How do tools like Snyk or OWASP Dependency-Check help?
37. What is **Server-Side Request Forgery (SSRF)**? How can it lead to cloud metadata exposure?
38. Explain **Log Injection/Log Forging**. How does it relate to the Log4Shell vulnerability?
39. What is **Path Traversal / Directory Traversal**?
40. Explain **Command Injection**.
41. What is **HTTP Parameter Pollution**?
42. Discuss **Clickjacking** and how `X-Frame-Options` or `Content-Security-Policy` frames help.
43. What is **Subdomain Takeover**?
44. Explain **Open Redirect** vulnerabilities.
45. What is **Session Fixation**?
46. Discuss **Sensitive Data Exposure** (e.g., PII in logs).
47. What is **Insufficient Logging & Monitoring**?
48. Explain **Race Conditions** in security (TOCTOU - Time-of-Check to Time-of-Use).
49. What is **Padding Oracle Attack**?
50. Discuss **JWT Security Pitfalls** (e.g., `none` algorithm, weak secrets).
51. What is **Distributed Denial of Service (DDoS)** tuning at the application level?
52. Explain **API Security** specialized risks (BOLA, Broken Function Level Authorization).
53. What is **Credential Stuffing** vs. **Brute Force**?
54. Explain **Business Logic Vulnerabilities**.
55. What is **Mass Assignment** vulnerability?

## 3. Web & API Security (25 Questions)
56. Explain **OAuth 2.0** Flows (Authorization Code, Client Credentials, PKCE).
57. What is **OpenID Connect (OIDC)**? How does it differ from OAuth 2.0?
58. What are **JSON Web Tokens (JWT)**? Explain Jose headers, Payload, and Signature.
59. How do you securely handle **JWT Revocation**?
60. What is **CORS (Cross-Origin Resource Sharing)**? Is it a security feature?
61. Explain **Strict-Transport-Security (HSTS)** header.
62. What is **Secure and HttpOnly** flag on cookies?
63. How to implement **Rate Limiting** and **Throttling** for APIs?
64. Discuss **API Key vs. OAuth Token**.
65. What is **Mutual TLS (mTLS)** and where is it used?
66. Explain **GraphQL Security** risks (e.g., Deeply nested queries, Batching attacks).
67. How to prevent **Data Scraping**?
68. What are **Security Headers** you should always include in your responses?
69. Explain **SameSite Cookie Attribute** (`Strict`, `Lax`, `None`).
70. How to handle **File Upload Security** (Virus scanning, file extension validation, storage)?
71. What is **Subresource Integrity (SRI)**?
72. Discuss **Websocket Security** (WSS and CSWSH).
73. How to secure **Microservices Communication**?
74. What is **API Gateway's** role in security?
75. Explain **Content-Type Sniffing** and `X-Content-Type-Options: nosniff`.
76. What is **JSON Hijacking**?
77. Discuss **Third-party Auth** (Google/GitHub login) security implications.
78. How to perform **Audit Logging** for security events?
79. What is **Input Sanitization vs. Validation**?
80. Explain **Output Encoding**.

## 4. Cloud, Container & Infrastructure Security (20 Questions)
81. Explain the **Shared Responsibility Model** in Cloud Security.
82. What is **Infrastructure as Code (IaC) Scanning**?
83. How to secure **Docker Images** (Distroless, non-root users, scanning)?
84. What is **Kubernetes Role-Based Access Control (RBAC)**?
85. Explain **Kubernetes Network Policies**.
86. What is **Secret Scanning** in CI/CD pipelines?
87. How to manage **Cloud IAM Roles** for EC2/EKS?
88. What is **Runtime Application Self-Protection (RASP)**?
89. Explain **Web Application Firewall (WAF)** vs. **Firewall**.
90. What is **Serverless (Lambda) Security**?
91. Discuss **Data at Rest** vs. **Data in Transit** encryption in AWS/Azure.
92. What is **Log Management and SIEM** (Security Information and Event Management)?
93. Explain **VPC Peering** security risks.
94. What is **Cloud Workload Protection Platform (CWPP)**?
95. How to handle **Orphaned Resources** security risks?
96. Discuss **Immutable Infrastructure** and security.
97. What is **DevSecOps**? How do you integrate security into the pipeline?
98. Explain **Container Escape** attacks.
99. What is **Privileged Container** and why is it a risk?
100. How to use **HashiCorp Vault** for dynamic secrets?

## 5. Java/Spring Specific Security (25 Questions)
101. How does **Spring Security's Filter Chain** work internally?
102. Explain **Method-level Security** in Spring (`@PreAuthorize`).
103. How to prevent **SSRF in Java** (Whitelisting IPs/Domains)?
104. What are the security risks of **Java Reflection**?
105. How to secure **Spring Boot Actuator** endpoints?
106. What is **JCA (Java Cryptography Architecture)**?
107. Explain **Java KeyStore (JKS)** vs. **PKCS12**.
108. How to handle **Security Manager** in older Java versions vs modern alternatives?
109. What is the impact of **Log4Shell** on the Java ecosystem?
110. How to implement **Custom UserDetailsService** securely?
111. Discuss **Session Management strategies** (Redis vs. In-memory).
112. How to prevent **LDAP Injection** in Java?
113. What is the difference between `AntMatchers` and `MvcMatchers` in Spring Security?
114. How to handle **Password Encoding** updates (Migration to stronger algorithms)?
115. Explain **Spring Security SAML** integration.
116. How to secure **RMI/JMX** interfaces?
117. What is **Java's Secure Coding Guidelines** from Oracle?
118. How to handle **Encryption in JPA/Hibernate** (Attribute Converters)?
119. What is **Spring Security ACL**?
120. How to implement **OAuth2 Resource Server** in Spring Boot?
121. Discuss **H2 Console security** in development.
122. How to prevent **Classloader manipulation** attacks?
123. What is **GCM mode** in AES and why use it over CBC?
124. How to handle **Large Payload/Zip Bomb** protection in Java?
125. Explain **CVE (Common Vulnerabilities and Exposures)** and how to track them.

## 6. Security Testing & Process (25 Questions)
126. What is **SAST (Static Application Security Testing)**?
127. What is **DAST (Dynamic Application Security Testing)**?
128. What is **IAST (Interactive Application Security Testing)**?
129. Explain **Fuzz Testing**.
130. What is **Threat Modeling** (STRIDE/PASTA)?
131. How to conduct a **Security Code Review**?
132. What is a **Bug Bounty** program?
133. Explain **Penetration Testing** phases.
134. What is **Red Teaming vs. Blue Teaming**?
135. Discuss **Vulnerability Management** lifecycle.
136. What is **SOC 2, ISO 27001, and HIPAA** compliance?
137. How to handle **Responsible Disclosure**?
138. What is **GDPR** and its impact on data security (Right to be Forgotten, Data Encryption)?
139. Explain **PCI-DSS** compliance for payment processing.
140. What is an **Incident Response Plan**?
141. How to perform **Post-Mortem** after a security breach?
142. What is **Static Analysis** for binaries?
143. Discuss **Third-party Risk Assessment**.
144. What is **Supply Chain Security** (Software Bill of Materials - SBOM)?
145. Explain **Content Disarm and Reconstruction (CDR)**.
146. How to measure **Security Maturity** (SAMM / BSIMM)?
147. What is **Security as Code**?
148. Discuss **Human Factors** in security (Phishing, Social Engineering).
149. How to design a **Secure Development Lifecycle (SDL)**?
150. Summary: What is the most important security lesson you've learned as a Senior Engineer?

---
*Generated by Antigravity for SDE3 Interview Prep.*
