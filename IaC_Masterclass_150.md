# 150 Infrastructure as Code (IaC) Interview Questions (SDE3)

## 1. IaC Fundamentals & Philosophy (25 Questions)
1. What is **Infrastructure as Code (IaC)**? How does it differ from traditional manual infrastructure management?
2. Explain the core benefits of IaC: **Consistency, Speed, Lower Cost, and Risk Reduction**.
3. What is the difference between **Declarative** and **Imperative** IaC? Provide examples of tools for each.
4. Explain **Idempotency** in the context of IaC. Why is it the most critical property of an IaC tool?
5. What is the difference between **Mutable** and **Immutable** infrastructure?
6. Explain the concept of **Infrastructure Drift**. How can IaC tools detect and fix it?
7. What is **Configuration Management** vs. **Infrastructure Orchestration**? (e.g., Ansible vs. Terraform).
8. Explain the **Push** vs. **Pull** model in IaC tools.
9. What is **GitOps** and how is it the "evolution" of IaC?
10. How does IaC facilitate **Disaster Recovery (DR)**?
11. Explain **Cloud Agnostic** vs. **Cloud Specific** IaC tools.
12. What is the role of the **State File** in IaC?
13. How to handle **Secrets Management** (Passwords, Keys) in IaC code?
14. Explain **Modularization** in IaC. Why is code reuse important for infrastructure?
15. What is **Continuous Infrastructure (CI/CD for Infra)**?
16. How do you perform **Testing** for IaC? (Unit, Integration, Policy testing).
17. Explain **Policy as Code (PaC)**. (e.g., OPA, Sentinel).
18. What is the **DRY (Don't Repeat Yourself)** principle in IaC?
19. How to manage **Multiple Environments** (Dev, Staging, Prod) using IaC?
20. What is **Day 0, Day 1, and Day 2** operations in infrastructure?
21. Explain **Version Control** strategies for IaC repositories.
22. What is the impact of **Concurrency** (multiple people running IaC at the same time) and how to handle it?
23. Explain **Provisioning** vs. **Deployment**.
24. What is **Self-service Infrastructure**?
25. How does IaC improve **Security and Compliance Audits**?

## 2. Terraform Masterclass (40 Questions)
26. What is **Terraform**? Explain its architecture (Core vs. Plugins/Providers).
27. Describe the **Terraform Workflow**: `init`, `plan`, `apply`, `destroy`.
28. What happens during `terraform init`? What are **Providers** and **Modules**?
29. Explain the **Terraform State (`terraform.tfstate`)**. Where should it be stored in a production environment?
30. What is a **State Backend** (e.g., S3 + DynamoDB)? Why is **State Locking** necessary?
31. How to handle **Sensitive Data** in Terraform state files?
32. Explain **Terraform Modules**. How to create a reusable module?
33. What is the difference between **Input Variables**, **Local Values**, and **Output Values**?
34. Explain **Data Sources**. How to query existing infrastructure that was not created by Terraform?
35. What is **Terraform Registry**?
36. Describe **Resource Dependencies** (Implicit vs. Explicit using `depends_on`).
37. Explain the **Taint and Untaint** commands. When would you manually "taint" a resource?
38. What is the **Terraform Lifecycle** block (`create_before_destroy`, `prevent_destroy`, `ignore_changes`)?
39. Explain **Terraform Workspaces**. How do they differ from Git branches?
40. What is **Terraform Cloud** vs. **Terraform Enterprise**?
41. Explain **Provisioners** (`local-exec`, `remote-exec`). Why should they be used as a last resort?
42. What is **Terraform Refresh**?
43. How to **Import** existing infrastructure into Terraform state?
44. Explain **Graph Command** (`terraform graph`). How does Terraform determine the execution order?
45. What is **HCL (HashiCorp Configuration Language)**?
46. How to handle **Loops** in Terraform (`count` vs. `for_each`)? When to use which?
47. Explain **Dynamic Blocks**.
48. What is **Terraform Console** used for?
49. How to perform a **Partial Apply** using `-target`? Why is it discouraged?
50. Explain **Variable Validation** in Terraform.
51. What is the **Check** block introduced in Terraform 1.5?
52. How to use **Terraform Functions** (e.g., `lookup`, `merge`, `element`)?
53. Explain **Terraform State MV** (moving resources between modules) and **RM** (removing from state).
54. What is **Terraform 1.0+** compatibility promise?
55. How to implementation **Blue-Green Infrastructure** using Terraform?
56. What is **OpenTofu**? (The fork of Terraform).
57. Explain **Terraform Providers Version Constraint** strategy (`~>`, `>=`, `=`).
58. How to debug Terraform (`TF_LOG`)?
59. What is the difference between `terraform plan` and `terraform plan -out=tfplan`?
60. Explain **Aliasing Providers** (handling multiple regions or accounts).
61. How to manage **S3 Buckets and DynamoDB** for the backend itself?
62. What is **Terraform Metadata**?
63. Explain **Computed Attributes**.
64. How to handle **Race Conditions** in cloud resource creation?
65. Discuss **Monolith vs. Micro-states** (Splitting large terraform codes into smaller state files).

## 3. Ansible: Configuration Management (25 Questions)
66. What is **Ansible**? Why is it considered "Agentless"?
67. Explain **Ad-hoc Commands** vs. **Playbooks**.
68. What is an **Inventory** file (Static vs. Dynamic)?
69. Explain **Play, Task, and Module** in an Ansible Playbook.
70. What are **Roles**? How do they help in organizing large Ansible projects?
71. What is **Ansible Galaxy**?
72. Explain **Variables** in Ansible (Group Vars, Host Vars, Facts).
73. What is **Gathers Facts**? Why would you disable it?
74. Explain **Handlers**. When are they triggered?
75. What is **Ansible Vault**? How to store encrypted passwords?
76. Explain **Idempotency** in Ansible. Give an example of a non-idempotent task.
77. What is a **Condition (`when`)** in Ansible?
78. Explain **Loops (`loop`, `with_items`)**.
79. What are **Templates (`jinja2`)**?
80. Explain **Check Mode (`--check`)** and **Diff Mode (`--diff`)**.
81. What is **Ansible Tower / AWX**?
82. Explain **Connection Plugins** (SSH, WinRM, Docker).
83. How to handle **Escalation (`become`)** in Ansible?
84. What is **Ansible Lint**?
85. Explain **Error Handling** in Playbooks (`ignore_errors`, `failed_when`, `changed_when`).
86. What is the **Strategy** parameter (`linear` vs. `free`)?
87. How to speed up Ansible with **Pipelining** and **Mitogen**?
88. Explain **Blocks** for task grouping and exception handling.
89. How to use **Local Action** and **Delegate To**?
90. Compare **Ansible vs. Puppet vs. Chef**.

## 4. Cloud-Native IaC & Alternatives (20 Questions)
91. What is **CloudFormation** (AWS Specific)?
92. Compare **Terraform vs. CloudFormation**.
93. What is **AWS CDK (Cloud Development Kit)**? How does it allow writing infra in Java/TypeScript?
94. Explain **CDK Constructs** (L1, L2, L3).
95. What is **Pulumi**? How does it differ from Terraform?
96. Explain **ARM Templates** (Azure) and **Bicep**.
97. What is **Google Cloud Deployment Manager**?
98. Explain **Crossplane**. How does it turn K8s into a control plane for any infrastructure?
99. What is **Karpenter** for infrastructure autoscaling?
100. Explain **Serverless Framework** for IaC.
101. What is **Helm** as IaC for Kubernetes?
102. What is **CDK8s** (Cloud Development Kit for Kubernetes)?
103. Explain **Terraformer** (Reverse Terraform).
104. What is **Terragrunt**? Why is it used to make Terraform more DRY?
105. Explain **Terraform-compliance** and **Checkov**.
106. What is **Infracost**? How to see the cost of a Terraform plan?
107. Explain **StackSets** in CloudFormation.
108. What is **Drift Detection** in AWS CloudFormation?
109. Explain **AWS SAM (Serverless Application Model)**.
110. How to choose between **HCL (Terraform)** and **General Purpose Languages (CDK/Pulumi)**?

## 5. IaC Security, Testing & Best Practices (20 Questions)
111. What are the top **Security Risks** in IaC?
112. How to perform **Static Analysis** on IaC code?
113. Explain **Policy as Code (PaC)** using **Open Policy Agent (OPA) / Rego**.
114. How to prevent **Hardcoded Credentials** in IaC? (Pre-commit hooks).
115. Explain **Unit Testing** for modules (e.g., `terratest` in Go or `kitchen-terraform`).
116. What is **Integration Testing** in IaC?
117. How to manage **Least Privilege Access** for IaC service accounts?
118. Explain the **Module Versioning** strategy for an enterprise.
119. What is a **Gold Image** vs. **Standardized Module**?
120. How to handle **Multi-account and Multi-region** security boundaries?
121. Explain **Cloud Custodian**.
122. How to perform **Rollbacks** in infrastructure?
123. What is **Ephemeral Infrastructure** and how does it improve security?
124. Explain **TFLint** and its rules.
125. How to implementation **Cost Governance** via IaC?
126. What is the impact of **Orphaned Resources** and how to prune them?
127. Explain **State File Protection** (Encryption at rest/in transit).
128. How to handle **Manual Changes (Hotfixes)** in a production environment?
129. What is **Infrastructure Auditing**?
130. Best practices for **Naming Conventions** in IaC.

## 6. Real-world Scenarios & Troubleshooting (20 Questions)
131. "A terraform apply failed midway and now the state is locked." How to fix it?
132. "My infrastructure drift is too large to fix automatically." What is the manual-to-automated recovery path?
133. Design the IaC pipeline for a **Multi-tier Java App** (VPC, DB, K8s, Load Balancer).
134. How to migrate from **Ansible-managed infra** to **Terraform-managed infra**?
135. "A delete-protect resource was accidentally deleted manually." How to reconcile Terraform?
136. How to handle **Circular Dependencies** in Terraform across different modules?
137. Design a **Cross-cloud** (AWS + Azure) Failover using IaC.
138. How to troubleshoot **Slow Terraform Plans** on large scale (>5000 resources)?
139. How to manage **Legacy Infrastructure** (Pet vs. Cattle)?
140. "We need to change a resource property that requires a full recreation (Destroy/Create)." How to do it zero-downtime?
141. How to use **Terraform to manage Kubernetes objects**? Is it a good idea?
142. How to create an **Account Vending Machine** using IaC?
143. Discuss the impact of **Provider updates** on stable infrastructure.
144. How to handle **Rate Limiting** from Cloud APIs during large IaC runs?
145. What is the **blast radius** of an IaC change? How to minimize it?
146. How to perform **Stress Testing** on infrastructure code?
147. Explain **Blue/Green deployments for DB instances** via IaC.
148. How would you handle **Shared VPCs** across multiple Terraform states?
149. "Our Ansible playbooks are slow." List 3 optimization techniques.
150. Summary: What is the most critical mistake you've seen in IaC implementation, and how would you prevent it today?

---
*Generated by Antigravity for SDE3 Interview Prep.*
