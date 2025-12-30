# 150 AWS Lambda Interview Questions (SDE3)

## 1. Fundamentals & Execution Model (25 Questions)
1. What is **AWS Lambda**? How does the "Serverless" model differ from traditional EC2-based hosting?
2. Explain the **Event-driven** nature of Lambda.
3. What is a **Lambda Function**? What are the core components (Handler, Event Object, Context Object)?
4. Describe the **Lambda Execution Lifecycle**: Init, Invoke, and Shutdown phases.
5. What is the **Execution Environment (Sandbox)**? How is it isolated?
6. Explain the concept of **Firecracker MicroVMs**.
7. What is the **Handler** method in Java? Describe the input and output types.
8. Explain the **Context Object**. What information can you retrieve from it (e.g., Request ID, Remaining Time)?
9. What are the **Supported Runtimes**? How do you use a **Custom Runtime**?
10. Explain **Statelessness** in Lambda. Why is it important?
11. How does Lambda handle **Concurrency**? What is the difference between **Reserved Concurrency** and **Provisioned Concurrency**?
12. What is the **Default Concurrency Limit** per region?
13. Explain **Synchronous** vs. **Asynchronous** invocation.
14. What is **Event Source Mapping (ESM)**?
15. Explain the **Retry Behavior** for synchronous vs. asynchronous vs. stream-based invocations.
16. What are **Dead Letter Queues (DLQ)** in Lambda? How do they differ from **Lambda Destinations**?
17. Explain **Lambda Destinations**. Which invoke types support them?
18. What is the maximum **Execution Timeout** for a Lambda function?
19. What is the maximum **Deployment Package Size** (zipped vs. unzipped)?
20. Explain the `/tmp` directory. What is its size limit and persistence?
21. What are **Lambda Layers**? How do they help in code reuse and dependency management?
22. How many Layers can you attach to a single function?
23. What is the difference between **Zip-based** and **Container-based** Lambda deployments?
24. Explain **Function URL**. When would you use it instead of API Gateway?
25. What is **Lambda@Edge**?

## 2. Performance: Cold Starts & Optimization (25 Questions)
26. What is a **Cold Start**? Why does it happen?
27. Explain **Warm Start**. How does Lambda reuse execution environments?
28. What factors influence **Cold Start Latency**? (Runtime, Memory, VPC, Package Size).
29. How does **Provisioned Concurrency** eliminate cold starts?
30. Explain **SnapStart** for Java. How does it work (Checkpoint, Restore)?
31. What are the requirements for enabling **SnapStart**?
32. How to handle **State/Randomness** issues when using SnapStart?
33. Describe the relationship between **Memory** and **CPU/Network** in Lambda.
34. How to use **AWS Lambda Power Tuning** to find the optimal memory setting?
35. What is the impact of **Package Size** on cold starts?
36. Explain **Lazy Loading** of dependencies vs. **Static Initialization**.
37. Why is Java typically slower for cold starts compared to Node.js or Python?
38. How does **GraalVM Native Image** help with Java Lambda performance?
39. What is **Tiered Compilation** in the JVM and how it affects Lambda?
40. Explain the **Keep-Alive** strategy for downstream connections in Lambda.
41. How to optimize **Database Connection Pooling** in a serverless environment? (RDS Proxy).
42. What is the impact of **Logging** on Lambda execution time?
43. Explain **Execution Environment Reuse** and the risk of variable leakage.
44. How to handle **Initial Handshake** overhead (e.g., KMS, AppConfig) in the Init phase?
45. What is **Asynchronous Processing** within a single Lambda execution?
46. Explain **HTTP/2** and **gRPC** support in Lambda.
47. What is **Compute Optimizer** for Lambda?
48. How to handle **Large Payloads** (>6MB) for Lambda?
49. What is **Recursive Invocation** and how to prevent it?
50. Explain **Burstable Concurrency** and its relationship with quotas.

## 3. Networking & Security (25 Questions)
51. Explain **Lambda in a VPC**. Why was it slow previously and how did **Hyperplane (PrivateLink)** fix it?
52. What is an **Elastic Network Interface (ENI)** in the context of Lambda?
53. Does a Lambda in a VPC have **Public Internet Access** by default? How to enable it?
54. Explain **Security Groups** for Lambda.
55. What is **IAM Role for Lambda (Execution Role)** vs. **Resource-based Policy**?
56. How to follow the **Least Privilege Principle** for a Lambda function?
57. Explain **Environment Variables Encryption** using KMS.
58. What is the **AWS Lambda Environment Variable** size limit?
59. How to securely manage **Secrets** (e.g., DB passwords) in Lambda? (Secrets Manager vs. Parameter Store).
60. What is **Credential Caching**?
61. Explain **PrivateLink** for Lambda (Interface VPC Endpoints).
62. How to prevent **Cross-account access** to Lambda functions?
63. What is **VPC Endpoints (Gateway vs Interface)** used by Lambda to access S3/DynamoDB?
64. Explain **MTLS** support for Lambda.
65. How to implementation **Outbound Proxy** for Lambda?
66. What is **Lambda Authorizer** for API Gateway?
67. Explain **Token-based** vs. **Request-based** authorizers.
68. How to perform **Static Analysis** on Lambda code for security?
69. What is **Amazon Inspector** for Lambda?
70. Explain **Code Signing** for Lambda.
71. How to monitor **Unauthorized Access** attempts?
72. What is **VPC Flow Logs** for Lambda?
73. How to handle **IP Exhaustion** in a VPC subnet dedicated to Lambda?
74. Explain **Runtime Security** for Lambda.
75. What is **WAF** integration with Lambda Function URLs?

