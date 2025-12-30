# 150 Concurrency & Multi-threading Interview Questions (SDE3)

## 1. Concurrency Fundamentals (20 Questions)
1. Explain the difference between Process and Thread.
2. What is "Concurrency" vs "Parallelism"?
3. Describe the Life Cycle of a Thread in Java.
4. What is the difference between `start()` and `run()` methods in the `Thread` class?
5. How can you create a thread in Java (Thread class, Runnable, Callable)?
6. What is a "Daemon Thread"? Give a real-world example.
7. Explain the "Main Thread" and its role.
8. What is "Thread Priority"? Does it guarantee execution order?
9. Explain `Thread.sleep()` vs `Thread.yield()`.
10. What is `join()` and how does it work internally?
11. How do you stop a thread in Java? Why is `Thread.stop()` deprecated?
12. Explain "Thread Interruption" mechanism. How do `isInterrupted()` and `interrupted()` differ?
13. What is a "Race Condition"? Provide a code snippet example.
14. What is "Mutual Exclusion" (Mutex)?
15. Explain "Context Switching" and its overhead.
16. What is a "Thread Leak" and how to detect it?
17. Explain "Visibility" and "Atomicity" in the context of multi-threading.
18. What is "Liveness" (Deadlock, Livelock, Starvation)?
19. How does "Amdahl’s Law" relate to multi-threaded applications?
20. Explain "Thread Safety" and how to achieve it.

## 2. Synchronization & Locks (25 Questions)
21. Explain the `synchronized` keyword (Method vs Block).
22. What is an "Intrinsic Lock" (Monitor Lock)?
23. How does "Reentrant Synchronization" work?
24. Compare `synchronized` vs `ReentrantLock`.
25. Explain the concept of "Fairness" in locks.
26. What is a "ReadWriteLock"? When should it be used?
27. Describe `ReentrantReadWriteLock` internals.
28. What is a "StampedLock" and how does "Optimistic Reading" work?
29. Explain `wait()`, `notify()`, and `notifyAll()`. Why must they be called from a synchronized context?
30. Difference between `notify()` and `notifyAll()`. When is `notify()` dangerous?
31. What is the "Spurious Wakeup" problem?
32. Explain the "Condition" interface in `java.util.concurrent.locks`.
33. How do you implement a "Producer-Consumer" problem using `ReentrantLock` and `Condition`?
34. What is "Lock Striping"? (Reference `ConcurrentHashMap`).
35. What is "Lock Coarsening" and "Lock Elision"?
36. Describe the "Double-Checked Locking" pattern.
37. What is "Hierarchical Locking"?
38. Explain "Spinlocks" vs "Blocking Locks."
39. What is "Adaptive Spinning" in JVM?
40. How to find which thread holds a lock using `jstack`?
41. What is a "Distributed Lock"?
42. Explain "Optimistic Locking" vs "Pessimistic Locking."
43. How does `Thread.holdsLock(obj)` work?
44. What is the "Bakery Algorithm" for synchronization?
45. Discuss the impact of `synchronized` on JVM optimizations.

## 3. High-Level Concurrency Utilities (JUC) (25 Questions)
46. What is the **Executor Framework**?
47. Compare `Executor`, `ExecutorService`, and `ScheduledExecutorService`.
48. Explain the different types of **ThreadPools** (`Fixed`, `Cached`, `Single`, `Scheduled`).
49. How do you tune the parameters of `ThreadPoolExecutor` (Core size, Max size, Keep-alive)?
50. What are the different "Rejection Policies" in `ThreadPoolExecutor`?
51. Explain the "Work Stealing" algorithm in `ForkJoinPool`.
52. What is the difference between `submit()` and `execute()`?
53. How does `Callable` differ from `Runnable`?
54. What is a `Future`? How do you handle exceptions in a `Future`?
55. Explain `CompletableFuture`. How does it facilitate asynchronous programming?
56. Explain `thenApply`, `thenAccept`, `thenRun` in `CompletableFuture`.
57. Difference between `thenCompose` and `thenCombine`.
58. How do you handle multiple `CompletableFutures` (e.g., `allOf`, `anyOf`)?
59. What is a `CountDownLatch`? Give a use case.
60. What is a `CyclicBarrier`? How does it differ from `CountDownLatch`?
61. Explain `Semaphore`. How is it used for rate limiting?
62. What is an `Exchanger`?
63. Explain `Phaser`. When is it better than `CyclicBarrier`?
64. How does `CompletionService` work?
65. Discuss `java.util.concurrent.atomic` package.
66. Explain `AtomicInteger`, `AtomicLong`, `AtomicReference`.
67. What is `CAS` (Compare-And-Swap)?
68. Explain the "ABA Problem" and how `AtomicStampedReference` solves it.
69. What are `LongAdder` and `DoubleAdder`? Why are they faster than `AtomicLong` under high contention?
70. Explain `LongAccumulator`.

