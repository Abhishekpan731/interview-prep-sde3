# 150 AWS EC2 Interview Questions (SDE3)

## 1. EC2 Fundamentals & Architecture (25 Questions)
1. What is **Amazon EC2 (Elastic Compute Cloud)**? Why is it considered the backbone of AWS IaaS?
2. Explain the difference between **Region** and **Availability Zone (AZ)** in the context of EC2.
3. What is an **AMI (Amazon Machine Image)**? What are its core components?
4. Explain **HVM (Hardware Virtual Machine)** vs. **PV (Paralell Virtualization)**. Why is HVM standard now?
5. What is the **Nitro System**? How does it improve performance and security compared to Xen?
6. Explain the **Instance Lifecycle**: Pending, Running, Stopping, Stopped, Shutting-down, and Terminated.
7. What is the difference between **Stopping** and **Terminating** an instance?
8. Explain **Instance Metadata** (IMDS v1 vs v2). How do you retrieve an instance's public IP using `curl`?
9. What is **Instance User Data**? How is it used for bootstrapping?
10. What is **EC2 Placement Groups**? Explain **Cluster, Partition, and Spread** placement.
11. Explain **Tenancy**: Shared vs. Dedicated Instances vs. Dedicated Hosts.
12. What are **EC2 Fleet** and **Spot Fleet**?
13. Explain the role of **Cloud-init** in EC2.
14. What is the **AWS Marketplace**?
15. Explain **Instance Types** naming convention (e.g., m5.large). What do the letters (m, c, r, g, i, z) stand for?
16. What is the difference between a **New Generation** vs. **Old Generation** instance?
17. Explain **Graviton** (ARM-based) instances. Why are they popular for SDE3s to consider?
18. What is **Enhanced Networking** (ENA)?
19. Explain **Elastic Fabric Adapter (EFA)** and its use in HPC.
20. What is **Elastic IP (EIP)**? When should you use it?
21. Explain the **Default VPC** and how EC2 interacts with it.
22. What is an **Enclave (AWS Nitro Enclaves)**?
23. Explain **Amazon DLM (Data Lifecycle Manager)**.
24. How does AWS handle **Hypervisor-level Security**?
25. What is **Local Zones** and **Wavelength** in the context of EC2?

## 2. Instance Types & Performance (20 Questions)
26. Explain **Burstable Performance Instances (T-series)** and the **CPU Credit** system.
27. What is **Unlimited Mode** for burstable instances?
28. Compare **Compute Optimized (C)** vs. **Memory Optimized (R)** vs. **Storage Optimized (I)** instances.
29. When would you choose **G (GPU)** or **P (Inf/Trn)** instances?
30. What is **High Memory** (12TB+) instances and their use cases?
31. How to benchmark EC2 performance correctly?
32. Explain **SR-IOV (Single Root I/O Virtualization)**.
33. What is **Intel AVX-512** support in EC2?
34. How does **EBS Optimization** affect instance performance?
35. What is the impact of **Steal Time** on EC2 performance metrics?
36. Explain **Cluster Networking** performance.
37. How to handle **PPS (Packets Per Second)** limits on smaller instance types?
38. What is **NVMe** storage on EC2 instances?
39. Explain **Instance Store (Ephemeral Storage)** vs. **EBS**.
40. What is the **Compute Optimizer** and how it helps in right-sizing?
41. How to monitor **GPU Utilization** on EC2?
42. Explain **Amazon Elastic Graphics**.
43. What is **FPGA (F1)** instances?
44. How to optimize **Memory Bandwidth** in high-scale EC2 applications?
45. Discuss the trade-offs of **Vertical vs. Horizontal Scaling** with EC2.

## 3. Storage: EBS & Instance Store (25 Questions)
46. What is **Amazon EBS (Elastic Block Store)**?
47. Compare **SSD-backed** (gp2, gp3, io1, io2) vs. **HDD-backed** (st1, sc1) volumes.
48. What is the difference between **gp2** and **gp3**? Why is gp3 preferred for decupling IOPS from size?
49. Explain **Provisioned IOPS (io1/io2)**. What is **Multi-Attach**?
50. What are **EBS Snapshots**? How do you perform an incremental backup?
51. Explain **Fast Snapshot Restore (FSR)**.
52. What is **EBS Encryption** (KMS)? Can you encrypt an existing unencrypted volume?
53. Explain **Snapshot Sharing** across accounts.
54. What is the **EBS Multi-Attach** feature? What are the filesystem requirements for it?
55. Explain **Instance Store (Ephemeral)**. What happens to data during a reboot vs. a stop/start?
56. When would you prefer **Instance Store** over EBS? (e.g., Lustre, Redis, Temp files).
57. What is **TRIM** support in EBS?
58. Explain **Volume Initialization (Pre-warming)** for EBS volumes.
59. What is **Thin Provisioning** in the context of EBS?
60. How to increase the size of an **EBS Volume** without downtime?
61. Explain **Elastic Volumes** feature.
62. What is the difference between **DeleteOnTermination** attribute?
63. Explain **Cold HDD (sc1)** vs. **Throughput Optimized HDD (st1)**.
64. How to handle **Stale Snapshots** and cost management?
65. Explain **EBS Performance Bursting** (Bucket-and-token logic).
66. What is **Amazon EFS (Elastic File System)** vs. **Amazon FSx**?
67. How to mount multiple EBS volumes to a single instance?
68. What is **RAID 0, 1, 10** on EC2? When to use software RAID?
69. Explain **Block Express** for io2 volumes.
70. How to troubleshoot **High Disk Latency** on EBS?

