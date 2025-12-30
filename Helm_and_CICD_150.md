# 150 Helm & CI/CD Interview Questions (SDE3)

## 1. Helm: The Kubernetes Package Manager (60 Questions)
### Architecture & Fundamentals (15 Questions)
1. What is **Helm** and why is it essential for Kubernetes application management?
2. Explain the difference between **Helm v2** and **Helm v3**. Why was Tiller removed?
3. What is a **Chart**? Explain the standard directory structure of a Helm chart.
4. What is the role of `Chart.yaml`? Difference between `version` and `appVersion`.
5. Explain the importance of `values.yaml` vs. `values-production.yaml`.
6. What are **Helm Templates**? Which templating engine does Helm use?
7. Explain **Helm Releases**. How does Helm track release history?
8. What is the **Helm Library Chart** vs. **Application Chart**?
9. Explain **Helm Repositories**. How to create and host your own chart repository?
10. What is **Chart.lock** and how does it relate to dependency management?
11. Explain **Helm Contexts** and how it interacts with `kubeconfig`.
12. What is the **Helm SDK**?
13. Explain **Oci-based Registries** for Helm charts (e.g., ECR, Harbor).
14. What are **Post-rendering** scripts in Helm?
15. How does Helm handle **Three-way Strategic Merge Patches**?

### Templating & Logic (15 Questions)
16. How to use **Functions and Pipelines** in Helm templates?
17. Explain the use of `define`, `template`, and `include`.
18. What is the difference between `template` and `include`? (Scope and spacing).
19. How to handle **Whitespaces and Indentation** in templates using `-` and `indent`/`nindent`?
20. Explain **Flow Control** in Helm: `if/else`, `with`, and `range`.
21. What are **Built-in Objects** in Helm (e.g., `.Release`, `.Chart`, `.Values`, `.Capabilities`)?
22. How to access **Cluster Capabilities** inside a template?
23. Explain **Global Values** and how to pass them between parent and sub-charts.
24. How to use **Named Templates** (Helper templates) in `_helpers.tpl`?
25. Explain the **Required** function for mandatory values.
26. How to use **Default** and **Coalesce** functions?
27. Explaining **Type Conversion** functions in Helm (e.g., `toString`, `toInt`).
28. How to use **Dictionary and List** functions (`dict`, `list`, `append`)?
29. How to escape special characters in Helm templates?
30. What is the **Quote** and **Squote** function usage?

### Release Management & Operations (15 Questions)
31. Explain `helm install`, `upgrade`, and `rollback`.
32. What is the **Atomic** flag in Helm?
33. Explain the **Wait** and **Timeout** flags.
34. What are **Helm Hooks**? List common hooks (`pre-install`, `post-upgrade`, `test`).
35. How to use **Helm Tests** to verify a release?
36. Explain **Helm Rollback** internals. Does it roll back the cluster resource or just the metadata?
37. How to manage **Large Charts** with complex sub-charts?
38. Explain **Dependency Update** and `helm dep build`.
39. How to use **Set** flags (`--set`, `--set-string`, `--set-file`) vs. values files?
40. What is **Helm Lint** and **Helm Template** command for debugging?
41. How to manage **Sensitive Data** in Helm? (Helm-Secrets, Sops-secrets-operator).
42. Explain **Helm Plugins**. Mention a few popular ones (Diff, S3, Secrets).
43. What is the **Helm Dashboard**?
44. How to perform a **Dry Run** and what are its limitations?
45. Explain **Helm History** and how many versions it keeps by default.

### Security & Best Practices (15 Questions)
46. How to secure a Helm installation in a **Multi-tenant** cluster?
47. What are **Chart Provenance** and **Integrity**? How to use GPG signatures?
48. Explain **RBAC requirements** for Helm v3.
49. How to avoid **Configuration Drift** when using Helm?
50. Why should you avoid putting **Plaintext Secrets** in `values.yaml`?
51. Best practices for **Versioning** charts in an enterprise environment.
52. How to handle **Infrastructure Changes** (like LoadBalancer creation) through Helm?
53. Should you use Helm to manage **CRDs**? What are the limitations?
54. Explain the **Immutable Releases** principle.
55. How to structure charts for **Environment-Specific** overrides (Dev, Staging, Prod)?
56. What is **Helm Template Validation** using JSON Schema?
57. How to implement **Unit Testing** for Helm charts (using `unittest` plugin)?
58. Discuss the trade-offs of using **Helm vs. Kustomize**.
59. How to safely **Delete** a release without losing persistent data?
60. Future of Helm: Discussion on **Declarative Helm** (e.g., Helmfile).

## 2. CI/CD: Continuous Integration & Continuous Delivery (90 Questions)
### Core Concepts & Metrics (15 Questions)
61. Define **CI (Continuous Integration)**, **CD (Continuous Delivery)**, and **CD (Continuous Deployment)**.
62. What is the **Build Pipeline**?
63. Explain the **Shift Left** principle in CI/CD.
64. What are the **DORA Metrics** (Deployment Frequency, Lead Time for Changes, MTTR, Change Failure Rate)?
65. Explain the **Commit Stage** of a pipeline.
66. What is **Technical Debt** and how can CI/CD help manage it?
67. Explain **Trunk-Based Development** vs. **GitFlow**.
68. What is **Feature Toggling**? How does it allow decoupling deployment from release?
69. Explain **Idempotent Deployments**.
70. What is **Orchestration vs. Choreography** in CI/CD pipelines?
71. What is a **Build Artifact**? Mention common artifact repositories (Nexus, Artifactory).
72. Explain **Immutable Artifacts**.
73. What is a **Staging Environment** and why is it crucial?
74. Explain **Developer Experience (DevEx)** in the context of CI/CD.
75. What is the **Deployment Pipeline Pattern**?

