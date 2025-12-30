# 150 Java Stream API (Java 8-21+) Interview Questions (SDE3)

## 1. Stream API Fundamentals (25 Questions)
1. What is the **Java Stream API**? How does it differ from the **Collections Framework**?
2. Explain the core philosophy: **Declarative vs. Imperative** data processing.
3. What are the three parts of a Stream pipeline: **Source, Intermediate Operations, and Terminal Operation**?
4. What is **Lazy Evaluation** in Streams? How does it improve performance?
5. Explain the concept of **Pipelining** and **Internal Iteration**.
6. What is the difference between a **Stream** and an **Iterator**? Can a Stream be reused?
7. How do Streams handle **Side Effects**? Why is the functional approach preferred?
8. Explain **Short-circuiting** operations. Give examples of intermediate and terminal short-circuiting operations.
9. What is the difference between `Stream.of()`, `Arrays.stream()`, and `Collection.stream()`?
10. Explain **Intermediate Operations** vs. **Terminal Operations**. Which ones return a new Stream?
11. What is an **Intermediate Stateless** operation vs. an **Intermediate Stateful** operation?
12. Why are stateful operations (like `sorted()`, `distinct()`) more expensive?
13. Explain **Ordering** in Streams (`map` vs `sorted` in parallel streams).
14. What is the **BaseStream** interface?
15. How do you create a Stream from a **File** (`Files.lines`)?
16. How do you create a Stream from a **String** (`chars()`)?
17. Explain the **Builder Pattern** in Streams (`Stream.builder()`).
18. What is the difference between `Collection.parallelStream()` and `Stream.parallel()`?
19. How do you create an **Infinite Stream** using `Stream.iterate()` or `Stream.generate()`?
20. What is the role of the **Spliterator** in the Stream API?
21. Explain **Stream.concat()**. What are its performance implications for large pipelines?
22. How to convert a Stream back to a **Collection** or **Array**?
23. What is the impact of **Nulls** in a Stream?
24. Explain **Stream.empty()**.
25. How has the Stream API evolved from **Java 8 to Java 21**?

## 2. Intermediate Operations & Transformations (25 Questions)
26. Explain **`filter(Predicate)`**. Is it stateless?
27. Explain **`map(Function)`**. What is its primary use case?
28. What is the difference between **`map()`** and **`flatMap()`**?
29. When should you use **`flatMap()`**? (Dealing with nested collections or Optionals).
30. Explain **`peek(Consumer)`**. Why is it primarily for debugging? Does it trigger evaluation?
31. What is the difference between `peek()` and `forEach()`?
32. Explain **`distinct()`**. How does it identify unique elements (`equals()` vs `hashCode()`)?
33. Explain **`sorted()`** and `sorted(Comparator)`. Is it a short-circuiting operation?
34. What is **`limit(long)`**? How does it behave in parallel streams?
35. What is **`skip(long)`**?
36. Explain **`mapToInt()`, `mapToLong()`, and `mapToDouble()`**. Why are they preferred over `map()` for primitives?
37. What is **`boxed()`** mapping?
38. Explain **`takeWhile(Predicate)`** (Java 9). How does it differ from `filter()`?
39. Explain **`dropWhile(Predicate)`** (Java 9).
40. How to implementation **Pagination** using `skip()` and `limit()`?
41. Can you use **`map()`** to change the type of a Stream (e.g., `Stream<String>` to `Stream<Integer>`)?
42. Explain the risks of **Stateful Lambda Expressions** in intermediate operations.
43. How to filter elements based on their **Index** (using `IntStream` ranges)?
44. Explain **`Stream.ofNullable()`** (Java 9).
45. How to handle **Exceptions** inside a `map()` or `filter()` block? (Wrapper functions/Either pattern).
46. What is the **Performance overhead** of frequent mapping?
47. How to implement **Conditional Mapping**?
48. Explain **Stream.flatMapToDouble** and similar primitive variants.
49. What is the difference between `map(Optional::get)` and `flatMap(Optional::stream)`?
50. How does the **Order of operations** in a pipeline affect performance (filter-first vs map-first)?