## 4. Networking & VPC Integration (25 Questions)
71. What is an **Elastic Network Interface (ENI)**?
72. How do you attach **Multiple ENIs** to an instance? (Multi-homing).
73. Explain **Public IP** vs. **Private IP** vs. **Elastic IP**.
74. How does EC2 handle **DNS** resolution?
75. What is the **VPC Peering** impact on EC2 latency?
76. Explain **Security Groups**. Why are they "Stateful"?
77. Explain **Network ACLs (NACLs)**. Why are they "Stateless"?
78. What is the impact of **MTU (Maximum Transmission Unit)** on EC2 networking? (Standard 1500 vs. Jumbo 9001).
79. Explain **VPC Endpoints (Interface vs. Gateway)**. How to access S3 from EC2 privately?
80. What is a **NAT Gateway**? How to allow outbound-only internet access for EC2 in private subnets?
81. Explain **Egress-only Internet Gateway**.
82. What is **AWS Transit Gateway**?
83. Explain **Direct Connect** vs. **VPN** for EC2 connectivity.
84. What is **Global Accelerator** for EC2?
85. How to perform **TCP Tuning** on an EC2 instance for high-throughput web services?
86. Explain **Flow Logs**. How to troubleshoot "Connection Timeout" using Flow Logs?
87. What is **VPC Ingress Routing**?
88. Explain **BYOIP (Bring Your Own IP)** to EC2.
89. How to handle **IP Exhaustion** in a large scale EC2 fleet?
90. What is **Reachability Analyzer**?
91. Explain **Network Load Balancer (NLB)** vs. **Application Load Balancer (ALB)** for EC2 targets.
92. How does **Cross-Zone Load Balancing** work?
93. What is **GWLB (Gateway Load Balancer)** for firewalls?
94. Explain **Sticky Sessions (Target Group Level)**.
95. How to handle **Health Checks** for EC2 instances behind a Load Balancer?

## 5. Security & IAM (15 Questions)
96. Explain **IAM Roles for EC2**. Why is it better than using IAM User Access Keys?
97. What is the **Instance Profile**?
98. How does the EC2 instance retrieve **Temporary Credentials** from the metadata service?
99. Explain **EC2 Key Pairs**. What happens if you lose the `.pem` file?
100. What is **AWS Systems Manager (SSM) Session Manager**? Why is it safer than opening SSH (Port 22)?
101. Explain **EC2 Image Builder**. How to create hardened AMIs?
102. What is **AWS Inspector** for EC2?
103. How to implement **Disk Encryption at Rest** for existing instances?
104. Explain **Security Group Rules** logic (Allow-only, no Deny rules).
105. What is the impact of **Nested Security Groups**?
106. Explain **Permission Boundaries** for EC2 administrators.
107. How to secure **Instance Metadata Service (IMDSv2)** only?
108. What is **Amazon GuardDuty** for EC2?
109. How to perform **Forensics** on a compromised EC2 instance?
110. Explain **WAF** integration with ALBs facing EC2.

## 6. High Availability & Auto Scaling (20 Questions)
111. What is an **Auto Scaling Group (ASG)**?
112. Explain **Launch Configuration** vs. **Launch Template**. Why is the template preferred?
113. What are **Scaling Policies**: Target Tracking, Step Scaling, and Simple Scaling?
114. Explain **Predictive Scaling**.
115. What is the **Cooldown Period** and **Warm-up Time**?
116. How does **ASG Termination Policy** work? How to protect an instance from scale-in?
117. Explain **Self-Healing** properties of an ASG.
118. What is a **Health Check Grace Period**?
119. Explain **Multi-AZ Deployment** for EC2.
120. How to perform a **Rolling Update** of an ASG fleet?
121. What is **Instant Refresh** in ASG?
122. Explain **Scheduled Scaling**.
123. How to use **Spot Instances in an ASG**? What is a **Mixed Instances Policy**?
124. What is the **Spot Instance Interruption Notice**? How to handle it cleanly?
125. Explain **Capacity Reservations** (On-Demand Capacity Reservations).
126. What is **Reserved Instances (RI)** vs. **Savings Plans**?
127. Explain **Zonal vs. Regional Reserved Instances**.
128. What is **Convertible RI**?
129. How to handle **Database (Stateful) Auto Scaling** with EC2?
130. What is **Lifecycle Hooks** in ASG?

## 7. Operations & Optimization (20 Questions)
131. How to **Migrate an EC2 instance** from one AWS account to another?
132. How to **Move an EC2 instance** from one Region to another?
133. Explain **CloudWatch Metrics** for EC2: CPU, Disk Read/Write, Network In/Out. (Why is RAM missing?).
134. How to monitor **Memory (RAM) Utilization**? (CloudWatch Agent).
135. What is **Detailed Monitoring** (1-minute intervals)?
136. Explain **Custom Metrics** for EC2.
137. How to automate **Instance Start/Stop** to save costs? (Instance Scheduler).
138. What is **VPC Flow Logs** analytics?
139. Explain **Resource Tags**. Why are they crucial for cost allocation?
140. How to troubleshoot an **Instance that is not reachable via SSH**?
141. What is the **System Status Check** vs. **Instance Status Check**?
142. How to recover an instance from a **System Check Failure**?
143. Explain **Instance Console Output** and **Screenshot** feature.
144. What is **AWS Config** for EC2?
145. How to implement **Patch Management** with SSM Patch Manager?
146. Explain **AWS X-Ray** for applications running on EC2.
147. What is **EC2 Launch** (for Windows)?
148. How to handle **Time Synchronization** (Amazon Time Sync Service)?
149. Explain **Capacity Rebalancing** in ASG.
150. Summary: As an SDE3, walk through your strategy for migrating a monolith from on-premise to a highly available, cost-optimized EC2 architecture.

---
*Generated by Antigravity for SDE3 Interview Prep.*
