# 150 Java 8 to Java 21+ Features Interview Questions (SDE3)

## 1. Java 8: The Paradigm Shift (30 Questions)
1. What were the major themes of Java 8 release?
2. Explain **Lambda Expressions**. What is the syntax?
3. What are **Functional Interfaces**? List some built-in ones in `java.util.function`.
4. Difference between `@FunctionalInterface` and a regular interface.
5. Explain **Method References** and the four types (Static, Instance of particular object, Instance of arbitrary object, Constructor).
6. What is the **Stream API**? How does it differ from Collections?
7. Explain **Intermediate Operations** vs **Terminal Operations** in Streams.
8. What is **Lazy Evaluation** in Streams?
9. Explain `filter`, `map`, `flatMap`, `sorted`.
10. Difference between `map()` and `flatMap()`.
11. Explain terminal operations like `collect`, `forEach`, `reduce`, `findFirst`, `anyMatch`.
12. How does `Stream.reduce()` work?
13. What is a **Parallel Stream**? When should you use it?
14. How to handle exceptions inside a Lambda/Stream?
15. What are **Default Methods** in interfaces? Why were they introduced?
16. How does Java 8 solve the "Multiple Inheritance" (Diamond Problem) with default methods?
17. What are **Static Methods** in interfaces?
18. Explain the **Optional** class. What problem does it solve?
19. Difference between `Optional.of()`, `Optional.ofNullable()`, and `Optional.empty()`.
20. Best practices for using `Optional`. Why should it not be used for fields or parameters?
21. Explain the **Date and Time API** (`java.time`). Why was `java.util.Date` bad?
22. Key classes in the new Date API: `LocalDate`, `LocalTime`, `LocalDateTime`, `ZonedDateTime`, `Instant`, `Duration`, `Period`.
23. How to handle time zones in the new Date API?
24. What is `Nashorn` Javascript Engine (removed later, but relevant history)?
25. Explain **CompletableFuture** and its improvements over `Future`.
26. What is the **String.join()** method?
27. Explain **Base64** encoding/decoding support in Java 8.
28. How did **Type Annotations** improve?
29. What is the **metaspace** (Java 8 memory change)?
30. How to use `forEach` with a Map in Java 8?

## 2. Java 9: Modularity & Refinement (20 Questions)
31. What is **Project Jigsaw** (Java Module System)?
32. What is a `module-info.java` file?
33. Explain directives: `requires`, `exports`, `opens`, `uses`, `provides`.
34. What is the difference between `exports` and `opens`?
35. How does modularity help in limiting the JDK footprint (`jlink`)?
36. What is **JShell** (REPL)?
37. Explain **Private methods in Interfaces**.
38. New factory methods for collections: `List.of()`, `Set.of()`, `Map.of()`.
39. Improvements in the **Stream API** in Java 9 (`takeWhile`, `dropWhile`, `iterate`, `ofNullable`).
40. Improvements in **Optional** (`ifPresentOrElse`, `or`, `stream`).
41. What is the **HTTP/2 Client** (Incubator in 9, standardized in 11)?
42. Explain **Multi-Release JAR files**.
43. What is the **Process API** improvement?
44. Explain **StackWalker API**. Why is it better than `Thread.getStackTrace()`?
45. Changes in `@Deprecated` annotation (since and forRemoval).
46. What is the **Collection Factory** performance improvement?
47. How did Java 9 change the internal representation of `String` (Compact Strings)?
48. What is the impact of Java 9 on Reflection (Encapsulation)?
49. How to run a Java Application with Modules?
50. Explain **Resource-based Try-with-resources** enhancement in 9.

## 3. Java 10 & 11: LTS & Productivity (20 Questions)
51. What is **Local Variable Type Inference** (`var`)?
52. Rules for using `var`. Can it be used for fields or method parameters?
53. Does `var` affect performance or compile-time overhead?
54. **G1 Garbage Collector** improvements in Java 10 (Parallel Full GC).
55. What is **Application Class-Data Sharing (AppCDS)**?
56. Explain the standardization of **HTTP/2 Client** in Java 11.
57. How to send asynchronous requests with `HttpClient`?
58. New methods in the **String** class (Java 11): `isBlank`, `lines`, `repeat`, `strip`.
59. How to run a single-file source code directly (`java HelloWorld.java`)?
60. What are **Flight Recorder** and **Mission Control** becoming open source?
61. **Epsilon Garbage Collector** (No-op) introduction.
62. **ZGC** (Experimental in 11).
63. Removal of Java EE and CORBA modules from JDK 11.
64. Improvements in `Optional.isEmpty()`.
65. Explain **Nest-Based Access Control**.
66. How to use `var` in Lambda parameters (Java 11)?
67. What is **Dynamic Class-File Constants**?
68. Explain the licensing changes starting from Java 11 (Oracle JDK vs OpenJDK).
69. Performance improvements in `String.repeat()`.
70. How many years is the LTS support cycle for Java 11?