## 3. Terminal Operations: Collect, Reduce & Find (30 Questions)
51. What is a **Terminal Operation**? Why triggers the execution of the entire pipeline?
52. Explain **`forEach(Consumer)`** vs. **`forEachOrdered(Consumer)`**. When does the latter matter?
53. Explain **`collect(Collector)`**. What is the **Collectors** utility class?
54. What is the difference between **`reduce()`** and **`collect()`**?
55. Explain the three parameters of **`reduce(identity, accumulator, combiner)`**.
56. Why is the **Combiner** necessary in the `reduce()` method?
57. Explain **`Collectors.toList()`** vs. **`Stream.toList()`** (Java 16). Which one is immutable?
58. Explain **`Collectors.toSet()`** and **`Collectors.toCollection()`**.
59. What is **`Collectors.toMap()`**? How do you handle **Duplicate Keys** with a merge function?
60. Explain **`Collectors.groupingBy()`**. What is a **Classification Function**?
61. What is **`groupingBy(classifier, downstream)`**? Provide an example of nested grouping.
62. Explain **`Collectors.partitioningBy()`**. How does it differ from `groupingBy()`?
63. What is **`Collectors.joining()`**? How to use delimiters, prefixes, and suffixes?
64. Explain **`Collectors.counting()`**, **`summingInt()`**, and **`averagingDouble()`**.
65. What is **`Collectors.summarizingInt()`**? What data does the `IntSummaryStatistics` provide?
66. Explain **`Collectors.mapping()`** and **`flatMapping()`** (downstream collectors).
67. What is **`Collectors.reducing()`**?
68. Explain **`Collectors.collectingAndThen()`**.
69. What is **`Collectors.teeing()`** (Java 12)? Provide a use case for simultaneous data aggregation.
70. Explain **`findFirst()`** vs. **`findAny()`**. Which one is faster in parallel streams?
71. Explain **`anyMatch(Predicate)`**, **`allMatch(Predicate)`**, and **`noneMatch(Predicate)`**.
172. Are matching operations (`anyMatch`, etc.) short-circuiting?
73. Explain **`max(Comparator)`** and **`min(Comparator)`**.
74. What is the **`count()`** terminal operation? How has it been optimized in recent Java versions?
75. Explain **`toArray()`**. How to use the constructor reference `String[]::new`?
76. What is the impact of **Side effects** inside `collect()`?
77. How to create a **Custom Collector** by implementing the `Collector` interface?
78. What are the 4 functions of a `Collector`: **supplier, accumulator, combiner, finisher**?
79. What is the **`Characteristics`** of a Collector (IDENTITY_FINISH, CONCURRENT, UNORDERED)?
80. How to handle **empty streams** in `reduce()`? (Optional result).

## 4. Parallel Streams & Performance Internals (25 Questions)
81. How does a **Parallel Stream** work? (Fork/Join Framework).
82. What is the **Common ForkJoinPool**? How do you control its parallelism level?
83. When should you **NOT** use parallel streams? (Small data, high complexity, stateful ops).
84. Explain the **Performance overhead** of thread management in parallel streams.
85. How does the **Source data structure** affect parallel performance (ArrayList vs. LinkedList)?
86. Why is a **Spliterator** for a `HashMap` better than for a `LinkedList`?
87. What is the impact of **Boxing/Unboxing** on stream performance?
88. Explain the **N * Q model** for deciding whether to go parallel.
89. How to change the **Parallelism** level for a specific stream (using a custom ForkJoinPool)?
90. What is the danger of **Shared Mutable State** in parallel streams?
91. Explain how **`limit()`** and **`skip()`** can kill performance in parallel mode.
92. What is the difference between **`Stream.parallel()`** and **`Stream.sequential()`**? Can a stream be both?
93. How does **`sorted()`** behave in a parallel stream?
94. Explain the **Combiner** logic for the `collect()` operation in parallel.
95. What is the cost of **Thread-local storage** in functional pipelines?
96. How to monitor **Stream Pipeline Performance**?
97. Explain **Pipeline Fusing** and **Loop unrolling** in JIT for streams.
98. What is the impact of **False Sharing** on stream processing?
99. How does the **`Collector.Characteristics.CONCURRENT`** affect parallel `collect()`?
100. Discuss the future of Parallel Streams with **Project Loom (Virtual Threads)**.
101. What is the difference between parallel streams and **CompletableFuture.allOf()**?
102. How to handle **Thread-safety** when using `Collectors.toConcurrentMap()`?
103. Does `forEach()` preserve order in parallel streams?
104. What is the behavior of **`findFirst()`** in parallel?
105. How to avoid **Common Pool Saturation**?

