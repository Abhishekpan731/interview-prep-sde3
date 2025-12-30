# 150 AWS Security, Identity & KMS Interview Questions (SDE3)

## 1. Advanced IAM (Identity & Access Management) (40 Questions)
1. What is the **IAM Policy Evaluation Logic**? Explain the hierarchy: Explicit Deny -> SCPs -> Resource-based Policies -> Identity-based Policies -> Permission Boundaries -> Implicit Deny.
2. What is the difference between **Identity-based Policies** and **Resource-based Policies**?
3. Explain **Service Control Policies (SCPs)**. How do they affect the Root user in a member account?
4. What are **Permission Boundaries**? Give a use case (e.g., delegating role creation to a developer without allowing privilege escalation).
5. Explain **Session Policies**. When are they used during `AssumeRole` operations?
6. What is the difference between **IAM Users, Groups, and Roles**?
7. Explain **Cross-account Access** using Roles. How does the "Trust Policy" differ from the "Permissions Policy"?
8. What is **External ID** in a Trust Policy? Why is it crucial to prevent the "Confused Deputy" problem?
9. Explain **Attribute-Based Access Control (ABAC)** vs. **Role-Based Access Control (RBAC)** in AWS.
10. How to implementation ABAC using **Tags** (`aws:PrincipalTag`, `aws:ResourceTag`)?
11. What is **IAM Access Analyzer**? How does it detect public or cross-account access?
12. Explain the **Principal** element in a policy. Who can be a principal?
13. What is **Action, Resource, and Condition** in a JSON policy?
14. Explain **aws:SourceIp** vs. **aws:SourceVpc** conditions.
15. What is the **Confused Deputy Problem** and how does `aws:SourceArn` or `aws:SourceAccount` mitigate it?
16. Explain **Inline Policies** vs. **Managed Policies** (AWS Managed vs. Customer Managed).
17. What is the **Maximum Policy Size**? How to handle very large policies?
18. Explain **IAM Roles for EC2 (Instance Profiles)**. How is the credential delivered to the instance?
19. What is the **IAM Credential Report**?
20. Explain **IAM Access Advisor**. How does it help in achieving "Least Privilege"?
21. What is **Web Identity Federation**? How does it differ from **SAML Federation**?
22. Explain **OIDC (OpenID Connect)** integration with IAM.
23. What is **AWS STS (Security Token Service)**? List its key methods: `AssumeRole`, `GetSessionToken`, `GetFederationToken`.
24. How to perform **Multi-Factor Authentication (MFA)** enforced access for specific IAM actions?
25. Explain the **Implicit Deny** principle.
26. What happens when there is an **Explicit Deny** in any policy?
27. How to manage **IAM for Lambda** (Execution Roles)?
28. Explain **IAM Roles for Service Accounts (IRSA)** in EKS.
29. What is the purpose of the **IAM Root User**? List 3 things only the Root user can do.
30. How to secure the Root user in an organization?
31. Explain **IAM Policy Versioning**.
32. What is **Policy Variables** (e.g., `${aws:username}`)?
33. How to implementation **Time-based access** in IAM?
34. Explain **IAM Access Analyzer for Policy Validation**.
35. What is **IAM Roles Anywhere**?
36. How to handle **API Key Rotation** for IAM users?
37. Explain the **Role Chain** limit.
38. What is **Service-Linked Roles**?
39. How to troubleshoot "Access Denied" errors? (Mentioning CloudTrail and Access Analyzer).
40. Explain **IAM Identity Center (successor to AWS SSO)**.

## 2. AWS KMS (Key Management Service) (40 Questions)
41. What is **AWS KMS**? How does it handle encryption at scale?
42. Explain **Customer Master Keys (CMKs)** (now called KMS Keys).
43. What is the difference between **AWS Managed Keys** vs. **Customer Managed Keys** vs. **AWS Owned Keys**?
44. Explain **Symmetric** vs. **Asymmetric** keys in KMS.
45. What is **Envelope Encryption**? Why is it used for large data?
46. Describe the workflow: **GenerateDataKey -> Encrypt with DEK -> Store ciphertexts**.
47. What is the **Plaintext Data Key** and why should it be deleted from memory immediately?
48. Explain **KMS Key Policies**. How do they differ from IAM policies?
49. What happens if a KMS Key Policy doesn't allow the Root user access? Can you fix it?
50. Explain **KMS Grants**. When are they used instead of Key Policies?
51. What is a **Grant Token**?
52. Explain **KMS Via Service** (e.g., `kms:ViaService` condition).
53. What is **Encryption Context**? How does it provide additional integrity and prevent tampering?
54. Explain **Key Rotation**: Automatic vs. Manual.
55. What is the frequency of **Automatic Key Rotation** for Customer Managed Keys?
56. Can you **Export** a private key from KMS?
57. What is **KMS Custom Key Store** (CloudHSM integration)?
58. Explain **FIPS 140-2 Level 2** vs. **Level 3** (KMS vs. CloudHSM).
59. What is **Multi-Region Keys**? How do they help in disaster recovery and global data flows?
60. Explain the **KMS Import Key Material** feature. What are the risks?
61. What is **Key Deletion**? Explain the "7 to 30 days" waiting period.
62. How to monitor KMS usage using **CloudTrail**?
63. What is **KMS Request Quotas** (TPS)? How to handle throttling in high-scale apps?
64. Explain **KMS Hierarchical Keys**.
65. What is **AWS Encryption SDK**?
66. Explain **SSE-S3 vs. SSE-KMS vs. SSE-C**.
67. How to handle **Cross-account KMS Access**?
68. What is the difference between **`kms:Encrypt`** and **`kms:GenerateDataKey`** permissions?
69. Explain **Asymmetric Key Sign/Verify** using KMS.
70. What is **KMS Key Aliases**?
71. How to implementation **Re-encryption** of data when a key is rotated?
72. What is **Ciphertext Portability**?
73. Explain the **Automatic Key Rotation for AWS Managed Keys**.
74. How to use **KMS with EBS Volumes**?
75. What is **KMS Default Policies**?
76. Explain **KMS External Key Store (XKS)**.
77. How to enforce **Encryption in Transit** using KMS? (Briefly mentioned in SSL context).
78. What is **Signing Keys** in KMS?
79. How to use **KMS with Lambda environment variables**?
80. Explain **KMS Audit Logs**.

