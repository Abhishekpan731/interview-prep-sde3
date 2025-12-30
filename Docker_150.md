# 150 Docker Interview Questions (SDE3)

## 1. Docker Fundamentals & Core Architecture (25 Questions)
1. What is Docker and how does it differ from traditional Virtual Machines (VMs)?
2. Explain the **Docker Architecture**: Client, Daemon, Registry, Images, and Containers.
3. What are **Control Groups (cgroups)** and how does Docker use them for resource limiting?
4. What are **Namespaces**? Explain the different types (PID, NET, IPC, MNT, UTS, USER) used by Docker.
5. What is the **Union File System (UnionFS)** and its role in Docker layers?
6. Explain the concept of **Copy-on-Write (CoW)** in the context of Docker images and containers.
7. What is a **Docker Image** and how is it structured in layers?
8. What is the difference between an **Image** and a **Container**?
9. Explain the **Docker Engine** components: `dockerd`, `containerd`, and `runC`.
10. What is the **OCI (Open Container Initiative)**?
11. How does Docker handle **Process Isolation**?
12. What is the **Docker Registry**? Difference between Public and Private registries.
13. Explain the **Docker Lifecycle**: Create, Start, Stop, Pause, Kill, and Delete.
14. What is the purpose of the **Docker Host**?
15. How does Docker differ from **LXC (Linux Containers)**?
16. What is **libcontainer**?
17. Explain **Docker Content Trust (DCT)**.
18. What is **Docker Desktop** vs. **Docker Engine** on Linux?
19. How does Docker handle **Signal Propagation** to the application process?
20. What is the difference between `docker run` and `docker start`?
21. Explain the **Storage Driver** (overlay2, aufs, devicemapper). Which is the default?
22. What is **Docker Context**?
23. How does Docker achieve **Near-zero Overhead** compared to VMs?
24. Explain the **Docker Hub** features like automated builds and webhooks.
25. What is **Docker Architecture (Amd64 vs Arm64)** support and multi-arch images?

## 2. Dockerfile & Image Management (25 Questions)
26. What are the best practices for writing a **Production-grade Dockerfile**?
27. Difference between `CMD` and `ENTRYPOINT`. When to use which?
28. Explain **Multi-stage Builds**. How do they reduce image size?
29. What is the **Build Cache**? How do you optimize Dockerfile instructions to leverage it?
30. Difference between `ADD` and `COPY`. Why is `COPY` usually preferred?
31. How to use `.dockerignore` effectively?
32. What is an **Intermediate Image**?
33. Explain the impact of the **Number of Layers** on image performance and size.
34. How to minimize the size of a Docker image? (Alpine vs. Slim vs. Distroless).
35. What is **Distroless Image** and why is it highly secure?
36. Explain **Squashing Layers** in Docker images.
37. How to handle **Secrets** in a Dockerfile safely? (Hint: Secret mounts).
38. Explain `ENV` vs. `ARG`. Which one is available at runtime?
39. What is **BuildKit** and its advantages over the legacy builder?
40. How to include **Build Metadata** using `LABEL`?
41. Explain the difference between `WORKDIR` and `RUN cd`.
42. How to handle **Permissions** (Non-root users) in a Dockerfile?
43. What is the `EXPOSE` instruction? Does it actually open ports?
44. Explain **Dangling Images** and how to prune them.
45. What is **Docker Image Tagging** strategy for production (SemVer vs. Git Hash)?
46. How to verify the **Integrity** of a downloaded image?
47. What is **Docker Manifest**?
48. Explain **Onbuild** triggers in a Dockerfile.
49. How to handle **Environment-specific** binaries in the build phase?
50. Discuss the trade-offs of **Self-hosting** a registry vs. using Managed services (ECR, GCR).

## 3. Storage & Volumes (20 Questions)
51. What are the three ways to persist data in Docker: **Volumes, Bind Mounts, and tmpfs**?
52. Compare **Volumes vs. Bind Mounts**. Which one is managed by Docker?
53. What is the **Volume Driver** and how does it allow integration with cloud storage (S3, Azure Files)?
54. Explain **Named Volumes** vs. **Anonymous Volumes**.
55. How to share data between two running containers?
56. What happens to the data in a volume when the container is deleted?
57. Explain **Volume Pruning**.
58. How to perform a **Backup and Restore** of a Docker volume?
59. What is **Read-only Mount** and why use it?
60. Explain the **Storage Quota** for containers.
61. How does Docker handle **I/O performance** on different storage drivers?
62. What is the impact of **File Locking** in bind mounts on Windows/Mac?
63. Explain **Data Volume Containers** (legacy pattern).
64. How to use **Local Persist** volume drivers?
65. What is **ZFS/Btrfs** support in Docker storage?
66. How to mount a **Config File** as a volume?
67. Explain **Mount Propagation** (`shared`, `slave`, `private`).
68. How to troubleshoot **Permission Denied** errors in volumes?
69. What is the purpose of `docker volume inspect`?
70. How to scale storage for a containerized database?

