# 150 Elasticsearch Interview Questions (SDE3)

## 1. Core Architecture & Fundamentals (20 Questions)
1. What is **Elasticsearch**? How does it differ from a traditional Relational Database?
2. Explain the high-level architecture: **Cluster, Node, Index, Document, and Type** (and the removal of types).
3. What are the different types of **Nodes** (Master, Data, Ingest, Coordinating, Machine Learning)?
4. Explain the concept of **Sharding**. What is the difference between a **Primary Shard** and a **Replica Shard**?
5. How does Elasticsearch handle **Data Distribution** across shards?
6. What is the **Cluster State**? Which node is responsible for managing it?
7. Explain the **Split-Brain Problem** in Elasticsearch. How was it solved in 7.x+ (Master election changes)?
8. What is the **Discovery** process? Explain how nodes find each other and form a cluster.
9. Describe the **Lifecycle of a Document Write** request.
10. Describe the **Lifecycle of a Search** request (Scatter-Gather pattern).
11. What is a **Segment**? How do segments relate to shards?
12. Explain the **Inverted Index** and how it enables fast full-text search.
13. What is the role of **Apache Lucene** in Elasticsearch?
14. Explain **Document Versioning** and how optimistic concurrency control works.
15. What are **Near Real-Time (NRT)** searches? Why is it not "Real-Time"?
16. Explain the **Refresh** vs. **Flush** operations.
17. What is the **Transaction Log (Translog)** and why is it needed?
18. How does Elasticsearch handle **Node Failures** and shard rebalancing?
19. What is **Cross-Cluster Search (CCS)**?
20. Explain **Elasticsearch's REST API** and its stateless nature.

## 2. Internals & Lucene Deep Dive (20 Questions)
21. Deep dive into the **Inverted Index structure**: Posting lists, Term frequency, and Dictionary.
22. What is **FST (Finite State Transducer)** and how does it optimize term lookups?
23. Explain **Doc Values**. Why are they needed for sorting and aggregations?
24. What are **Field Data** (in-memory) vs. **Doc Values** (on-disk)?
25. Explain the **Merge Process** of segments. Why is it expensive?
26. What is the impact of **Deleted Documents** on index size and merge performance?
27. How does Lucene handle **Immutable Segments**?
28. Explain **Scoring** in Elasticsearch (TF-IDF vs. BM25).
29. What is **Coordinate Tracking**?
30. How does Elasticsearch handle **Multilingual Search** at the internal level?
31. Explain **Term Vectors** and when to use them.
32. What is the **Global Ordinals** concept in aggregations?
33. Describe the **Binary representation** of documents in Lucene.
34. How does the **Circuit Breaker** mechanism prevent OOM?
35. What is the **Request Cache** vs. **Node Query Cache**?
36. Explain the **Filesystem Cache** and its critical role in ES performance.
37. What is the difference between **Normalization** and **Stemming**?
38. How does Elasticsearch manage **Memory (Heap vs. Off-heap)**?
39. Explain the **Block Cache**.
40. What is **Point-in-Time (PIT)** and how is it used in search?

## 3. Mapping, Analysis & Data Modeling (25 Questions)
41. What is **Mapping**? Explain **Dynamic Mapping** vs. **Explicit Mapping**.
42. What are **Dynamic Templates**?
43. Explain **Analysis Pipeline**: Character Filters, Tokenizers, and Token Filters.
44. What is the difference between a **Text** field and a **Keyword** field?
45. Explain **Analyzers**: Standard, Simple, Whitespace, and Language Analyzers.
46. How do you create a **Custom Analyzer**?
47. What is **Index-time Analysis** vs. **Search-time Analysis**? Why should they usually match?
48. Explain the **Ignore Above** setting in mappings.
49. What are **Nested Objects**? How do they differ from standard Object arrays in terms of search?
50. Explain **Join Datatype (Parent-Child)**. When should you use it, and what are the performance impacts?
51. What is **Denormalization** in Elasticsearch? Why is it preferred?
52. How to handle **Arrays** in mappings?
53. Explain **Field Aliases**.
54. What is the **_source** field? When would you disable it?
55. What is the **_all** field (deprecated) and its replacement?
56. Explain **Multi-fields** (e.g., a field being both text and keyword).
57. What are **Meta-fields** (`_id`, `_index`, `_routing`)?
58. Explain **Routing** in detail. How can custom routing improve performance?
59. What is **Mapping Explosion** and how to prevent it?
60. Explain **Null Value** handling in mappings.
61. What is the **Search As You Type** field type?
62. Explain the **Rank Feature** and **Rank Features** datatypes.
63. How to handle **Large Monolithic Documents** vs. **Flattened Documents**?
64. What is the **Flattened** datatype?
65. Explain **Date** math and date formats in mappings.

