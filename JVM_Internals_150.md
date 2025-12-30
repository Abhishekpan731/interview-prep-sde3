# 150 JVM Internals & Memory Management Interview Questions (SDE3)

## 1. JVM Architecture (20 Questions)
1. Explain the high-level architecture of the JVM (Classloader, Runtime Data Areas, Execution Engine).
2. What are the different types of ClassLoaders? Explain the Delegation Model.
3. How does the "Bootstrap ClassLoader" differ from others since it's not a Java object?
4. What is the role of `Class.forName()` vs `ClassLoader.loadClass()`?
5. Deep dive into the "Execution Engine" components: Interpreter, JIT Compiler, and GC.
6. What is the "Constant Pool" in a class file vs the "Run-time Constant Pool"?
7. Explain the "Native Method Stack" and its purpose.
8. How does the JVM handle thread-local storage for program counters and stacks?
9. What is "Metaspace"? How is it different from "PermGen" in older versions?
10. What is "Compressed Class Space" within Metaspace?
11. Explain "Direct Memory" and how it's managed via `java.nio.DirectByteBuffer`.
12. What are "Safe Points" and "Safe Regions"? Why are they critical for GC?
13. How does the JVM handle Object Header layout (Mark Word and Class Metadata Address)?
14. What is "Biased Locking" and why was it deprecated/removed in recent Java versions?
15. Explain "Tiered Compilation" (Levels 0 to 4).
16. How does "Code Cache" management work and what happens when it's full?
17. What are "Intrinsics" in the JVM?
18. Explain "Escape Analysis" and how it enables "Scalar Replacement" and "Stack Allocation."
19. Discuss "On-Stack Replacement (OSR)".
20. How does the JVM facilitate "Dynamic Proxy" generation at the bytecode level?

## 2. Java Memory Model (JMM) (20 Questions)
21. Define the JMM and its necessity in multi-core systems.
22. Explain the "Happens-Before" relationship in depth.
23. How does `volatile` ensure visibility and prevent instruction reordering?
24. Explain the difference between "Total Store Ordering" (TSO) and "Weak Memory Models" (ARM/PowerPC) and how JVM abstracts this.
25. What is "Sequential Consistency" vs "Linearizability"?
26. How do "Memory Barriers" (LoadLoad, LoadStore, etc.) work in the JVM?
27. Discuss the "Final Field Freeze" rule.
28. What are the synchronization actions in JMM?
29. How does the "Piggybacking" technique work with volatile variables?
30. Explain "Double-Checked Locking" and the role of the volatile keyword in its fix.
31. How does the JVM handle 64-bit `long` and `double` atomicity challenges?
32. What is the impact of "Cache Lines" and "False Sharing" on the JMM?
33. How does `@Contended` help with cache line alignment?
34. Explain the "Out-of-Thin-Air" safety property in JMM.
35. How does JVM handle "Data Races" vs "Race Conditions"?
36. Discuss the interaction between JMM and the underlying CPU cache coherence protocols (MESI).
37. What is "Release-Acquire" semantics?
38. Explain the "Monitor Lock" semantics in JMM.
39. How does JMM allow for compiler optimizations like loop unrolling and constant folding?
40. Discuss the future of JMM (Project Panama and Foreign Memory).

## 3. Garbage Collection Algorithms (30 Questions)
41. Explain the "Generational Hypothesis."
42. Compare "Reference Counting" vs "Tracing Collectors."
43. How does "Mark-Sweep-Compact" work?
44. Explain "Copying Collectors" and why survivors spaces exist.
45. Deep dive into **G1 GC**: Regions, Humongous Objects, and Remembered Sets (RSets).
46. What is "SATB" (Snapshot-At-The-Beginning) in G1?
47. How does the "Pause Time Goal" (`-XX:MaxGCPauseMillis`) influence G1 behavior?
48. Explain the **ZGC** (Z Garbage Collector): Colored Pointers and Load Barriers.
49. How does ZGC achieve <1ms pause times regardless of heap size?
50. What is **Shenandoah GC** and how does "Brooks Pointers" comparison work?
51. Compare G1, ZGC, and Shenandoah.
52. Explain "Stop-the-World" (STW) pauses.
53. What is "Concurrent Marking"?
54. Discuss "Parallel Scavenge" vs "ParNew."
55. Explain "CMS" and why it was deprecated (Fragmentation and Floating Garbage).
56. What is the "Card Table" and how is it used during partial GCs?
57. Explain "Write Barriers" in GC.
58. What is the "Promotion Failure" and "Concurrent Mode Failure"?
59. How does the JVM decide when to trigger a "Full GC"?
60. Explain "TLAB" (Thread Local Allocation Buffer) and its performance benefits.
61. What is "PLAB" (Promotion Local Allocation Buffer)?
62. How is "Eden" sized dynamically?
63. Explain "Tenuring Threshold" and "Adaptive Size Policy."
64. Discuss "Object Aging" process.
65. What are "Root Sets" in GC tracing?
66. Explain the "Liveness" calculation.
67. Discuss "Compaction Strategies" (Moving vs Non-moving).
68. What is "Generational ZGC" (introduced in Java 21)?
69. Explain "Epsilon GC" (No-op) and its use cases.
70. How does GC handle "Soft References" vs "Weak References"?

