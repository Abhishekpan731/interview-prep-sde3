# 150 Generative AI & LLM Integration Interview Questions (SDE3)

## 1. LLM Fundamentals & OpenAI API (20 Questions)
1. What is a **Large Language Model (LLM)**? Explain the Transformer architecture at a high level.
2. What are **Tokens**? How does tokenization affect cost and context window limits?
3. Explain the difference between **Base Models** and **Instruct/Chat Models**.
4. What is the **Context Window**? What happens when a prompt exceeds this limit?
5. Explain **Hallucination** in LLMs. Why does it happen and how to mitigate it?
6. What is **Temperature** in LLM parameters? How does it affect the "creativity" vs. "determinism" of responses?
7. Explain **Top-P (Nucleus Sampling)** and how it relates to Temperature.
8. What is **Fine-tuning** vs. **Few-shot Prompting** vs. **Zero-shot Prompting**?
9. Explain the **OpenAI API** architecture. How do you handle **Streaming Responses** in Java?
10. What are **System, User, and Assistant Messages** in the Chat Completions API?
11. How to handle **Rate Limiting** and **Retries** when calling OpenAI APIs from a Spring Boot app?
12. What is **Function Calling** (OpenAI tool use)? How does the LLM decide which function to call?
13. Explain **JSON Mode** in OpenAI API. How do you ensure the LLM returns valid JSON?
14. What is the **Embeddings API**? What is the dimensionality of models like `text-embedding-3-small/large`?
15. Explain **Model Distillation**. Why is it useful for SDE3s to consider?
16. What is the difference between **GPT-4, GPT-4o, and GPT-4-turbo**?
17. How to monitor **Token Usage** and **Costs** in a production Java application?
18. What is **Multi-modal LLM**? (e.g., GPT-4o with Vision).
19. Explain **Prompt Injection** attacks.
20. What is **RLHF (Reinforcement Learning from Human Feedback)**?

## 2. Spring AI (25 Questions)
21. What is **Spring AI**? How does it integrate into the Spring Boot ecosystem?
22. Explain the **AiClient** (or `ChatClient`) interface. How does it abstract different LLM providers?
23. What is the **Prompt** class in Spring AI? How does it encapsulate messages and options?
24. Explain **Spring AI Portability**. How easy is it to switch from OpenAI to Azure OpenAI or Bedrock?
25. How do you use **Spring AI Templates** (`PromptTemplate`) for dynamic prompt generation?
26. What is the **OutputParser** in Spring AI? How do you map LLM string responses to Java POJOs?
27. Explain the **EmbeddingClient** in Spring AI.
28. How does Spring AI handle **Streaming AI Responses** using `Flux`?
29. What is the **VectorStore** abstraction in Spring AI? Which implementations are currently supported?
30. How to configure **API Keys** and **Model parameters** via Spring Boot `application.yml`?
31. Explain **Spring AI ETL (Extract, Transform, Load)** pipeline for data ingestion.
32. What is a **DocumentReader** in Spring AI? (e.g., `PagePdfDocumentReader`).
33. Explain the **TextSplitter** (Token-based vs. Character-based). Why is chunking important?
34. How do you use **Advisor** (experimental/new feature) in Spring AI?
35. What is the role of **SimpleVectorStore** vs. a production-grade store like `PgVector`?
36. How to implementation **History/Conversation Context** in a Spring AI `ChatClient`?
37. Explain **Spring AI Observability**. How to track inference latency and success rates?
38. What is the **ImageClient** and **AudioClient** in Spring AI?
39. How do you use **Environment Variables** for AI secrets in a Kubernetes-deployed Spring AI app?
40. Explain the **Auto-configuration** magic behind `ChatClient.Builder`.
41. How to implement **Multi-model fallback** using Spring AI?
42. What is **Semantic Routing** in the context of Spring AI?
43. How do you handle **Long-running AI tasks** in Spring Boot?
44. Explain **Spring AI's support for Local Models** (Ollama).
45. Future of Spring AI: Discussion on **Agents** and **Tool calling** support.

## 3. LangChain4j & Semantic Kernel (25 Questions)
46. What is **LangChain4j**? How does it differ from the Python LangChain?
47. Explain the **AI Service** interface in LangChain4j. How does it use Proxy objects?
48. What is **Memory** in LangChain4j? Explain `ChatMemory` and its implementations like `MessageWindowChatMemory`.
49. How do you implement **Tools (Function Calling)** in LangChain4j? Explain the `@Tool` annotation.
50. What are **Chains** in LangChain? How to build a custom processing pipeline?
51. Explain **Output Extractors** in LangChain4j.
52. What is **Structured Outputs** and how does LangChain4j enforce them?
53. Explain **Azure OpenAI SDK** vs **LangChain4j** for Java.
54. What is **Semantic Kernel (SK)** for Java? How does Microsoft's approach differ from LangChain?
55. Explain the concepts of **Skills, Plugins, and Functions** in Semantic Kernel.
56. What is a **Kernel** in SK? How do you configure it with AI services?
57. Explain **Connectors** in Semantic Kernel.
58. What is the **Planner** in Semantic Kernel? How does it automatically create chains?
59. How to handle **State** across multiple AI interactions in a stateful SK application?
60. Compare **Spring AI vs. LangChain4j**. Which one has better enterprise support?
61. Explain **Document Loaders** and **Document Transformers** in LangChain4j.
62. What is the **EmbeddingModel** and **EmbeddingStore** in LangChain4j?
63. How to perform **Batch Processing** of embeddings in LangChain4j?
64. Explain **Prompt Templates** and **Variables** in LangChain4j.
65. What is **Moderate** (Content Filtering) in LangChain4j?
66. How do you implement **Caching** for LLM responses to save costs?
67. Explain **Tracing** with LangChain4j (integration with LangSmith or OpenTelemetry).
68. What is **Re-ranking** in a LangChain4j pipeline?
69. How do you implement a **conversational agent** that can query a SQL database?
70. What is **Semantic Memory** in Semantic Kernel?