## 4. Query DSL & Searching (25 Questions)
66. Explain the difference between **Query Context** and **Filter Context**. Which one is cached?
67. What is the **Match Query** vs. **Term Query**?
68. Explain **Match Phrase Query** and the **Slop** parameter.
69. What is **Multi-Match Query**? Explain the different types (`best_fields`, `most_fields`, `cross_fields`).
70. Explain the **Bool Query**: `must`, `filter`, `should`, and `must_not`.
71. How does the **minimum_should_match** parameter work?
72. What is **Query String Query** vs. **Simple Query String**?
73. Explain **Prefix, Wildcard, and Regexp** queries. What are the performance risks?
74. What is **Fuzzy Query**? Explain Levenshtein distance.
75. What is the **Function Score Query**? How to use it for custom relevance?
76. Explain **Pagination** in Elasticsearch: `from` + `size`, `scroll`, and `search_after`.
77. Why is `from` + `size` risky for deep pagination?
78. What is **Boosting**? (Index-time vs. Query-time).
79. Explain **Dis Max Query**.
80. What is a **Span Query**? Provide a use case.
81. Explain **Geo Queries**: `geo_distance`, `geo_bounding_box`, `geo_polygon`.
82. What is the **Range Query**?
83. How to use **Highlighting** in search results?
84. Explain **Source Filtering** in search requests.
85. What is **Explain API**? How to read its output to debug relevance?
86. Explain **Profile API** for query performance debugging.
87. What is **Reindexing**? When and how to do it zero-downtime using **Aliases**?
88. Explain the **Update By Query** and **Delete By Query** APIs.
89. What is **Scripted Query** (Painless)?
90. How to search across **Multiple Indices**?

## 5. Aggregations & Analytics (15 Questions)
91. What are **Aggregations**? Explain the three main types: **Metric, Bucket, and Pipeline**.
92. Explain **Terms Aggregation**. What is the accuracy problem (Estimated vs. Exact)?
93. What are **Sub-aggregations**?
94. Explain **Date Histogram** vs. **Histogram** aggregations.
95. What is **Filter Aggregation** vs. **Filters Aggregation**?
96. Explain **Cardinality Aggregation**. How does it use HyperLogLog++?
97. What is **Significant Terms** aggregation? When is it used (e.g., fraud/anomaly detection)?
98. Explain **Percentiles** and **Percentile Ranks** metric aggregations.
99. What are **Pipeline Aggregations** (e.g., `avg_bucket`, `moving_avg`, `derivative`)?
100. What is the **Bucket Selector** aggregation?
101. How does **Global Aggregation** work?
102. Explain **Range, Date Range, and IP Range** aggregations.
103. What is the impact of **High Cardinality** fields on aggregation performance?
104. How do you aggregate on **Nested** fields?
105. What is **Matrix Aggregations**?

## 6. Cluster Operations, Scaling & HA (15 Questions)
106. How do you perform a **Rolling Restart** of an Elasticsearch cluster?
107. Explain **Shard Allocation** processes.
108. What are **Shard Allocation Awareness** and **Forced Awareness**?
109. How to handle **Hot-Warm-Cold Architecture** in Elasticsearch?
110. Explain **Index Lifecycle Management (ILM)**.
111. What is a **Snapshot**? How to perform backups and restores?
112. Explain **Cross-Cluster Replication (CCR)** for DR.
113. What is **Shard Splitting** vs. **Shrinking**?
114. How to scale a cluster for **Bulk Indexing** vs. **High Query Throughput**?
115. Explain **Cluster State Updates** and potential bottlenecks.
116. What is the **Voting Configuration** in master election?
117. How to manage **Large Clusters** (>100 nodes)?
118. What is the **Gateway API** for cluster metadata?
119. Explain **Data Tiers** (Content, Hot, Warm, Cold, Frozen).
120. How to use **Elasticsearch Service (Cloud)** vs. self-hosted overhead.

## 7. Performance Tuning & Best Practices (15 Questions)
121. How to optimize **Indexing Speed**? (Bulk size, refresh interval, replica count).
122. How to optimize **Search Speed**? (Filtering, Doc values, avoidance of scripts).
123. What is the recommended **Shard Size** (General rule of thumb)?
124. How many shards per GB of heap is ideal?
125. Why should you avoid too many **Small Shards**?
126. Explain **FieldData Cache Tuning**.
127. How to tune the **OS (Linux)** for Elasticsearch (swapping, file descriptors, mmapfs)?
128. What is the significance of the **Half-RAM Heap** rule? Why not go over 32GB?
129. How to handle **Garbage Collection (GC)** tuning for G1GC in ES?
130. Explain **Bulk API** best practices.
131. How to use **Search Templates**?
132. What is **Request Collapsing**?
133. Explain **Index Sorting** for search optimization.
134. How to monitor **Slow Logs** (Search and Index)?
135. What is the **Force Merge** API? When should you (and shouldn't you) use it?

## 8. Modern Search & AI Features (15 Questions)
136. What is **Vector Search** in Elasticsearch?
137. Explain **Dense Vector** vs. **Sparse Vector** datatypes.
138. How does **k-Nearest Neighbors (kNN)** search work in ES?
139. Explain **HNSW (Hierarchical Navigable Small World)** algorithm for vector indexing.
140. What is **ELSER (Elastic Learned Sparse EncodeR)**?
141. Explain **Hybrid Search** (combining BM25 and Vector Search) using **Reciprocal Rank Fusion (RRF)**.
142. What is **Inference API**?
143. How to use **NLP (Natural Language Processing)** models with Elasticsearch?
144. What is **Semantic Search**?
145. Explain **Elasticsearch Query Language (ESQL)** introduced in recent versions.
146. How does ES integrate with **Large Language Models (LLMs)** (RAG pattern)?
147. What is **Kibana** and its role in the Elastic Stack?
148. Explain **Logstash** and **Beats** (The "ELK" vs. "Elastic" stack).
149. What is **Machine Learning** in the Elastic Stack (Anomaly detection)?
150. Future of Elasticsearch: Discussion on **Stateless Architecture** and Cloud Native developments.

---
*Generated by Antigravity for SDE3 Interview Prep.*