## 5. Primitive & Specialized Streams (20 Questions)
106. What are **`IntStream`, `LongStream`, and `DoubleStream`**?
107. Why were these specialized interfaces introduced? (Avoiding `Integer` object overhead).
108. Explain **`range(start, end)`** vs. **`rangeClosed(start, end)`** in `IntStream`.
109. How to convert a specialized stream back to an object stream (`boxed()`)?
110. Explain the **Aggregation methods** available only in primitive streams: `sum()`, `average()`, `summaryStatistics()`.
111. What is **`IntStream.asLongStream()`** and **`asDoubleStream()`**?
112. How to use **`IntStream.iterate()`**?
113. How to generate **Random Numbers** using `Random.ints()`?
114. What is **`LongStream.of()`**?
115. How to convert `IntStream` to `List<Integer>`?
116. Explain the **Bitwise operations** using primitive streams.
117. What is the **performance difference** between `Stream<Integer>` and `IntStream` for 1M elements?
118. How to use **`mapToObj()`**?
119. Explain **`Stream.generate(Supplier)`** for constant streams.
120. How to use Specialized Streams for **Coordinate Geometry**?
121. What is the **`OptionalInt`**, **`OptionalLong`**, and **`OptionalDouble`**?
122. How to convert a specialized stream to an array (`toArray()`)?
123. Can you use `Collectors` with primitive streams directly?
124. How to handle **Overflows** in `IntStream.sum()`?
125. Use cases for **`LongStream.range()`** in high-throughput timers.

## 6. Advanced Patterns, Optional & Troubleshooting (25 Questions)
126. How to use **`Optional`** with the Stream API?
127. Explain **`Stream.mapMulti()`** (Java 16). How is it a more efficient alternative to `flatMap()`?
128. What is the **Stream of Optionals** pattern? How to filter out empty optionals efficiently?
129. How to implementation **Custom Spliterators** for complex data sources?
130. Explain **`Stream.gatherers()`** (Java 22 Preview/JEP 461). How it revolutionizes custom intermediate operations?
131. What is **Short-circuiting terminal operation** vs **Short-circuiting intermediate operation**?
132. How to handle **Nested Stream Pipelines**? (Performance vs. Readability).
133. Explain **Recursion** using the Stream API.
134. How to perform **Batching** of a Stream (processing elements in chunks of N)?
135. What are **Checked Exceptions** inside Lambda expressions (The "ThrowingLambda" workaround)?
136. Explain **Data Locality** in Stream processing.
137. How to identify **Memory Leaks** caused by infinite streams?
138. What is the impact of **Stream pipeline length** on stack depth?
139. How to **Debug a Stream** using a debugger (Visualize Stream Operations in IntelliJ)?
140. How to use **Stream API with SQL Results** (via JPA/JDBC)?
141. Explain **Stream.peek()** limitations in Java 9+ due to optimization (if the result is predictable, peek might not run).
142. How to implement **Teeing** (Splitting a stream into two pipelines and merging results)?
143. Use of **`Collections.unmodifiableList(collect(...))`** vs **`toList()`**.
144. How to perform **Zipping** (merging two streams into a stream of pairs)?
145. What is the **Effectively Final** requirement for variables used in stream lambdas?
146. How to avoid **Boxing** in `Collectors.summingInt()`?
147. Explain **Stream-based File processing** for multi-GB files.
148. How to handle **Resource Closing** in Streams (using try-with-resources with `Files.lines`)?
149. What is the difference between **Imperative loops** and **Functional streams** in terms of bytecode?
150. Summary: When should you **NOT** use a Stream? (Readability, very tight performance loops, primitives manipulation).

---
*Generated by Antigravity for SDE3 Interview Prep.*