## 4. RAG (Retrieval Augmented Generation) Pipelines (30 Questions)
71. What is **RAG**? Why is it superior to fine-tuning for most enterprise knowledge tasks?
72. Explain the **RAG Architecture**: Ingestion, Retrieval, and Generation.
73. What is **Semantic Search** vs. **Keyword Search** (BM25)?
74. Explain **Hybrid Search**. Why is combining Vector and Keyword search effective?
75. What is **Chunking**? Explain strategies like Fixed-size, Overlapping, and Semantic-based chunking.
76. What is the impact of **Chunk Size** and **Overlap** on retrieval quality?
77. Explain the **Retrieval Step**. How do you use "K" (top_k) to control context?
78. What is **Metadata Filtering**? How does it improve the relevance of retrieved documents?
79. Explain **Re-ranking (Cross-Encoders)**. Why use it after initial vector retrieval?
80. What is **Query Expansion** or **Multi-Query** retrieval?
81. Explain **Hypothetical Document Embeddings (HyDE)**.
82. What is **Context Compression**? How to fit more relevant data into the context window?
83. Explain **Lost in the Middle** phenomenon and how RAG pipelines address it.
84. How do you handle **Stale Data** in a vector store? (Incremental updates).
85. Explain **Self-RAG**. How does the model self-correct based on retrieved knowledge?
86. What is **Graph RAG**? How does a Knowledge Graph improve context?
87. How to implementation **Multimodal RAG** (images and text)?
88. What is a **Retriever** vs. a **document store**?
89. Explain **Parent Document Retrieval** (storing small chunks, returning full context).
90. What is **Sentence Window Retrieval**?
91. How to evaluate RAG performance? Explain **RAGAS** metrics (Faithfulness, Answer Relevance).
92. Explain **Context Precision** and **Context Recall**.
93. What is **Grounding** in the context of RAG?
94. How to prevent the LLM from using "pretrained knowledge" when RAG context is provided?
95. What are **Hallucinations in RAG**? (e.g., ignoring the context).
96. Discuss the cost of **Embedding** large datasets.
97. How to design a **High-throughput RAG** system for 10k users?
98. What is **Agentic RAG**? (the LLM decides whether to search or not).
99. Explain **Modular RAG**.
100. How to handle **Permissions/Security** at the retrieval layer (filtering documents by user role)?

## 5. Vector Databases & Search (20 Questions)
101. What is a **Vector Database**? How does it store and index high-dimensional embeddings?
102. Explain **Cosine Similarity** vs. **Euclidean Distance (L2)** vs. **Dot Product**.
103. What is **ANN (Approximate Nearest Neighbor)** search?
104. Explain the **HNSW (Hierarchical Navigable Small World)** algorithm.
105. What is **IVF (Inverted File Index)**?
106. Compare **Pinecone, Weaviate, Milvus, Qdrant, and PgVector**.
107. What are the benefits of using **PgVector** (PostgreSQL extension) for Java developers?
108. Explain **Vector Indexing** vs. **Vector Storage**.
109. What is **Dimensionality Reduction** (PCA)? Is it used in vector DBs?
110. How do you perform **Filtering during Vector Search**? (Pre-filtering vs. Post-filtering).
111. Explain **Metadata CRUD** operations in vector databases.
112. What is **Elasticsearch's Vector Search** capability vs. a dedicated Vector DB?
113. Explain **Scalability** in Vector Databases (sharding and replication).
114. How to choose the right **Embedding Model** for your specific domain (medical, legal, coding)?
115. Explain **Cross-modal Embeddings** (CLIP).
116. What is **Latency of ANN search** on 10 million vectors?
117. How to handle **high-cardinality metadata** filtering?
118. What is **Sub-index** searching?
119. How to monitor **Vector Store health** (Recall rate, Query latency)?
120. Future of databases: Are all databases becoming Vector DBs?

## 6. Engineering & Production Deployment (20 Questions)
121. How to implementation **Caching** for LLM API calls? (Semantic Cache).
122. Explain **Prompt Versioning** and A/B testing for prompts.
123. How to handle **Long-context LLMs** (e.g., 100k+ tokens) performance?
124. What is **LLMOps**?
125. How to track **End-to-end Traces** of an AI request (Web -> API -> Vector Store -> LLM -> Web)?
126. Explain **Cost Optimization** strategies for LLM-based products.
127. How to implement **Guardrails** (e.g., NeMo Guardrails) to prevent toxic outputs?
128. What is **Red Teaming** for Generative AI applications?
129. How to handle **Private LLM Deployment** (VPC, Private Endpoints)?
130. What is **Azure OpenAI Service** vs. **Public OpenAI API**?
131. How to perform **Offline Evaluation** of a prompt change?
132. Explain **Unit Testing** in LLM applications. (How to test a "stochastic" component?).
133. What is **Data Privacy** in LLMs? How to ensure PII is not sent to the provider?
134. Explain **Input/Output Sanitization** for AI apps.
135. How to implementation **Usage Quotas and Throttling** per user?
136. What is the role of **Semantic Kernels** in building scalable "Agents"?
137. How to use **GPU acceleration** locally with Ollama?
138. Explain **Multi-tenant LLM Applications**.
139. How to design for **Model Obsolescence**? (e.g., swapping GPT-4 for GPT-5).
140. Summary: What is the most challenging part of bringing a Generative AI feature to production in a regulated enterprise?

---
*Generated by Antigravity for SDE3 Interview Prep.*
