# Core Java Interview Questions for SDE3 (150 Questions)

This document contains a comprehensive list of Core Java interview questions tailored for SDE3 level positions. The questions focus on depth, internals, performance, and modern Java features.

---

## Section 1: Memory Management & JVM Internals (15 Questions)
1. Explain the **Java Memory Model (JMM)** and the concept of "Happens-Before" relationship.
2. How does the **G1 Garbage Collector** work compared to CMS? Discuss the concept of "Humongous Objects."
3. Explain the **ZGC (Z Garbage Collector)**. How does it achieve sub-millisecond pause times?
4. What is **Compressed OOPs**? How does it help in memory optimization?
5. Describe the lifecycle of an object in the Heap memory (Eden, Survivor, Tenured).
6. How would you diagnose a **Metaspace OutOfMemoryError**?
7. Explain the difference between **Strong, Weak, Soft, and Phantom references**. Give a real-world use case for each.
8. How does the **JIT (Just-In-Time) compiler** perform optimizations like Inlining and Escape Analysis?
9. What are **Stack Maps** in Java bytecode?
10. Explain **Tiered Compilation** in JVM.
11. How do you find a memory leak in a large-scale Java application? (Tools: JVisualVM, MAT, etc.)
12. What is the role of the **Code Cache** in the JVM?
13. Explain **Dynamic CDS (Class Data Sharing)** introduced in recent Java versions.
14. How does the JVM handle **Direct Memory** allocation (via `ByteBuffer.allocateDirect`)?
15. Discuss the impact of **NUMA (Non-Uniform Memory Access)** awareness on JVM performance.

## Section 2: Concurrency & Multi-threading (25 Questions)
16. Explain the internal working of **ReentrantLock**. How does it differ from `synchronized`?
17. What is **CAS (Compare-And-Swap)**? How do `Atomic` classes use it?
18. Explain the implementation of **ConcurrentHashMap** in Java 8+. How does it manage locking?
19. Discuss the **Fork/Join Framework**. How does "Work Stealing" work?
20. What are **Virtual Threads (Project Loom)**? How do they differ from Platform Threads?
21. Explain **Structured Concurrency** and its benefits.
22. How does `CompletableFuture` work? Discuss the difference between `thenApply` and `thenCompose`.
23. What is the **Dining Philosophers Problem** and how would you solve it in Java using `Locks`?
24. Explain the **ABA problem** in lock-free programming. How does `AtomicStampedReference` solve it?
25. Describe the internal working of the **ExecutorService** and the `LinkedBlockingQueue` size implications.
26. What is a **Phaser**? How is it different from `CountDownLatch` and `CyclicBarrier`?
27. Explain **ThreadLocal** and its potential for memory leaks. How does `InheritableThreadLocal` work?
28. What is **False Sharing** and how does `@Contended` annotation help?
29. Explain **LongAdder** vs `AtomicLong`. When would you prefer one over the other?
30. How do you implement a **Distributed Lock** using Java (conceptually, via Redis or Zookeeper)?
31. What are **Scoped Values**? (Java 21+)
32. Explain the **Ready-to-Run vs Blocked vs Waiting** states in Java threads.
33. How would you implement a **Rate Limiter** using Java Concurrency utilities?
34. Discuss the performance impact of context switching in high-throughput applications.
35. How does the **Volatile** keyword ensure visibility across threads? Does it ensure atomicity?
36. Explain the **Double-Checked Locking** pattern. Why was it broken before Java 5?
37. What is the purpose of `Thread.yield()` and `Thread.onSpinWait()`?
38. Explain **Exchanger** class usage.
39. How do you handle a **Deadlock**? How can `jstack` help?
40. Discuss the internals of `AbstractQueuedSynchronizer` (AQS).