## 4. Triggers & Event Source Integrations (25 Questions)
76. Explain **S3 Triggers**. How to handle duplicate events?
77. Explain **DynamoDB Streams** integration. What is the **Batch Size** and **Batch Window**?
78. What is **Kinesis Data Streams** integration? Explain **Parallelization Factor**.
79. How does Lambda handle **SQS** as a trigger? (Polling vs. Pushing).
80. Explain **SQS FIFO** queues and Lambda. What is the **Message Group ID** impact?
81. What is the **Scaling behavior** for SQS-Lambda integration?
82. Explain **SNS** as a trigger. Is it synchronous?
83. How to use **EventBridge (CloudWatch Events)** to schedule Lambda?
84. Explain **API Gateway (REST vs. HTTP API)** as a trigger.
85. What is **Proxy Integration** vs. **Non-proxy Integration**?
86. How to handle **WebSocket** APIs with Lambda?
87. Explain **Cognito Triggers** (Post-confirmation, Pre-token generation).
88. What is **ALB (Application Load Balancer)** as a Lambda trigger?
89. Explain **MSK (Managed Streaming for Kafka)** as a trigger.
90. How to handle **Partial Batch Failures** in SQS/Kinesis (Bisect on Batch Errror)?
91. What is **Self-managed Kafka** as a trigger?
92. Explain **Step Functions** integration. Why use Step Functions for long-running workflows?
93. What is **AppSync** (GraphQL) as a trigger?
94. Explain **EventBridge Pipes**.
95. How does **Filtering** work at the Event Source level (to save Lambda invocations)?
96. Explain **CloudWatch Logs** as a trigger (Subscription Filters).
97. What is **IoT Core** trigger?
98. Explain **Custom Event Sources**.
99. How to handle **Eventual Consistency** when a Lambda is triggered by S3?
100. Discuss the **Payload Limit** differences across triggers (SQS vs. Kinesis vs. API Gateway).

## 5. Deployment, Versions & Aliases (20 Questions)
101. What is a **Lambda Version**? Are versions mutable?
102. What is a **Lambda Alias**? How is it used for **Blue-Green Deployments**?
103. Explain **Traffic Shifting (Canary Deployment)** using Aliases.
104. What is **AWS SAM (Serverless Application Model)**?
105. How does **AWS CDK** handle Lambda deployments?
106. Explain **$LATEST** version.
107. What is **Weighted Aliases**?
108. How to perform **Rollbacks** in a serverless environment?
109. Explain **CloudFormation** deployment for Lambda.
110. What is **Serverless Framework** vs. **AWS SAM**?
111. How to automate **Environment-specific** configurations?
112. What is **Cross-Region** replication of Lambda functions?
113. Explain **Lambda Container Images**. How do you optimize the image size?
114. What are the **Reserved Environment Variables** (e.g., `_HANDLER`, `AWS_REGION`)?
115. How to handle **Hotfixes** in production without affecting current traffic?
116. What is **CodeDeploy**'s role in Lambda deployments?
117. Explain **Linear vs. Canary** deployment patterns in SAM.
118. How to manage **Large Teams** working on the same Lambda repository?
119. What is **Local Testing** for Lambda? (SAM Local).
120. Explain **CI/CD Pipelines** for Lambda (GitHub Actions/CodePipeline).

## 6. Observability & Troubleshooting (20 Questions)
121. How does **CloudWatch Logs** work with Lambda? How are Log Groups named?
122. Explain **CloudWatch Metrics**: Invocations, Errors, DeadLetterErrors, Duration, Throttles.
123. How to track **Concurrent Executions** via metrics?
124. What is **AWS X-Ray**? How do you trace a request across Lambda and DynamoDB?
125. Explain the **X-Ray Daemon** in the Lambda environment.
126. What is **CloudWatch Insights** for high-scale log searching?
127. How to troubleshoot **Throttling** issues?
128. What does a **429 Too Many Requests** error mean in Lambda?
129. How to debug **Timeout** errors?
130. Explain **Permissions errors** in Lambda.
131. What is the **Impact of Log Verbosity** on CloudWatch costs?
132. How to capture **Custom Metrics** from Lambda?
133. Explain **Distributed Tracing** for asynchronous event chains.
134. What are **Lambda Extensions**? (e.g., Datadog, New Relic extensions).
135. Explain **Internal vs. External Extensions**.
136. How to debug **Memory Leaks** in long-lived execution environments?
137. What is **Telemetry API** in Lambda?
138. How to monitor **Cold Start Frequency**?
139. Explain **Real-time Log Streaming**.
140. How to handle **Log Aggregation** from thousands of Lambda functions?

## 7. Advanced Scenarios & Design (10 Questions)
141. "My Lambda works locally but fails in AWS." Walk me through the diagnostic steps.
142. Design a **Saga Pattern** using Lambda and Step Functions.
143. How to implementation **Idempotency** in a Lambda function triggered by an SQS queue? (PowerTools).
144. What is **Orchestration vs. Choreography** in a serverless microservices architecture?
145. Compare **Lambda vs. Fargate** for long-running or CPU-intensive tasks.
146. How to design a **High-throughput API** with Lambda and Redis?
147. Discuss the cost implications of **Lambda vs. EC2** at a steady state of 1000 requests/sec.
148. How to handle **Compliance (PCI-DSS/HIPAA)** in a Lambda environment?
149. Explain **Serverless Architecture Patterns** (The Strangler Fig, Event-Sourcing).
150. Summary: What is the most challenging Lambda-based system you've built, and how did you overcome the platform's limitations (e.g., timeouts, state)?

---
*Generated by Antigravity for SDE3 Interview Prep.*