## 4. Networking (20 Questions)
71. Explain the **Docker Network Drivers**: `bridge`, `host`, `none`, `overlay`, and `macvlan`.
72. What is the **Default Bridge Network**?
73. How does **User-defined Bridge** network differ from the default bridge? (Automatic DNS).
74. Explain **Container Linking** (legacy) vs. **Network Aliases**.
75. What is **Port Mapping** (`-p`) and how does it work with Iptables?
76. Explain **Host Networking**. When is it necessary?
77. What is **Overlay Network**? How does it facilitate communication across multiple Docker hosts?
78. How does Docker handle **Internal DNS** and Service Discovery?
79. What is **Macvlan** and why would you use it in legacy network environments?
80. Explain **Network Isolation** between different Docker networks.
81. How to attach a container to **Multiple Networks**?
82. What is **Iptables** and how does Docker interact with it?
83. Explain **IPAM (IP Address Management)** in Docker.
84. How to troubleshoot connectivity issues between containers?
85. What is **Docker Proxy** (`docker-proxy`)?
86. Explain **Exposing vs. Publishing** a port.
87. How to limit **Bandwidth** for a container network?
88. What is **Container Network Interface (CNI)**?
89. How to configure **Static IPs** for containers?
90. Explain the role of `/etc/hosts` and `/etc/resolv.conf` in a container.

## 5. Docker Compose & Orchestration (20 Questions)
91. What is **Docker Compose** and what problem does it solve?
92. Explain the **docker-compose.yml** structure: Services, Networks, Volumes.
93. What is the difference between `docker-compose up`, `start`, and `run`?
94. How to handle **Dependencies** between services in Compose (`depends_on` vs. health checks)?
95. Explain **Environment Variable** interpolation in Compose files.
96. What are **Compose Profiles**?
97. How to use **Multiple Compose Files** for different environments (base + override)?
98. What is **Docker Swarm**? How does it differ from Kubernetes?
99. Explain **Nodes, Services, and Tasks** in Docker Swarm.
100. What is a **Stack** in Docker?
101. Explain **Scaling Services** in Swarm/Compose.
102. What is the **Routing Mesh** in Docker Swarm?
103. How to perform **Rolling Updates** in Swarm?
104. What is **Docker Secret** and **Docker Config** in Swarm mode?
105. Compare **Docker Compose V1 (python)** vs. **V2 (Go/CLI plugin)**.
106. How to restart services automatically in Compose?
107. Explain `build` instruction in a compose file vs. using a pre-built image.
108. How to use **Docker Compose for Local Development** (Hot reloading).
109. What is the `docker-compose.override.yml` default behavior?
110. How to debug a failing service in Docker Compose?

## 6. Security, Monitoring & Best Practices (20 Questions)
111. What are the key **Security Risks** associated with Docker containers?
112. Explain the **Docker Daemon Attack Surface**.
113. Why should you never run a container as **Root**?
114. What are **Capabilities** (`cap-add`, `cap-drop`) and how to use them for hardening?
115. Explain **Seccomp (Secure Computing Mode)** profiles in Docker.
116. What is **AppArmor** and **SELinux** in the context of Docker?
117. How to perform **Image Vulnerability Scanning**?
118. What is **Docker Bench for Security**?
119. Explain **Read-only Root Filesystem** for containers.
120. How to limit **Memory and CPU** usage for a container?
121. What is **OOM Killer** and how to prevent it from killing your container?
122. Explain **Docker Logging Drivers** (json-file, syslog, gelf, fluentd, splunk).
123. How to handle **Log Rotation** in Docker?
124. How to monitor Docker metrics using `docker stats` and `Prometheus/cAdvisor`?
125. What is the **Docker Audit** process?
126. How to secure the **Docker Socket** (`/var/run/docker.sock`)?
127. Explain **User Namespace Remapping**.
128. What is the impact of **privileged mode**?
129. How to handle **Zombie Processes** in containers? (The `init` process/tini).
130. What is the **Healthcheck** instruction and why is it crucial for production?

## 7. Advanced Scenarios & Troubleshooting (20 Questions)
131. How to debug a **Crashed Container**? (`docker logs`, `inspect`, `exit code`).
132. What is `docker exec` vs. `docker attach`?
133. How to recover files from a **Deleted Container** (if volumes weren't used)?
134. Explain the **Docker Events** command.
135. How to troubleshoot **High CPU/Memory** usage on the Docker host?
136. What is **Docker-in-Docker (DinD)** and **Docker-outside-of-Docker (DooD)**?
137. When would you use **Sidecar Containers** in a Docker environment?
138. How to perform a **Live Migration** of a container?
139. Explain the **Checkpoint and Restore** feature.
140. How to optimize **Network Latency** between containers?
141. Troubleshooting **DNS Resolution** failures in containers.
142. How to clean up **Zombie Containers and Volumes**?
143. Explain **Docker Plugin** architecture.
144. How to handle **Database Migrations** inside Docker?
145. What is the impact of **Time Sync** issues on containers?
146. How to use Docker for **CI/CD Pipelines** (Jenkins/GitHub Actions)?
147. Explain **Docker Desktop's VM** overhead on non-Linux systems.
148. How to run **Windows Containers** on Windows?
149. What is **Rootless Docker**?
150. Future of Docker: Discussion on **WebAssembly (Wasm)** vs. Containers.

---
*Generated by Antigravity for SDE3 Interview Prep.*
