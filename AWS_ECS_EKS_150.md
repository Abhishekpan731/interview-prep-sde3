# 150 AWS ECS & EKS Interview Questions (SDE3)

## 1. ECS (Elastic Container Service) Fundamentals (20 Questions)
1. What is **Amazon ECS**? How does it differ from a standard Kubernetes environment?
2. Explain the core components: **Cluster, Task Definition, Task, and Service**.
3. What is a **Task Definition**? Describe the parameters like `networkMode`, `cpu`, and `memory`.
4. Explain the difference between **Launch Types**: **EC2 vs. Fargate**.
5. What are **ECS Container Instances**?
6. Describe the role of the **ECS Agent**.
7. What is an **ECS Cluster**? Does it provide any compute by default?
8. Explain **Task Scheduling** in ECS (Replica vs. Daemon strategies).
9. What is the **Task Placement Strategy** (Binpack, Random, Spread)?
10. What is **Task Placement Constraint** (distinctInstance, memberOf)?
11. Explain **Service Auto Scaling** in ECS.
12. What is the **ECS Capacity Provider**? How does it manage the scaling of ASGs automatically?
13. Explain **Fargate Capacity Providers**.
14. What is **ECS Managed Scaling**?
15. How does ECS handle **Service Discovery** (AWS Cloud Map)?
16. Explain **Task Role** vs. **Task Execution Role**.
17. What is **ECS Anywhere**?
18. How to perform **Blue/Green Deployments** in ECS using AWS CodeDeploy?
19. Explain **Rolling Updates** in ECS (Minimum healthy percent, Maximum percent).
20. What is **Amazon ECS optimized AMI**?

## 2. EKS (Elastic Kubernetes Service) Fundamentals (20 Questions)
21. What is **Amazon EKS**? How does it manage the Kubernetes Control Plane?
22. Explain the **EKS Control Plane** architecture across multiple AZs.
23. What are **EKS Managed Node Groups**?
24. Explain **Self-managed Nodes** in EKS.
25. What is **EKS Fargate**? What are the limitations of running Kubernetes pods on Fargate?
26. How to create an EKS cluster using `eksctl` vs. Terraform?
27. What is the **VPC CNI Plugin** for Kubernetes? Why is it unique to EKS?
28. Explain **EKS Add-ons**.
29. What is the role of the **aws-auth ConfigMap** in EKS?
30. Explain **EKS Pod Identity** (The successor to IRSA).
31. What is **Karpenter** and why is it preferred over Cluster Autoscaler for EKS?
32. Explain **Horizontal Pod Autoscaler (HPA)** vs. **Vertical Pod Autoscaler (VPA)** in EKS.
33. What is the **EKS Optimized Bottlerocket OS**?
34. Explain **EKS Distro** and **EKS Anywhere**.
35. How does EKS handle **Control Plane Logging**?
36. What is the **Amazon EKS Connector**?
37. Explain **Service Accounts** and their binding to IAM Roles.
38. What is **CoreDNS** in the context of EKS?
39. How to upgrade an EKS cluster version with zero downtime?
40. Explain **Kube-proxy** role in EKS.

## 3. Container Networking (20 Questions)
41. Explain **awsvpc Network Mode** in ECS. Why is it the recommended mode?
42. What is the **ENI Trunking** feature for ECS?
43. Compare **awsvpc, bridge, and host** network modes in ECS.
44. How does the **Amazon VPC CNI** assign VPC IP addresses to EKS Pods?
45. What is **IP Exhaustion** in EKS development and how to mitigate it (Custom Networking)?
46. Explain **Security Groups for Pods** (EKS).
47. How to implement **Network Policies** in EKS (using Calico or VPC CNI)?
48. Explain the integration of **Application Load Balancer (ALB)** with ECS (Target Groups).
49. What is the **AWS Load Balancer Controller** for EKS?
50. Explain **IP-mode** vs. **Instance-mode** for Load Balancers in EKS.
51. What is **TargetGroupBinding** in EKS?
52. How does **Internal Service Discovery** work in ECS vs. EKS?
53. Explain **App Mesh** (Istio/Envoy) integration with ECS and EKS.
54. What is **VPC Peering** or **Transit Gateway** impact on container communication?
55. Explain **PrivateLink** support for ECS/EKS control plane access.
56. How to handle **External Traffic Policy** in EKS?
57. What is **NodePort** vs. **LoadBalancer** service type in EKS?
58. Explain **Ingress Class** and **Ingress Controller** in EKS.
59. How to achieve **mTLS** between containers in AWS?
60. Explain the **Default Gateway** for containers in Fargate.

## 4. Storage & Persistence (15 Questions)
61. How to use **Amazon EFS** with ECS Tasks?
62. How to use **Amazon EBS** with ECS (recently announced EBS integration for ECS)?
63. Explain **Bind Mounts** and **Docker Volumes** in ECS.
64. What is **EKS Container Storage Interface (CSI)**?
65. How to use **EBS CSI Driver** in EKS?
66. Explain **Dynamic Provisioning** of volumes in EKS using StorageClasses.
67. How to use **Amazon FSx for Lustre/NetApp ONTAP** with EKS?
68. What is **Ephemeral Storage** in Fargate?
69. Explain **Volume Snapshots** in Kubernetes.
70. How to handle **StatefulSets** in EKS across different Availability Zones?
71. What is the **EFS CSI Driver** for EKS?
72. Explain **PersistentVolume (PV)** and **PersistentVolumeClaim (PVC)**.
73. How to share data between containers in the **Same Task/Pod**?
74. What is the **ReadWriteMany (RWX)** limitation with EBS and how to solve it?
75. Explain **Storage Quotas** in containerized environments.