## 4. Advanced JVM Tuning & Monitoring (20 Questions)
71. Difference between `-Xms` and `-Xmx`. Why equal them in production?
72. How do you tune the `-XX:NewRatio` and `-XX:SurvivorRatio`?
73. Explain `-XX:+PrintGCDetails` vs `-Xlog:gc` (Unified Logging).
74. How do you interpret a GC log?
75. What is the "Heap Dump" and how to analyze it with Eclipse MAT?
76. Explain "Shallow Heap" vs "Retained Heap."
77. How to identify "GCRoots" for a leaking object?
78. What is "JFR" (Java Flight Recorder)?
79. Explain "JMC" (Java Mission Control).
80. How to use `jstat` to monitor GC in real-time?
81. What is `jstack` and how to find thread contention?
82. Explain `jmap` and security/performance risks of using it on live apps.
83. How to use `jcmd` as a swiss-army knife tool?
84. Tuning for "High Throughput" vs "Low Latency."
85. Explain `-XX:+UseStringDeduplication`.
86. What is "Compressed OOPs" (`-XX:+UseCompressedOops`)?
87. How does the JVM handle swapping/paging?
88. Monitoring "Native Memory Tracking" (NMT).
89. How to tune "Metaspace" limits?
90. Explain the impact of `-XX:MaxDirectMemorySize`.

## 5. Bytecode & Classloading (20 Questions)
91. What is the structure of a `.class` file?
92. Explain "Magic Number" and "Version" fields in bytecode.
93. What is "InvokeDynamic" (indy)? Why was it added in Java 7?
94. Explain `invokestatic`, `invokespecial`, `invokevirtual`, `invokeinterface`.
95. How do Lambdas get translated into bytecode?
96. Discuss "ASM" and "ByteBuddy" libraries.
97. What is "Class Data Sharing (CDS)" and "AppCDS"?
98. Explain "Dynamic Class Loading."
99. How to implement a custom ClassLoader for hot-reloading?
100. Discuss the "Verification" phase of classloading.
101. What is "Class Unloading"?
102. Explain the "Linking" process: Verification, Preparation, and Resolution.
103. How does "Reflection" bypass Java access controls?
104. What is "Method Handles" API (`java.lang.invoke`)?
105. Performance difference between Reflection and MethodHandles.
106. Explain "Service Loader" (SPI) and its classloading implications.
107. Discuss "OSGi" classloading vs standard JVM classloading.
108. What is the impact of Java Modules (Project Jigsaw) on class visibility?
109. Explain "Shading" (Maven Shade Plugin) and why it's needed for class collisions.
110. How to debug `ClassNotFoundException` vs `NoClassDefFoundError`.

## 6. Advanced JIT Optimizations (15 Questions)
111. Explain "Dead Code Elimination."
112. What is "Loop Unrolling"?
113. Explain "Method Inlining" depth and limits.
114. Discuss "Monomorphic" vs "Polymorphic" inline caches.
115. What are "Deoptimizations"? When do they happen?
116. Explain "Null Check Elimination."
117. What is "Bound Check Elimination" for arrays?
118. Discuss "Strength Reduction."
119. What is "Lock Coarsening" vs "Lock Elision"?
120. Explain "Vectorization" (Auto-vectorization).
121. How does JIT handle "Virtual Calls"?
122. Explain "Profiling" used by C1/C2 compilers.
123. Discuss the "Graal VM" and its "AOT (Ahead-of-Time)" compilation.
124. What is "Native Image" in GraalVM?
125. Benefits of GraalVM's "Truffle Framework."

## 7. Troubleshooting Real-world Scenarios (25 Questions)
126. "Application slows down after 2 hours." How to diagnose?
127. "Metaspace OOM" but no heavy classloading. Potential causes?
128. "High CPU usage by GC threads." What to tune?
129. "Long STW pauses but heap is only 50% full."
130. "Native Memory Leak" diagnosis steps.
131. "False Sharing" symptoms and mitigation.
132. Identifying "Memory Leaks" in Thrid-party libraries.
133. Troubleshooting "Thread Deadlocks" vs "Livelocks."
134. "Slow Reflection" vs "MethodHandles" in high-frequency trading.
135. Dealing with "Humongous Allocations" in G1.
136. "Mixed GC" tuning in G1.
137. Effect of large pages (`-XX:+UseLargePages`) on JVM memory.
138. Troubleshooting "Direct Memory OOM."
139. How to handle `StackOverflowError` in deep recursions.
140. Performance impact of "Large Heap" (>100GB).
141. Tuning for "Containerized JVM" (Docker memory limits).
142. Explain `-XX:+UseContainerSupport`.
143. Impact of CPU quotas on JVM thread pools and GC.
144. Diagnosing "Stop-the-World" pauses caused by OS issues (e.g., swapping).
145. Identifying "Contended Locks" using `async-profiler`.
146. How to profile "Memory Allocation Rate" vs "Throughput."
147. Analysis of "Object Allocation" hot spots.
148. Troubleshooting "Socket/File Descriptor" leaks.
149. "JIT compilation failure" or crashes.
150. Future of JVM: Project Valhalla (Value Types) and its memory implications.

---
*Generated by Antigravity for SDE3 Interview Prep.*