## 4. Concurrent Collections (20 Questions)
71. Why is `Vector` and `Hashtable` considered legacy?
72. Explain `Collections.synchronizedList()` vs `CopyOnWriteArrayList`.
73. Internal working of `ConcurrentHashMap` in Java 8.
74. How does `ConcurrentHashMap` handle sizing and resizing without blocking?
75. What is the `computeIfAbsent` and `merge` method in `ConcurrentHashMap`?
76. Explain `ConcurrentSkipListMap` and its performance characteristics.
77. What is a `BlockingQueue`? List its implementations (`LinkedBlockingQueue`, `ArrayBlockingQueue`, `SynchronousQueue`).
78. What is the difference between `poll()`, `take()`, and `remove()` in a `BlockingQueue`?
79. Explain `PriorityBlockingQueue`.
80. What is `DelayQueue`? Give a real-world example (e.g., Cache eviction).
81. Explain `TransferQueue` (`LinkedTransferQueue`).
82. How does `CopyOnWriteArraySet` work?
83. What is the performance trade-off of `CopyOnWrite` collections?
84. How do you iterate over a concurrent collection safely?
85. Explain the "Fail-fast" vs "Fail-safe" iterators.
86. Why does `ConcurrentHashMap` not allow `null` keys or values?
87. Compare `ConcurrentLinkedQueue` vs `LinkedBlockingQueue`.
88. What is `Disruptor` (LMAX)? How does it achieve high throughput?
89. Explain "Ring Buffer" in the context of concurrency.
90. How to implement a custom Thread-safe collection?

## 5. Modern Concurrency: Project Loom & Beyond (20 Questions)
91. What are **Virtual Threads** (Project Loom)?
92. How do Virtual Threads differ from Platform (Kernel) Threads?
93. Explain the "Carrier Thread" concept in Loom.
94. What is "Mounting" and "Unmounting" of virtual threads?
95. How do Virtual Threads handle blocking I/O?
96. Discuss the performance benefits of using 1 million virtual threads.
97. What is **Structured Concurrency**?
98. Explain `StructuredTaskScope`.
99. What are **Scoped Values**? How do they replace `ThreadLocal`?
100. Why is `ThreadLocal` problematic for Virtual Threads?
101. Explain "Pinning" in Virtual Threads. How to avoid it?
102. How do Virtual Threads interact with `synchronized` blocks?
103. Discuss "Reactive Programming" vs "Virtual Threads."
104. What is the **Flow API** (Java 9)?
105. Explain `Publisher`, `Subscriber`, `Subscription`, and `Processor`.
106. How does "Backpressure" work in Flow API?
107. Discuss the impact of Project Loom on existing Thread Pools.
108. Explain the "Continuation" concept in Loom.
109. What is "Wait-free" vs "Lock-free" programming?
110. How to migrate a legacy app to use Virtual Threads?

## 6. Java Memory Model (Concurrency focus) (20 Questions)
111. What is the role of `volatile` in thread visibility?
112. Does `volatile` ensure atomicity? (e.g., `i++`).
113. Explain the "Happens-before" guarantee of `volatile` writes.
114. How does `final` field safety work in multi-threaded environments?
115. What is "Safe Publication"?
116. Explain "Immutable Objects" and why they are inherently thread-safe.
117. What is "Thread Confinement"?
118. Explain `ThreadLocal` usage and memory leak risks.
119. How does `InheritableThreadLocal` work?
120. Discuss "Memory Barriers" and "Fences" (Java 8 `Unsafe.loadFence()`, `VarHandle`).
121. What are **VarHandles** (Java 9)? How do they improve on `Atomic` classes?
122. Compare `VarHandle` vs `Reflection` for field access.
123. Explain "Release/Acquire" semantics in `VarHandle`.
124. What are "Opaque" and "Plain" memory access modes?
125. How to implement a "Non-blocking stack" (Treiber Stack)?
126. How to implement a "Non-blocking queue" (Michael-Scott Queue)?
127. What is "False Sharing" and how to mitigate it in Java 8+?
128. Explain the cost of `volatile` on modern CPU architectures.
129. How does the JVM handle `synchronized` on class objects vs instance objects?
130. Explain "Biased Locking" removal and its effect on concurrency performance.

## 7. Troubleshooting & Best Practices (20 Questions)
131. How to detect a "Deadlock" Programmatically?
132. How to avoid Deadlocks (Lock ordering, TryLock)?
133. What is "Starvation"? How can thread priority cause it?
134. Explain "Livelock" with an example.
135. How to analyze a "Thread Dump"?
136. Identifying "Thread Contention" using JVisualVM or JConsole.
137. How to find CPU-consuming threads in Linux for a Java process?
138. Best practices for naming threads.
139. Why should you never use `Thread.run()`?
140. When to use `Parallel Streams` vs `ExecutorService`?
141. Risks of using `parallelStream()` for I/O bound tasks.
142. How to handle `InterruptedException` properly?
143. Why is it bad to swallow `InterruptedException`?
144. Designing a "Graceful Shutdown" for an ExecutorService.
145. "Thread Starvation Deadlock" in ThreadPools.
146. How to limit the number of active threads in a system?
147. Discuss the impact of "Hyper-threading" on Java concurrency.
148. How to test multi-threaded code? (Tools: Lincheck, JCStress).
149. Using `Semaphore` for database connection pooling.
150. Future of Concurrency: Project Valhalla and how value types affect atomics.

---
*Generated by Antigravity for SDE3 Interview Prep.*