### CI/CD Tools & Implementation (20 Questions)
76. Compare **Jenkins** vs. **GitHub Actions** vs. **GitLab CI**.
77. Explain **Jenkins Pipelines** (Declarative vs. Scripted).
78. What are **GitHub Actions Workflows, Jobs, and Steps**?
79. Explain **Runners** in GitHub Actions (Self-hosted vs. GitHub-hosted).
80. What are **Reusable Workflows** and **Composite Actions**?
81. How to use **Environment Variables and Secrets** securely in CI/CD tools?
82. Explain **Caching** in CI/CD pipelines to speed up builds (e.g., `node_modules`, `maven` repository).
83. What are **Artifacts and Dependencies** in a pipeline?
84. How to handle **Parallel Jobs** to reduce pipeline execution time?
85. Explain **Matrix Builds** in GitHub Actions.
86. What is a **Webhook**? How is it used to trigger pipelines?
87. Explain **ChatOps**.
88. How to perform **Dynamic Analysis (DAST)** in a pipeline?
89. How to integrate **Static Code Analysis (SAST)** tools (SonarQube, Snyk)?
90. Explain the role of a **Build Agent**.
91. What is **CircleCI** Orbs?
92. How to handle **Manual Approvals** in a CD pipeline?
93. What is **Pipeline-as-Code**?
94. Explain **Shared Libraries** in Jenkins.
95. How to handle **Large Scale Jenkins** (Controller vs. Agents).

### GitOps & Modern Delivery (15 Questions)
96. What is **GitOps**? Explain the four core principles.
97. Explain **ArgoCD**. How does it synchronize K8s states?
98. What is **Flux CD**?
99. Compare **Pull-based vs. Push-based** CI/CD models.
100. What is a **Reconciler** in GitOps?
101. How to handle **Secret Management in GitOps** (Sealed Secrets, External Secrets Operator)?
102. Explain **Application of Applications** pattern in ArgoCD.
103. What is **Self-healing** in GitOps?
104. How to perform **Rollbacks in GitOps**?
105. Explain **Sync Polices** in ArgoCD (Manual vs. Automatic).
106. What is **Configuration Drift Detection**?
107. How to integrate **Argo Rollouts** for advanced deployment strategies?
108. Explain **Multi-cluster GitOps** management.
109. What is the role of **Helm in a GitOps Pipeline**?
110. Explain **Image Updater** automations.

### Testing & Quality Gates (15 Questions)
111. Difference between **Unit, Integration, and System Tests** in a pipeline.
112. What is **Contract Testing** (Pact)?
113. Explain **Smoke Testing** vs. **Sanity Testing**.
114. What are **Regression Tests**?
115. Explain **Code Coverage** as a quality gate.
116. What is **Performance Testing** in CI/CD (JMeter, K6)?
117. How to implement **Visual Regression Testing**?
118. What is **End-to-End (E2E) Testing** (Playwright, Cypress)?
119. Explain **Quality Gates**.
120. How to handle **Flaky Tests**?
121. What is **Test Impact Analysis**?
122. Explain **Mocking and Stubbing** in integration tests.
123. How to use **Testcontainers** in a CI pipeline?
124. Explain **Artifact Signing and Attestation** (Cosign/Sigstore).
125. What is the **Pyramid of Testing**?

### Deployment Strategies & Resilience (15 Questions)
126. Explain **Rolling Updates**.
127. What is **Blue-Green Deployment**? How to switch traffic at the LoadBalancer/DNS level?
128. Explain **Canary Release**. How to use metrics (Prometheus) to automate rollouts/rollbacks?
129. What is **Linear/Stepwise Rollout**?
130. Explain **A/B Testing** vs. Canary Deployment.
131. What is **Shadow Deployment** (Dark Launch)?
132. How to implement **Automated Rollbacks**?
133. Explain **Circuit Breakers** and **Retries** in the context of deployment resilience.
134. How to handle **Database Schema Migrations** in a zero-downtime deployment? (Flyway/Liquibase).
135. What is **Post-deployment Verification (PDV)**?
136. Explain **Traffic Shifting** with Service Mesh (Istio).
137. How to manage **Session Persistence** during a rollout?
138. Explain **Health Checks** integration with deployment tools.
139. What is **Mean Time to Recovery (MTTR)** improvement strategies?
140. How to handle **Stateful Application** updates?

### Security (DevSecOps) & Compliance (10 Questions)
141. What is **DevSecOps**?
142. Explain **SBOM (Software Bill of Materials)**.
143. What is **Secrets Scanning** (TruffleHog, Gitleaks)?
144. Explain **Infrastructure as Code (IaC) Scanning** (TFSec, Checkov).
145. What is the **Vulnerability Scan** for container images?
146. How to implement **Governance and Compliance** in CI/CD?
147. Explain **Open Policy Agent (OPA)** integration in pipelines.
148. What is **Supply Chain Security** (SLSA framework)?
149. How to handle **Audit Logs** for deployments?
150. Summary: What is the biggest challenge in scaling CI/CD for a 1000+ developer organization?

---
*Generated by Antigravity for SDE3 Interview Prep.*