## 5. Security & IAM (20 Questions)
76. Explain **IAM Roles for Service Accounts (IRSA)** in EKS. How does OIDC work here?
77. What is the difference between **ECS Task Role** and **Task Execution Role**?
78. How to use **AWS Secrets Manager** to inject secrets into ECS containers?
79. What is **External Secrets Operator** for EKS?
80. Explain **Pod Security Admission** in EKS.
81. How to restrict **Container Root Access**?
82. What is **Amazon ECR (Elastic Container Registry)**? Explain **Image Scanning**.
83. Explain **Pull-through Cache** in ECR.
84. How to perform **Image Signing** using AWS Signer for EKS?
85. What is **GuardDuty** for EKS and ECS?
86. Explain **Runtime Security Monitoring** (Falco/Sysdig) in EKS.
87. How to handle **Cross-account ECR Access**?
88. What is **IAM OIDC Provider** for EKS?
89. Explain **EKS Cluster Access Management** (API-based instead of configmap).
90. How to secure the **Kube-apiserver** endpoint (Public vs Private)?
91. What is **AWS Shield** and **WAF** integration with containerized apps?
92. Explain **Vulnerability Management** in a CI/CD pipeline for containers.
93. How to use **KMS** for encrypting EKS secrets in Etcd?
94. What is **Privileged Mode** in ECS and why is it blocked in Fargate?
95. Explain **Audit Logging** for EKS.

## 6. Scaling & High Availability (20 Questions)
196. Explain **Step Scaling** vs. **Target Tracking** in ECS.
97. How does **Cluster Autoscaler** work in EKS?
98. What is **Karpenter**? How does it optimize costs by picking right-sized instances?
99. Explain **HPA (Horizontal Pod Autoscaler)** metrics (CPU, Memory, Custom metrics).
100. How to use **KEDA** (Kubernetes Event-driven Autoscaling) on EKS?
101. What is the **Task Startup Latency** in Fargate vs EC2?
102. Explain **Multi-AZ Deployment** best practices for EKS.
103. What is **Topology Spread Constraints** in K8s?
104. How to handle **Spot Instances** in ECS and EKS?
105. Explain the **Termination Notice** for Spot instances and how the orchestrator handles it.
106. What is **Over-provisioning** in Kubernetes?
107. Explain **Readiness and Liveness Probes** in EKS vs **Health Checks** in ECS.
108. How to perform a **Canary Release** using Flagger or Istio on EKS?
109. What is **AWS App Mesh** traffic shifting?
110. Explain **Service Mesh** role in High Availability.
111. How to scale the **ECS Control Plane**? (It's abstract, but what are the limits?).
112. Explain **Cluster Capacity Providers** for ASG scaling.
113. What is **Graceful Shutdown** of tasks/pods?
114. How to handle **Thundering Herd** during mass container restarts?
115. Explain **Cross-Region Replication** for containerized apps.

## 7. Observability & Troubleshooting (20 Questions)
116. What is **AWS CloudWatch Container Insights**?
117. Explain **Fluent Bit** vs. **CloudWatch Logs Agent** for container logging.
118. How to use **AWS Distro for OpenTelemetry (ADOT)**?
119. Explain **X-Ray** tracing for microservices in ECS/EKS.
120. How to troubleshoot **CrashLoopBackOff** in EKS?
121. "Task stopped: Essential container in task exited." How to debug this in ECS?
122. Explain **ECS Exec**. Why is it better than SSH?
123. What is **kubectl debug** for pods?
124. How to monitor **Prometheus Metrics** on EKS (AMP - Amazon Managed Prometheus)?
125. What is **Amazon Managed Grafana** integration?
126. Explain **Control Plane Metrics** in EKS.
127. How to troubleshoot **Pending Pods** in EKS?
128. Explain **Target Group Health Check** failures.
129. How to check **Container Resource Usage** (CPU/RAM) in Fargate?
130. What is **AWS Health Dashboard** role in cluster maintenance?
131. How to capture **Heap Dumps** from a container in EKS?
132. Explain **Log Aggregation** strategies (EFK stack).
133. How to detect **Memory Leaks** in containerized environments?
134. What is **Sidecar container logging**?
135. Explain **Cost Observability** (KubeCost) in EKS.

## 8. Advanced & Real-world Scenarios (15 Questions)
136. "ECS vs EKS." When would you recommend one over the other for a startup vs enterprise?
137. Design a **CI/CD Pipeline** for a Java Spring Boot app to EKS using ArgoCD (GitOps).
138. How to implementation **Multi-tenancy** in EKS (Namespaces vs Clusters)?
139. Explain **VPC Lattice** and its impact on container networking.
140. How to migrate a **Monolith on EC2** to **Microservices on ECS Fargate**?
141. What is the **Operator Pattern** in EKS?
142. Explain **Kubebuilder** or **Operator SDK**.
143. How to run **GPU Workloads** on EKS for AI/ML?
144. What is the impact of **Graviton Instances** on container cost-performance?
145. Explain **Serverless Containers** (Fargate) vs **Serverless Functions** (Lambda).
146. How to handle **Database Migrations** in a containerized rollout?
147. Design a **Disaster Recovery** strategy for an EKS-based application.
148. What is **Service Mesh Hub**?
149. Explain **Cluster API (CAPI)**.
150. Summary: What is the most challenging production issue you've faced with ECS or EKS, and how did you resolve it?

---
*Generated by Antigravity for SDE3 Interview Prep.*
