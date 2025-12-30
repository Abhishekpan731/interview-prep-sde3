# 150 AWS Storage & Data Warehousing Interview Questions (SDE3)

## 1. Amazon S3 (Simple Storage Service) (40 Questions)
1. What is **Amazon S3**? Explain the concept of **Buckets and Objects**.
2. Explain the **S3 Data Consistency Model**. How does it handle read-after-write for new objects and updates?
3. List and explain the **S3 Storage Classes**: Standard, Intelligent-Tiering, Standard-IA, One Zone-IA, Glacier Instant Retrieval, Flexible Retrieval, and Deep Archive.
4. When should you use **S3 Intelligent-Tiering**? How does it save costs?
5. Explain **S3 Versioning**. How does it protect against accidental deletions?
6. What is a **Delete Marker**?
7. Explain **S3 Lifecycle Management**. How do you automate transitions and expirations?
8. What is **S3 Object Lock**? Explain **Governance Mode vs. Compliance Mode**.
9. Explain **S3 Replication**: Cross-Region (CRR) vs. Same-Region (SRR).
10. What is **S3 Batch Operations**?
11. Explain **S3 Inventory**.
12. What are the three ways to secure an S3 bucket? (**IAM Policies, Bucket Policies, and Access Control Lists (ACLs)**).
13. Explain **S3 Block Public Access**.
14. How to perform **S3 Encryption at Rest**? (SSE-S3, SSE-KMS, SSE-C).
15. Explain **S3 Encryption in Transit** (TLS/SSL).
16. What is **S3 Select**? How does it improve performance and cost for large datasets?
17. Explain **S3 Event Notifications**. Which AWS services can they trigger?
18. What is **S3 Transfer Acceleration**?
19. Explain **S3 Multi-part Upload**. Why is it mandatory for objects > 5GB?
20. What is the **S3 Object Key naming convention** best practice for high-performance requests (Prefixes)?
21. Explain **S3 Access Points**. How do they simplify data access for multi-tenant applications?
22. What is **S3 Multi-Region Access Points**?
23. Explain **S3 Object Tagging** vs. **Metadata**.
24. How to host a **Static Website** on S3?
25. What is the impact of **High Request Rates** on S3 (3,500 PUT/5,500 GET per second per prefix)?
26. Explain **S3 Storage Lens**.
27. What is **S3 Presigned URLs**? How to use them for secure temporary access?
28. How does **S3 Replication Time Control (RTC)** work?
29. Explain **S3 Storage Class Analysis**.
30. What is **S3 Byte-Range Fetches**?
31. How to perform **Cross-Account S3 Access**?
32. Explain **VPC Gateway Endpoints for S3**.
33. What is **S3 Outposts**?
34. Explain the difference between **Bucket ACLs** and **Object ACLs**.
35. How to implementation **Cost Allocation Tags** for S3 storage?
36. What is **S3 Object Lambda**? How to transform data as it is retrieved from S3?
37. Explain **S3 Glacier Select**.
38. How to perform a **Zero-Downtime Migration** of TBs of data to S3 (Snowball vs. DataSync)?
39. What is **S3 Express One Zone** (The new high-performance class)?
40. Discuss **Data Sovereignty** issues in S3.

## 2. Block & File Storage (EBS, EFS, FSx) (35 Questions)
41. Compare **Block Storage (EBS)** vs. **File Storage (EFS/FSx)** vs. **Object Storage (S3)**.
42. What is **Amazon EFS (Elastic File System)**? When to choose EFS over EBS?
43. Explain **EFS Performance Modes**: General Purpose vs. Max I/O.
44. Explain **EFS Throughput Modes**: Bursting, Provisioned, and Elastic.
45. How does **EFS Lifecycle Management** work?
46. What is **EFS Replication** across regions?
47. What is **Amazon FSx for Windows File Server**?
48. Explain **Amazon FSx for Lustre**. Why is it used for HPC and Machine Learning?
49. What is **Amazon FSx for NetApp ONTAP**?
50. What is **Amazon FSx for OpenZFS**?
51. How to mount an **EFS Volume** on multiple EC2 instances or Lambda functions?
52. What is the impact of **Network Latency** on EFS performance?
53. Explain **File Level Permissions** in EFS/FSx (POSIX/NFS).
54. What is **Amazon Backup** (Service for centralizing storage backups)?
55. Explain **EBS Volume Types** (recap but focusing on file-system aspects): gp3, io2, st1.
56. What is **EBS Multi-Attach** and which filesystems (like GFS2 or OCFS2) are required?
57. Explain **EBS Fast Snapshot Restore**.
58. What is **EBS direct APIs**?
59. How to handle **Database persistence** on EFS vs. EBS?
60. What is **Data Persistence** in FSx for Lustre (Scratch vs. Persistent)?
61. Explain **Storage Tiering** in FSx for NetApp ONTAP.
62. How to securely connect to EFS/FSx from **On-premise**?
63. What is **AWS Storage Gateway**? Explain File Gateway, Tape Gateway, and Volume Gateway.
64. How to use **S3 File Gateway** to provide an NFS/SMB interface to S3?
65. Explain **Amazon DataSync** for heavy storage migrations.
66. What is **Amazon Transfer Family** (SFTP, AS2, FTPS)?
67. How to monitor **Burst Credits** in EFS?
68. Explain **Mount Targets** and **Access Points** in EFS.
69. What is the maximum size of a single file in S3 vs. EFS?
70. How many instances can share an EBS volume vs. an EFS system?
71. What is **AWS Elastic Disaster Recovery (DRS)**?
72. How to handle **Quotas and Limits** in EFS?
73. Explain **Zonal vs Regional EFS**.
74. What is **Storage-optimized EC2** instances (I3/I4) and their relationship with local NVMe?
75. Compare **Local Instance Store** vs. **EBS** file persistence.