## 3. Security Hub & Compliance (30 Questions)
81. What is **AWS Security Hub**?
82. Explain **Security Hub Findings**.
83. What are **Security Standards** (CIS, PCI-DSS, AWS Foundational Best Practices)?
84. How does Security Hub **aggregate** findings from multiple accounts and regions?
85. Explain **AWS Config** and its relationship with Security Hub.
86. What are **Insights** in Security Hub?
87. How to automate **Remediation** using Security Hub and EventBridge?
88. What is **Security Hub Cross-Region Aggregation**?
89. Explain **Security Hub Product Integrations** (e.g., GuardDuty, Inspector, Macie).
90. How to use **Severity Labels** in Security Hub?
91. What is **AWS Artifact**?
92. Explain **AWS Audit Manager**.
93. How to create **Custom Actions** in Security Hub?
94. Explain the **Consolidated Control Findings** feature.
95. What is **AWS Trusted Advisor** vs. Security Hub?
96. How to manage **Security Hub Administrator Account** (Delegated Admin)?
97. Explain **Workflow Status** (New, Notified, Resolved, Suppressed).
98. What is **Finding Provider**?
99. How to send **Custom Findings** to Security Hub?
100. Explain **ASFF (AWS Security Finding Format)**.
101. How to perform **Continuous Compliance Monitoring**?
102. What is the impact of **Disabled Controls** in Security Hub?
103. How to filter findings by **Resource Type** or **Tag**?
104. Explain **Security Lake** and its integration with Security Hub.
105. What is **AWS Health Dashboard** vs. Security Hub?
106. How to implementation **Governance at Scale** using Security Hub?
107. Explain **Security Hub Pricing Model**.
108. What is **Global View** in Security Hub?
109. How to handle **Suppression of False Positives**?
110. Role of **Security Hub in Incident Response**.

## 4. Threat Detection & Network Security (40 Questions)
111. What is **Amazon GuardDuty**?
112. Explain **GuardDuty Threat Intel** (Feeds).
113. What are **GuardDuty Finding Types** (EC2, IAM, S3, RDS, Lambda, EKS)?
114. How does GuardDuty use **Machine Learning** for anomaly detection (e.g., unusual port scanning)?
115. What is **Amazon Inspector**? Difference between Network Reachability and Host Vulnerability.
116. Explain **Inspector Agent-based vs. Agentless** scanning.
117. What is **Amazon Macie**? How does it use ML to discover PII?
118. Explain **Macie Managed Data Identifiers** vs. **Custom Identifiers**.
119. What is **AWS WAF (Web Application Firewall)**?
120. Explain **WAF Web ACLs, Rules, and Rule Groups**.
121. What is **WAF Managed Rules** (from AWS and Marketplaces)?
122. How to mitigate **SQL Injection and XSS** using WAF?
123. Explain **AWS Shield (Standard vs. Advanced)**. What does Advanced provide?
124. What is **AWS Firewall Manager**? How to centrally manage WAF/Shield across an Org?
125. Explain **AWS Network Firewall**. How does it differ from Security Groups/NACLs?
126. What is **Amazon VPC Lattice** security?
127. Explain **SSL Termination at ALB/NLB** vs. **End-to-end Encryption**.
128. What is **AWS PrivateLink** and how it improves security by not exposing traffic to the internet?
129. Explain **VPC Flow Logs** for security analysis.
130. What is **Traffic Mirroring**?
131. How to handle **DDoS Protection** for a cloud-native app?
132. Explain **Amazon Cognito** security features (Advanced Security: Bot detection, MFA).
133. What is **AWS Directory Service** (Managed AD)?
134. Explain **AWS RAM (Resource Access Manager)** for shareable resources security.
135. What is the **AWS Shared Responsibility Model**?
136. Explain **Compliance as Code**.
137. How to perform **Penetration Testing** on AWS? (Permissions needed?).
138. What is **AWS CloudTrail** (Management vs. Data events)?
139. Explain **CloudTrail Insights** for detecting unusual account activity.
140. Summary: As an SDE3, design a multi-layered security architecture for a FinTech app in AWS, incorporating IAM, KMS, WAF, Security Hub, and GuardDuty.

---
*Generated by Antigravity for SDE3 Interview Prep.*
