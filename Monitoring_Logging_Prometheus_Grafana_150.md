# 150 Monitoring & Logging (Prometheus & Grafana) Interview Questions (SDE3)

## 1. Monitoring Fundamentals & Observability (25 Questions)
1. What is **Observability**? How does it differ from traditional **Monitoring**?
2. Explain the **Three Pillars of Observability**: Metrics, Logs, and Traces.
3. What are the **Golden Signals** (Latency, Traffic, Errors, Saturation)? 
4. Explain the **USE Method** (Utilization, Saturation, Errors) for infrastructure monitoring.
5. Explain the **RED Method** (Rate, Errors, Duration) for service monitoring.
6. What is the difference between **White-box Monitoring** and **Black-box Monitoring**?
7. What is a **Metric**? Explain the difference between **Counters, Gauges, Histograms, and Summaries**.
8. What is **Push-based** vs. **Pull-based** monitoring? 
9. Why does Prometheus use a Pull-based model? What are its pros and cons?
10. What is **Cardinality** in metrics? Why is high cardinality a major performance risk?
11. Explain **SLA (Service Level Agreement)**, **SLO (Service Level Objective)**, and **SLI (Service Level Indicator)**.
12. What is an **Error Budget**? How is it calculated from SLOs?
13. Explain **Distributed Tracing**. How does it connect to monitoring and logging?
14. What is **Context Propagation** in the context of observability?
15. What are **Dimensions/Labels** in metrics?
16. Explain **Sampling** in monitoring. When is it necessary?
17. What is **Heartbeat Monitoring**?
18. How do you monitor **Long-running Batch Jobs**?
19. Explain **Real User Monitoring (RUM)** vs. **Synthetic Monitoring**.
20. What is **Alert Fatigue** and how to prevent it?
21. Define **MTTR (Mean Time To Repair)** and **MTBF (Mean Time Between Failures)**.
22. How do you monitor **Third-party Dependencies**?
23. What is **Infrastructure as Code (IaC)** for monitoring?
24. Explain **Semantic Logging**.
25. How do you measure the **ROI of Observability**?

## 2. Prometheus: Architecture & Internals (30 Questions)
26. Describe the **Prometheus Architecture** (Server, Pushgateway, Alertmanager, Exporters).
27. How does Prometheus store data? Explain **TSDB (Time Series Database)**.
28. What is a **Time Series** in Prometheus? (Metric Name + Labels + Timestamp + Value).
29. Explain **Scraping**. What is the `/metrics` endpoint?
30. What is an **Exporter**? Mention some common ones (Node Exporter, Blackbox Exporter, JMX Exporter).
31. How does Prometheus handle **Service Discovery** (Kubernetes, Consul, File-based)?
32. What is the **Pushgateway**? When is it used instead of direct scraping?
33. Explain **PromQL (Prometheus Query Language)**.
34. What is the difference between **Instant Vector** and **Range Vector** in PromQL?
35. How to use the `rate()` function? Why is it only for Counters?
36. What is the difference between `rate()` and `irate()`?
37. Explain **Metric Re-labeling** (Inbound vs. Outbound).
38. What is **Recording Rules**? How do they improve dashboard performance?
39. Explain **Alerting Rules**. How are they sent to Alertmanager?
40. What is **Alertmanager**? Does it perform deduplication, grouping, and silencing?
41. How does Prometheus handle **Staleness** of metrics?
42. Explain the **Retention Policy** in Prometheus (Time-based vs. Size-based).
43. What is **Remote Write** and **Remote Read**?
44. Explain **Federation** in Prometheus. When would you use Hierarchical vs. Cross-service federation?
45. How does Prometheus scale? Discuss **Thanos** and **Cortex** for long-term storage and HA.
46. What is the impact of **High Churn** (short-lived series) on Prometheus memory?
47. How to monitor Prometheus itself?
48. Explain **Wal (Write-Ahead Log)** in Prometheus TSDB.
49. What is **Compaction** in the context of Prometheus segments?
50. How does Prometheus handle **Time Drift**?
51. What is the **Exemplars** support in Prometheus? How does it link metrics to traces?
52. Explain **Scrape Intervals** vs. **Evaluation Intervals**.
53. How to perform **Unit Testing** for Prometheus rules?
54. What is the **Prometheus Agent Mode**?
55. How to secure the Prometheus scrape endpoint?

## 3. Grafana: Visualization & Dashboards (25 Questions)
56. What is **Grafana**? How does it differ from Prometheus?
57. What are **Data Sources** in Grafana? (Prometheus, Loki, Tempo, MySQL, etc.).
58. Explain the concept of a **Dashboard** and **Panel**.
59. What are **Grafana Variables**? How do they enable dynamic, templated dashboards?
60. Explain **Dashboard Versioning** and rollback.
61. What are **Annotations** in Grafana? How to visualize deployment events alongside metrics?
62. How to create **Alerts in Grafana**? (Legacy Alerts vs. Unified Alerting).
63. What is **Grafana Loki**? How does its "Log-labeling" approach differ from ELK?
64. What is **Grafana Tempo**? How does it store traces?
65. Explain **Grafana Mimir**.
66. What is the **Grafana Plugin** architecture (Panels, Data sources, Apps)?
67. How to implement **Role-Based Access Control (RBAC)** in Grafana?
68. What is **Organization** vs. **Team** in Grafana?
69. How to optimize **Dashboard Load Time**?
70. Explain **Unified Alerting** across multiple data sources.
71. What is **Grafana OnCall**?
72. How to use **Ad-hoc Filters**?
73. Explain **Transformation** in Grafana (Join, Filter, Organize fields).
74. What is the **Explore** mode in Grafana?
75. How to share dashboards with **Snapshot** or **Public Dashboards**?
76. What is the **Grafana Agent**?
77. Explain **Library Panels**.
78. How to perform **Dashboard as Code** (Jsonnet, Terraform, Grafana API)?
79. What is **Grafana Faro** (Frontend observability)?
80. How to visualize **Heatmaps** and **Histogram** results from Prometheus?