## 3. Data Warehousing: Amazon Redshift (40 Questions)
76. What is **Amazon Redshift**? How does it differ from a standard OLTP database (like RDS)?
77. Explain **OLAP (Online Analytical Processing)** vs. **OLTP**.
78. Describe the **Redshift Architecture**: Leader Node and Compute Nodes.
79. What are **Slices** in a Redshift compute node?
80. Explain **Columnar Storage**. Why is it superior for analytical queries?
81. What is **Data Compression** in Redshift? How does it reduce I/O?
82. Explain **Distribution Keys (Distkeys)**: Auto, Even, Key, and All.
83. How to choose the right **Distribution Key** to avoid data skew and minimize shuffles?
84. Explain **Sort Keys**: Compound vs. Interleaved.
85. What is the difference between **Compound Sort Keys** and **Interleaved Sort Keys**?
86. Explain **Zone Maps**. How do they skip irrelevant data blocks?
87. What is **Redshift Spectrum**? How to query data directly from S3 without loading it?
88. Explain **Redshift Serverless**.
89. What is **Redshift Managed Storage (RMS)**?
90. Explain **RA3 Instance Types** and compute-storage separation.
91. What is **Concurrency Scaling**?
92. Explain **Workload Management (WLM)**: Manual vs. Automatic.
93. What is **Query Queues** and **SLA-based priority**?
94. Explain **Short Query Acceleration (SQA)**.
95. What is the **Redshift Advisor**?
96. How to perform **Data Ingestion** into Redshift (`COPY` command)?
97. Why is the `COPY` command preferred over individual `INSERT` statements?
98. Explain **Manifest Files** for the `COPY` command.
99. How to handle **Data Unload** (`UNLOAD` command) to S3?
100. What is **Redshift Data Sharing** across clusters?
101. Explain **Redshift ML** (Machine Learning integration).
102. What is **Federated Query** in Redshift (querying RDS/Aurora data)?
103. Explain **AQUA (Advanced Query Accelerator)**.
104. How to handle **Schema Design** for performance (Star vs. Snowflake schemas)?
105. Explain **Vacuuming** and **Analyzing** tables in Redshift.
106. What is **Deep Copy**?
107. How to manage **Redshift Snapshots**?
108. Explain **Encryption at Rest** (KMS/HSM) in Redshift.
109. What is the role of **Redshift Data API**?
110. How to optimize **Join Performance** in Redshift?
111. Explain **Primary and Foreign Key** constraints in Redshift (Why are they not enforced?).
112. How to monitor Redshift performance using **STL, STV, SVL, and SVV** system tables?
113. What is **Redshift Spectrum Partitioning**?
114. Explain **Integration with AWS Glue** and Data Catalog.
115. How to handle **Upgrading Redshift Cluster Versions**?
116. Discuss **Redshift Cost Management** (RI, Spectrum per-terabyte billing).
117. What is **User Defined Functions (UDFs)** in Redshift?
118. How to perform **Cross-Region Snapshot Copy**?
119. Explain **Identity Provider (IdP) Federation** for Redshift access.
120. What is **Redshift Query Editor v2**?

## 4. Modern Data Lake & Storage Design (35 Questions)
121. What is a **Data Lake**? How does it differ from a **Data Warehouse**?
122. Explain the **Lake House Architecture**.
123. What is **AWS Lake Formation**?
124. Explain **AWS Glue**: Data Catalog, Crawlers, and ETL Jobs.
125. What is **Blueprints** in Lake Formation?
126. Explain **Athena**. How to query S3 data using standard SQL (Presto-based)?
127. What is the difference between **Athena vs. Redshift Spectrum**?
128. How to optimize Athena query performance (Parquet/ORC, Partitioning)?
129. Explain **Glue DataBrew** (visual data prep).
130. What is **Glue Elastic Views**?
131. Explain **Amazon EMR (Elastic MapReduce)**. When to use EMR vs. Redshift?
132. What is **EMR Serverless**?
133. Explain **Hadoop, Spark, Presto, and Hive** in the context of AWS EMR.
134. What is **Amazon Quicksight**?
135. How to perform **Cross-Service Data Movement** (Pipeline vs. Glue vs. DataSync)?
136. Explain **Amazon Kinesis Data Firehose** for real-time storage into S3/Redshift.
137. What is **MSK Connect** for storage?
138. Explain **Data Governance** in the cloud.
139. How to manage **Data Privacy (GDPR/PII)** in a Data Lake (Macie integration)?
140. What is **Amazon Macie** and how it protects S3 data?
141. Explain **S3 VPC Endpoints (Gateway vs. Interface)** for Big Data flows.
142. How to handle **Metadata Management**?
143. Explain **Data Partitioning Strategies** for multi-year datasets.
144. What is the **S3 Storage Class Analysis** for big data optimization?
145. Compare **Parquet vs. Avro vs. ORC**. When to use which for storage and querying?
146. Explain **Snappy and Gzip compression** for big data storage.
147. How to implementation **Row-level and Column-level security** in Lake Formation?
148. What is **Redshift RA3 with Managed Storage** vs. **Legacy DS2/DC2** nodes?
149. Explain **Redshift Cross-cluster Data Sharing** security.
150. Summary: As an SDE3, design a comprehensive storage and analytical architecture for a company generating 50TB of logs per day, focusing on cost-efficiency, durability, and sub-second query performance.

---
*Generated by Antigravity for SDE3 Interview Prep.*
