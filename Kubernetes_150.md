# 150 Kubernetes (K8s) Interview Questions (SDE3)

## 1. Core Architecture & Control Plane (20 Questions)
1. Explain the high-level architecture of Kubernetes. What are the roles of the Control Plane and the Data Plane?
2. What is the **kube-apiserver**? Why is it the only component that communicates with Etcd?
3. What is **Etcd**? Explain its role as a distributed key-value store and how it handles consensus (Raft).
4. Describe the responsibilities of the **kube-scheduler**. How does it decide which node a pod should run on?
5. What is the **kube-controller-manager**? Name at least four internal controllers it manages.
6. Explain the role of **Cloud Controller Manager (CCM)**.
7. What is **kubelet**? How does it interact with the Container Runtime (CRI)?
8. What is **kube-proxy**? Explain the difference between `iptables` and `IPVS` modes.
9. Describe the **Kubernetes API Object Model**.
10. What is a **Label** and an **Annotation**? When would you use one over the other?
11. Explain the **Declarative vs. Imperative** management styles in Kubernetes.
12. What is the **Control Loop** (Reconciliation Loop) in Kubernetes?
13. How does the API Server handle **Concurrency Control** (Optimistic Locking via `resourceVersion`)?
14. What are **Admission Controllers**? Explain the difference between Mutating and Validating Webhooks.
15. What is the **Container Runtime Interface (CRI)**?
16. Explain the purpose of **Custom Resource Definitions (CRDs)**.
17. What is **ListWatch** mechanism used by controllers?
18. How does Kubernetes handle **Master High Availability (HA)**?
19. What is the role of the **Cloud Provider Interface**?
20. Explain the **Garbage Collection** process in Kubernetes for dead pods and unused images.

## 2. Workloads & Controllers (20 Questions)
21. What is a **Pod**? Why is it the smallest deployable unit?
22. Explain the **Init Container** vs. **Sidecar Container**.
23. What is a **Deployment**? How does it perform Rolling Updates and Rollbacks?
24. Compare **Deployment** vs. **StatefulSet**. When is a StatefulSet mandatory?
25. What is a **ReplicaSet** and how does it relate to a Deployment?
26. Explain **DaemonSet**. Provide a real-world use case (e.g., Log collection).
27. What is the difference between a **Job** and a **CronJob**?
28. How do you handle **Database Migrations** using Kubernetes Jobs?
29. Explain **Pod Lifecycle States**: Pending, Running, Succeeded, Failed, and Unknown.
30. What is **Pod Disruption Budget (PDB)**?
31. How do you perform a **Blue-Green Deployment** in Kubernetes?
32. What is a **Canary Deployment** and how to implement it using Service weights or Ingress?
33. Explain **Graceful Shutdown** of a pod. What is the role of `terminationGracePeriodSeconds`?
34. What is the **PreStop Hook** and why is it useful?
35. How does a **StatefulSet** provide stable network identifiers and persistent storage?
36. What is the difference between **Healthy** and **Ready** pods?
37. Explain the **OwnerReference** field in metadata.
38. How to debug a pod stuck in `CrashLoopBackOff`?
39. What is **Horizontal Pod Autoscaling (HPA)** and how does it calculate replicas?
40. What is **Vertical Pod Autoscaling (VPA)**? Can you use HPA and VPA together?

## 3. Networking & Service Discovery (20 Questions)
41. Explain the **Kubernetes Network Model** (Standardized IP-per-pod).
42. What is the **Container Network Interface (CNI)**? Name a few popular plugins (Calico, Flannel, Cilium).
43. What is a **Service**? Why is it needed if pods have IPs?
44. Explain **ClusterIP**, **NodePort**, and **LoadBalancer** service types.
45. What is **ExternalName** service?
46. Explain **Headless Service**. When would you use one (e.g., with StatefulSets)?
47. What is an **Endpoints** object and a **Slice**?
48. What is an **Ingress Controller**? How does it differ from a standard LoadBalancer service?
49. Explain **CoreDNS** and how service discovery works internally (`service-name.namespace.svc.cluster.local`).
50. What are **Network Policies**? How do they provide isolation?
51. Explain **Ingress vs. Egress** rules in a Network Policy.
52. What is the **Service Mesh**? (Istio, Linkerd). What problems does it solve?
53. How does **mTLS** work in a Service Mesh?
54. Explain **Traffic Splitting** (A/B testing) in Ingress vs. Service Mesh.
55. What is **kube-proxy's** role in load balancing traffic to services?
56. Explain the **HostNetwork** and **HostPort** settings.
57. What is **IP exhaustion** in a cluster and how to prevent it?
58. How do you handle **External Traffic Policy** (`Local` vs. `Cluster`)?
59. What is **BGP (Border Gateway Protocol)** in the context of CNI (e.g., Calico)?
60. Explain **Service Topology** and Topology Aware Routing.

## 4. Storage & Persistence (15 Questions)
61. What is a **PersistentVolume (PV)** and **PersistentVolumeClaim (PVC)**?
62. Explain the **Lifecycle of a PVC** (Provisioning, Binding, Using, Reclaiming).
63. What are the **Reclaim Policies**: Retain, Delete, and Recycle?
64. What is a **StorageClass**? How does it enable dynamic provisioning?
65. Explain **Volume Modes**: `Filesystem` vs. `Block`.
66. What are the **Access Modes**: RWO, ROX, RWX?
67. What is the **Container Storage Interface (CSI)**?
68. Explain **Ephemeral Volumes** (emptyDir, configMap, secret).
69. What is **HostPath** and why is it discouraged in production?
70. How to handle **Database persistence** on clouds (EBS, Azure Disk, GCE PD)?
71. What is **Volume Snapshots** and **Cloning**?
72. How do you manage **Storage Expansion**?
73. Explain **Local Persistent Volumes**.
74. What is **Volume Binding Mode** (`Immediate` vs. `WaitForFirstConsumer`)?
75. How to secure sensitive data in volumes?