## 4. Monitoring Java & Spring Boot (20 Questions)
81. What is **Micrometer**? How does it act as an abstraction layer for metrics?
82. Explain **Spring Boot Actuator**. What are the key endpoints for monitoring?
83. How to integrate **Prometheus with Spring Boot** using Micrometer?
84. What is the **JMX Exporter**? When would you use it over Micrometer?
85. How to create a **Custom Counter** or **Gauge** in Spring Boot?
86. Explain **Timer** and **DistributionSummary** in Micrometer.
87. How to add **Common Tags** (e.g., application name, environment) to all metrics?
88. How to monitor **Database Connection Pools** (HikariCP) metrics?
89. How to monitor **JVM Memory (Heap, Metaspace, GC)** using Actuator?
90. Explain **Micrometer Observation API** (introduced in Spring Boot 3).
91. How to monitor **@Scheduled Tasks**?
92. How to track **Custom Business Metrics** (e.g., number of orders per second)?
93. What is the performance impact of instrumentation?
94. How to monitor **Thread Pool exhaustion** in RestControllers?
95. How to use **Prometheus Exemplars** with Spring Boot 3?
96. How to monitor **Kafka Consumers/Producers** in a Spring app?
97. What is **Spring Cloud Sleuth** (legacy) vs. **Micrometer Tracing** (modern)?
98. How to handle **MeterRegistry** customization?
99. How to monitor **Caching** (Redis/Caffeine) performance in Spring?
100. How to use **Spring Boot Admin** alongside Prometheus/Grafana?

## 5. Logging & Log Aggregation (20 Questions)
101. What is **Log Aggregation**? Why is it needed in microservices?
102. Explain the **EFK/ELK Stack** (Elasticsearch, Fluentd/Logstash, Kibana).
103. Compare **Grafana Loki vs. ELK**. Why is Loki more cost-effective for high volumes?
104. What is **Structured Logging** (JSON format)? Why is it better than plain text for machines?
105. Explain **Log Levels** (DEBUG, INFO, WARN, ERROR, FATAL). When to use each?
106. What is a **Correlation ID**? How to pass it across microservices?
107. Explain **Log Rotation**.
108. How to handle **Logs in Kubernetes**? (Sidecar vs. DaemonSet vs. JSON files).
109. What is **Fluent Bit**? How does it differ from Fluentd?
110. Explain **Log Filtering and Parsing** (Grok, Regex).
111. How to prevent **Log Injection** attacks?
112. What is **Log Sampling** and when should you do it?
113. Explain **Log Retention Policies** for compliance (GDPR/HIPAA).
114. How to search for **Error Patterns** across millions of logs?
115. What is **Log-based Alerting**?
116. How to mask **Sensitive Data** (PII) in logs?
117. What is **Tail Sampling** in distributed tracing?
118. Explain **Contextual Logging**.
119. How to balance **Log Verbosity** vs. **Storage Cost**?
120. Future of Logging: Discussion on **OpenTelemetry Logs**.

## 6. Advanced Scenarios & Production Tuning (20 Questions)
121. How to troubleshoot a **Memory Leak** using Prometheus metrics?
122. How to detect a **Network Latency spike** between two services?
123. How to monitor **Kubernetes Clusters** (kube-state-metrics, cAdvisor)?
124. Explain **Probing** (Liveness/Readiness) vs. **Scraping**.
125. How to handle **High Availability** for Prometheus?
126. What is **Alert Suppression** and **Inhibition**?
127. How to monitor **Serverless (AWS Lambda)** using Prometheus?
128. Explain **Long-term Metric Retention** strategies.
129. How to implementation **Auto-remediation** based on alerts?
130. How to monitor a **Service Mesh** (Istio/Linkerd) metrics?
131. Explain **Observability Pipelines** (Cribl, Vector).
132. What is **OpenTelemetry (OTel)**? Why is it becoming the industry standard?
133. Explain the **OTel Collector** (Receiver, Processor, Exporter).
134. How does OTel replace vendor-specific exporters?
135. What is **Semantic Conventions** in OTel?
136. Explain **Metrics-to-Logs-to-Traces** navigation (Correlation).
137. How to optimize **Scrape Configs** for thousands of targets?
138. Troubleshooting **Dropped Scrapes**.
139. Explain **Prometheus Recording Rules** performance benefits.
140. How to handle **Dual Monitoring** during migration (e.g., Datadog to Prometheus).

## 7. Real-world Problem Solving (10 Questions)
141. "The dashboard is slow." Walk me through your debugging steps.
142. "We are getting too many false-positive alerts at night." How to fix?
143. "A critical service crashed but there were no logs." What happened and how to ensure it doesn't happen again?
144. Design a monitoring strategy for a **Global E-commerce Flash Sale**.
145. "Cardinality is exploding." How to find the offending label?
146. How to monitor **Database Deadlocks**?
147. "Metric values are intermittent/missing." What are the possible causes?
148. How would you design a dashboard for a **Business Stakeholder** (CTO level) vs. a **DevOps Engineer**?
149. How to handle **Timezone differences** in a global monitoring setup?
150. Summary: What is the most complex observability challenge you've solved in production?

---
*Generated by Antigravity for SDE3 Interview Prep.*