## 4. Java 12 to 16: Modern Syntax (20 Questions)
71. What are **Switch Expressions** (Standardized in 14)?
72. Explain the `yield` keyword in switch.
73. What are **Text Blocks** (Standardized in 15)?
74. How to handle whitespace and indentation in Text Blocks?
75. What are **Records** (Standardized in 16)?
76. What are the characteristics of a Record (Immutability, canonical constructor, accessors)?
77. Can a Record have static fields or additional methods?
78. What interfaces do Records automatically implement?
79. **Pattern Matching for instanceof** (Standardized in 16).
80. How does it eliminate casting?
81. What is the scope of the binding variable in pattern matching?
82. **Sealed Classes and Interfaces** (Preview in 15, standardized in 17).
83. What is **Helpful NullPointerExceptions** (Java 14)?
84. **Packaging Tool** (`jpackage`).
85. **Foreign-Memory Access API** (Incubator).
86. **Vector API** (Incubator).
87. Removal of the `Nashorn` engine and `Pack200` tool.
88. What is **Strong Encapsulation of JDK Internals** (Java 16)?
89. How to use `Stream.toList()` (Java 16) instead of `collect(Collectors.toList())`?
90. Performance of Records vs manually written POJOs.

## 5. Java 17: The Modern LTS (20 Questions)
91. Why is Java 17 considered the most stable modern version for enterprises?
92. Finalization of **Sealed Classes**. Use cases for Sealed Classes.
93. Permitted subclasses: `final`, `sealed`, `non-sealed`.
94. Implementation of **Floating-Point Semantics** (Always-Strict).
95. Deprecation of the **Applet API** for removal.
96. Removal of **RMI Activation**.
97. What is the **New macOS Rendering Pipeline** (Metal)?
98. **Pattern Matching for Switch** (Preview).
99. **Foreign Function & Memory API** (Evolution).
100. Context-Specific **Deserialization Filters**.
101. Performance of G1 and ZGC in Java 17 compared to 11.
102. What is the **Pseudo-Random Number Generators** enhancement?
103. Strong Encapsulation by default. How to bypass it if absolutely necessary?
104. How did Java 17 improve the support for Apple Silicon?
105. Discuss the removal of the Security Manager (Deprecation).
106. What is the "Permits" clause in Sealed Interfaces?
107. How Sealed classes improve API design and security.
108. Interaction between Records and Sealed Classes.
109. What is the "Exhaustiveness" check in switch expressions with sealed types?
110. How to migrate from Java 8/11 to 17?

## 6. Java 18 to 21+: The Cutting Edge (20 Questions)
111. What is **UTF-8 by Default** (Java 18)?
112. Simple Web Server (`jwebserver`).
113. **Code Snippets in Java API Documentation**.
114. Virtual Threads (Preview in 19, Standard in 21).
115. What is **Project Loom**?
116. How do **Virtual Threads** solve the scalability issue of platform threads?
117. Performance of 1 million threads in Java 21.
118. What is **Structured Concurrency** (Preview in 21)?
119. What is **Scoped Values** (Preview in 21)?
120. **Pattern Matching for switch** (Standardized in 21).
121. **Record Patterns** (Standardized in 21).
122. **Sequenced Collections** (Java 21).
123. Explain `SequencedCollection`, `SequencedSet`, `SequencedMap`.
124. New methods like `addFirst`, `addLast`, `getFirst`, `getLast`, `reversed`.
125. **String Templates** (Preview in 21).
126. **Unnamed Patterns and Variables** (Preview in 21).
127. **Unnamed Classes and Instance Main Methods** (Preview in 21).
128. **Generational ZGC** (Java 21).
129. **Vector API** (6th Incubator in 21).
130. Improvements in the **KeyStore** implementation.

## 7. Deep Dive & Comparison (20 Questions)
131. Compare **Lambda Expressions** vs **Anonymous Inner Classes**.
132. Compare **Streams** vs **Parallel Streams**. When does overhead exceed gain?
133. Compare **External Iteration** vs **Internal Iteration**.
134. Impact of **Records** on Serialization.
135. **Sealed Classes** vs **Enums** for modeling data.
136. Why were **Text Blocks** preferred over raw strings?
137. Difference between `var` and `Object` type.
138. How **Optional** changes the way you design APIs.
139. How **Modules** affect your project build (Maven/Gradle).
140. **Virtual Threads** vs **Reactive Frameworks** (RxJava/Project Reactor).
141. Why is **Project Loom** better for debugging than Reactive code?
142. Evolution of **Garbage Collection** from 8 to 21.
143. How **Pattern Matching** makes code more readable (Algebraic Data Types).
144. **Sequenced Collections** vs manually calling `iterator().next()`.
145. Security improvements in modern Java (e.g., removal of serialization vulnerabilities).
146. How is the JVM becoming "Cloud Native"?
147. Impact of **Valhalla** (Value Types) on future Java versions.
148. What is **Project Leyden** (Startup time)?
149. What is **Project Panama** (Interoperability)?
150. Summary: Which features were most impactful for SDE3 level development?

---
*Generated by Antigravity for SDE3 Interview Prep.*