## 5. Config, Secrets & RBAC (25 Questions)
76. What is a **ConfigMap**? How to inject it as environment variables or volume mounts?
77. What is a **Secret**? Is it encrypted at rest by default?
78. How to encrypt Secrets at rest in Etcd?
79. What is the difference between `opaque` and `service-account-token` secrets?
80. Explain **Role-Based Access Control (RBAC)**: Role, ClusterRole, RoleBinding, ClusterRoleBinding.
81. What is a **ServiceAccount**? Why is it preferred over raw credentials for pods?
82. Explain the **Subject, Verb, and Resource** in an RBAC rule.
83. What is the **Pod Security Admission (PSA)** (Successor to Pod Security Policy)?
84. How to perform **User Authentication** in K8s (Certificates, Tokens, OIDC)?
85. What is the **Aggregated API** and **API Extension**?
86. Explain **Resource Quotas** and **LimitRanges**.
87. What is the **Taint and Toleration** mechanism?
88. Explain **Node Affinity** vs. **Node Selector**.
89. What is **Pod Affinity and Anti-affinity**? Provide a use case for high availability.
90. Explain **PriorityClass** and Pod Preemption.
91. What is **SecurityContext** (runAsUser, fsGroup, privileged)?
92. How to avoid the "Confused Deputy" problem in Kubernetes?
93. What is **Impersonation** in K8s API?
94. Explain **Contexts** in `kubeconfig` files.
95. How to restrict access to the **Cloud Metadata API** from pods?
96. What is **RuntimeClass**?
97. Explain **Projected Volumes**.
98. What is **SelfSubjectAccessReview**?
99. How to audit API requests in Kubernetes?
100. Explain **Pod Security Standards (Privileged, Baseline, Restricted)**.

## 6. Scheduling & Node Management (15 Questions)
101. How does the **Scheduling Cycle** and **Binding Cycle** work?
102. What are **Predicates** (Filtering) and **Priorities** (Scoring) in scheduling?
103. Explain **Multiple Schedulers** support.
104. What is **Bin Packing** vs. **Least Requested** scheduling strategies?
105. How to handle **Node Maintenance**? (Drain vs. Cordon).
106. What is **Cluster Autoscaler**? How does it interact with HPA?
107. Explain **Over-provisioning** and **Pause Pods**.
108. What is **Karpenter** (AWS) and how does it differ from Cluster Autoscaler?
109. How to handle **Skew** in pod distribution? (Topology Spread Constraints).
110. Explain **Eviction** based on Node pressure (Memory, Disk).
111. What is **Soft vs. Hard Eviction**?
112. How to reserve resources for the OS and Kubelet (`kube-reserved`, `system-reserved`)?
113. Explain **OOMScoreAdj** and how K8s protects critical pods.
114. What is the **Static Pod**?
115. How to troubleshoot "NodeNotReady" status?

## 7. Observability & Troubleshooting (15 Questions)
116. Explain **Liveness, Readiness, and Startup Probes**.
117. What happens if a **Liveness Probe** fails?
118. What happens if a **Readiness Probe** fails?
119. How to debug a **Pending Pod**?
120. Explain the **Logging Architecture** in K8s (JSON files, Sidecar, DaemonSet).
121. What is the **Metrics Server** and how does it enable HPA?
122. How to use `kubectl debug` with **Ephemeral Containers**?
123. What is **Prometheus / Grafana** stack for K8s monitoring?
124. Explain **Distributed Tracing** in Kubernetes (Jaeger/Zipkin).
125. How to capture a **Heap Dump / Thread Dump** from a running pod?
126. What are the common **Exit Codes** (137, 139, 143) and their meanings?
127. How to troubleshoot **Network Latency** between services?
128. What is **Events** API and how long are events stored?
129. How to perform **Port Forwarding** and **Proxy** for local debugging?
130. Explain **Kube-state-metrics**.

## 8. Helm, GitOps & Advanced Ecosystem (20 Questions)
131. What is **Helm**? Explain Charts, Templates, and Values.
132. How does **Helm v3** differ from v2 (Removal of Tiller)?
133. Explain **Helm Release Management**.
134. What is **Kustomize**? How does it differ from Helm?
135. What is **GitOps**? (ArgoCD, Flux).
136. Explain the **Pull-based vs. Push-based** CI/CD in Kubernetes.
137. What are **Kubernetes Operators**? How do they extend the API?
138. Explain the **Operator SDK** and **Kubebuilder**.
139. What is a **Mutating Webhook**? Provide an example (e.g., Sidecar injection).
140. What is **Multi-tenancy** in Kubernetes? (Hard vs. Soft isolation).
141. Explain **Cloud Native Buildpacks**.
142. What is **Serverless on K8s**? (Knative).
143. Explain **Crossplane** (Infrastructure as Code through K8s API).
144. What is **Tekton**?
145. How to manage **Multi-cluster** communication? (Submariner, Cilium Mesh).
146. What is **KubeVirt** (Running VMs on K8s)?
147. Explain **Rootless Containers** in K8s.
148. What is **WebAssembly (Wasm)** in Kubernetes?
149. Discuss **Cost Optimization** in K8s (Spot instances, Right-sizing).
150. Future of Kubernetes: Discussion on **Gateway API** (Successor to Ingress).

---
*Generated by Antigravity for SDE3 Interview Prep.*