## Section 3: Collections Framework (20 Questions)
41. Explain the internal working of `HashMap` and why it was changed to use a balanced tree for collisions in Java 8.
42. What is the difference between `fail-fast` and `fail-safe` iterators?
43. How does `CopyOnWriteArrayList` achieve thread safety? What are the performance trade-offs?
44. Explain the internal implementation of `PriorityQueue`. What is the time complexity of its operations?
45. Why is `String` used as a common key in `HashMap`?
46. Compare `LinkedHashMap` and `TreeMap`. When would you use `AccessOrder` in `LinkedHashMap`?
47. How does `EnumMap` work internally? Why is it faster than `HashMap`?
48. What is `IdentityHashMap`? How does it violate the general contract of `Map`?
49. Explain the **Load Factor** and **Threshold** in `HashMap`.
50. How does `ArrayDeque` differ from `LinkedList` in terms of performance and memory?
51. What is the difference between `Collections.unmodifiableList` and `List.of`?
52. How would you implement a custom `Set` that maintains insertion order but allows fast retrieval?
53. Discuss the internals of `BitSet`.
54. Explain the use of `Spliterator` in Java 8.
55. How do you sort a `Map` by value?
56. What is the internal storage strategy of `ArrayList` during resizing?
57. Explain the "Contract between `equals()` and `hashCode()`". What happens if only one is overridden?
58. Discuss the **Diamond Problem** in the context of Default Methods in interfaces vs Collections.
59. How does `EnumSet` achieve its high performance? (Bit-vectors)
60. What is a **WeakHashMap** and how does it interact with the GC?

## Section 4: Java 8 to Java 21+ Features (20 Questions)
61. Explain the **Stream API** pipeline (Intermediate vs Terminal operations).
62. How does `parallelStream()` decide how many threads to use? Can you customize the ForkJoinPool?
63. What are **Default and Static methods** in interfaces? Why were they introduced?
64. Explain **Optional**. Why should it not be used for class fields or constructor arguments?
65. What are **Method References**? List the four types.
66. Explain **Lambda Expression** serialization.
67. What are **Records**? How do they differ from a standard POJO? Can they be extended?
68. Explain **Sealed Classes and Interfaces**. What are the permitted subclasses?
69. Discuss **Pattern Matching for switch** and **Record Patterns**.
70. What is **Text Blocks**? How do they handle indentation?
71. What is the **Foreign Function & Memory API (Panama)**?
72. Explain **Vector API** (Incubator).
73. What are **Switch Expressions** (yield vs return)?
74. How does `var` (Local Variable Type Inference) work? Does it affect performance?
75. Explain the improvements to `HttpClient` in Java 11.
76. What is the purpose of `StackWalker` API?
77. Discuss the impact of **Unmodifiable Collections** (Java 10+).
78. Explain **String Templates** (Java 21).
79. What are **Unnamed Classes and Instance Main Methods**?
80. How has `Thread.stop()` and `Thread.resume()` been handled in modern Java?

## Section 5: Design Patterns & SOLID (15 Questions)
81. Explain the **Strategy Pattern** and how Lambdas in Java 8 make it easier to implement.
82. Difference between **Factory and Abstract Factory** patterns with Java examples.
83. How do you implement a **Thread-Safe Singleton** (Bill Pugh's approach)?
84. Explain the **Proxy Pattern**. How is it used in Spring AOP?
85. What is the **Observer Pattern**? How is it different from the Pub-Sub model?
86. Describe the **Decorator Pattern**. How is it used in Java I/O classes?
87. What is the **Chain of Responsibility** pattern?
88. Explain the **Liskov Substitution Principle (LSP)** with a Java violation example.
89. How does the **Dependency Inversion Principle** relate to Spring's Dependency Injection?
90. Explain the **S** in SOLID (Single Responsibility) and how to avoid "God Classes."
91. What is the **Adapter Pattern**?
92. Explain the **Facade Pattern**.
93. How would you implement the **Flyweight Pattern** in Java for a massive object cache?
94. Explain the **Command Pattern**.
95. What is the **Visitor Pattern**?

## Section 6: Exception Handling & Best Practices (15 Questions)
96. Discuss **Checked vs Unchecked Exceptions**. Why is there a trend towards using Unchecked exceptions?
97. Explain **Try-with-Resources**. How does it handle multiple resources and "Suppressed Exceptions"?
98. What is the cost of throwing an exception in Java? (Stack trace generation)
99. How would you create a custom **AutoCloseable** resource?
100. Best practices for logging exceptions (SLF4J, Log4j2).
101. What is **Multi-catch** in Java 7?
102. Explain `ExceptionInInitializerError`. When does it occur?
103. Difference between `NoClassDefFoundError` and `ClassNotFoundException`.
104. How do you avoid "Swallowing Exceptions"?
105. What is the purpose of `assert` in Java?
106. Explain the **Fail-fast** principle in exception handling.
107. Can you catch `Error`? Under what circumstances should you?
108. What is the difference between `final`, `finally`, and `finalize()`?
109. How does `finally` interact with `System.exit()` or thread interruption?
110. Explain the **rethrow** capability in Java 7+.

## Section 7: Serialization & Networking (10 Questions)
111. How does **Java Serialization** work? What is `serialVersionUID`?
112. Explain the **Externalizable** interface vs `Serializable`.
113. How do you prevent sensitive fields from being serialized? (`transient` keyword)
114. What are the security risks associated with Java Deserialization?
115. Explain **Socket Programming** in Java (NIO vs OIO).
116. How does **TCP/IP** interaction work in Java sockets?
117. What is **Object Input/Output Stream inheritance**?
118. How do you implement custom serialization using `writeObject` and `readObject`?
119. Explain **RMI (Remote Method Invocation)** basics.
120. Discuss the replacement of Serialization with frameworks like **Protobuf or Jackson**.

## Section 8: IO & NIO (10 Questions)
121. Difference between **java.io** and **java.nio**.
122. Explain **Selectors, Channels, and Buffers** in NIO.
123. What is a **MappedByteBuffer**?
124. Explain **Scatter-Gather I/O**.
125. How does `Files.walk()` differ from `Files.list()`?
126. What is the **AsynchronousFileChannel**?
127. Explain **Zero-copy** in Java using `transferTo()`.
128. What is the difference between `Reader/Writer` and `InputStream/OutputStream`?
129. How do you handle file encoding in Java IO?
130. Explain the **New File System (NIO.2)** introduced in Java 7.

## Section 9: Performance Tuning & Profiling (10 Questions)
131. How would you perform **Escape Analysis**?
132. What is **Inlining** in JIT?
133. Explain **Dead code elimination**.
134. How do you use **JMH (Java Microbenchmark Harness)**? Why is it necessary?
135. What is the **Safe Point** in JVM? How do "Safe Point Polls" affect performance?
136. Explain **False Sharing** and its impact on L1/L2 cache.
137. How do you read a **Heap Dump**?
138. Discuss **GC tuning parameters** (e.g., `-Xmx`, `-XX:MaxGCPauseMillis`).
139. What is the **Service Provider Interface (SPI)**?
140. How do you profile a CPU-intensive Java application?

## Section 10: Miscellaneous Advanced Topics (10 Questions)
141. Explain **Reflection API**. What are its performance drawbacks?
142. How does **Dynamic Proxy** (`java.lang.reflect.Proxy`) work?
143. What is **Unsafe Class** in Java? Why is it being restricted?
144. Explain **Annotation Processing** (JSR 269).
145. What are **Java Agents** and how does `Instrumentation` work?
146. Discuss **Module System (Project Jigsaw)** (Java 9+).
147. Explain `ThreadLocalRandom` vs `Random`.
148. What is the **Compact Strings** feature in Java 9?
149. How does `System.gc()` work? Does it guarantee garbage collection?
150. Compare **JAR, WAR, and EAR** files. How has the cloud changed their usage?

---
*Generated by Antigravity for SDE3 Interview Prep.*
