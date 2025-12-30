# 150 Core Java Fundamentals Interview Questions (SDE3)

## 1. OOPs Concepts & Internals (25 Questions)
1. Explain the four pillars of OOPs (Abstraction, Encapsulation, Inheritance, Polymorphism) in a real-world system design context.
2. How does Java achieve "Multiple Inheritance" using Interfaces? Why was it restricted for classes?
3. Difference between "Composition" and "Inheritance." When is composition preferred?
4. What is "Dynamic Binding" vs "Static Binding"?
5. How does the JVM handle method overriding internally? (Virtual Method Table / vtable).
6. Explain "Method Hiding" with static methods.
7. What is "Is-A" vs "Has-A" relationship?
8. Can we override a `private` or `static` method? Why?
9. Explain the "Object" class and its mandatory methods (`equals`, `hashCode`, `toString`, `clone`, `finalize`).
10. What is the difference between "Abstraction" and "Data Hiding"?
11. How does the `super` keyword work? Can we use it in a static context?
12. Explain "Constructor Chaining."
13. What is the difference between an "Abstract Class" and an "Interface" in Java 8, Java 9+, and Java 21?
14. Can an interface have a constructor? Why or why not?
15. What are "Nested Classes" (Static, Inner, Local, Anonymous)?
16. Explain the "Shadowing" of variables.
17. What is "Tight Coupling" vs "Loose Coupling"?
18. How to achieve "Immutable" classes in Java?
19. What is a "Marker Interface"? Give examples (`Serializable`, `Cloneable`).
20. Explain "Covariant Return Types."
21. What is the impact of the `final` keyword on classes, methods, and variables?
22. Deep dive into "Polymorphism" at runtime and compile-time.
23. What is the "Liskov Substitution Principle" in the context of Java inheritance?
24. How to implement the "Delegation" pattern in Java?
25. Explain the "Initialization Order" of fields, blocks, and constructors.

## 2. Java Language Rules & Keywords (20 Questions)
26. Explain the `volatile` keyword. When is it mandatory?
27. What is the purpose of the `transient` keyword?
28. Explain the `strictfp` keyword. Is it still relevant in modern Java?
29. What is the difference between `==` and `.equals()`?
30. Explain the "String Constant Pool."
31. Why is `String` immutable in Java? Discuss security and performance.
32. Compare `String` vs `StringBuilder` vs `StringBuffer`.
33. What is "String Deduplication" in G1 GC?
34. Explain the `instanceof` operator. How has it changed in Java 16+?
35. What is the `native` keyword?
36. Explain the `synchronized` keyword. How does it affect thread visibility?
37. What is the difference between `throw` and `throws`?
38. Explain "Variable Arguments" (varargs). What are the limitations?
39. What is the difference between `Integer a = 100` and `Integer b = new Integer(100)`? (Integer Caching).
40. Explain "Autoboxing" and "Unboxing" performance pitfalls.
41. What is the `yield` keyword (Java 13+)?
42. Explain the `sealed` keyword (Java 17+).
43. What is the `non-sealed` keyword?
44. Explain the `record` keyword (Java 14+).
45. What are "Local Variable Type Inference" (`var`) limitations?

## 3. Exception Handling (20 Questions)
46. Explain the Exception Hierarchy in Java (`Throwable`, `Error`, `Exception`).
47. Checked vs Unchecked Exceptions. Why is there a debate about checked exceptions?
48. What is the "Exception Propagation" mechanism?
49. Explain the `finally` block. When does it NOT execute?
50. What happens if both the `try` block and the `finally` block have return statements?
51. Explain "Try-with-resources" (Java 7). How does it handle multiple resources?
52. What are "Suppressed Exceptions"?
53. How to create a "Custom Exception"?
54. Explain "Exception Chaining."
55. What is the cost of throwing an exception? (Stack trace generation).
56. Difference between `NoClassDefFoundError` and `ClassNotFoundException`.
57. Explain `ExceptionInInitializerError`.
58. What is a "StackOverflowError"? How to debug it?
59. How to handle exceptions in a "Multi-threaded" environment?
60. What is the `UncaughtExceptionHandler`?
61. Explain "Multi-catch" block (Java 7).
62. How does the JVM handle exceptions at the bytecode level? (Exception Table).
63. Can we catch an `Error`? Is it a good practice?
64. What is the difference between `fail-fast` and `fail-safe` in error handling?
65. Best practices for logging exceptions.

## 4. Generics (20 Questions)
66. What are "Generics"? Why were they introduced?
67. Explain "Type Erasure." What are its implications?
68. What is the difference between `List<Object>` and `List<?>`?
69. Explain "Bounded Wildcards" (`Upper Bounded` vs `Lower Bounded`).
70. What is the **PECS** rule (Producer Extends, Consumer Super)?
71. Can we have generic static methods?
72. Why can't we use primitives as type parameters in Generics?
73. How to get the generic type of a class at runtime? (Type tokens).
74. Explain generic "Wildcards" vs "Type Parameters."
75. What is a "Raw Type"? Why should we avoid it?
76. Can we create an array of generic types? Why?
77. Explain "Intersection Types" in Generics.
78. What is the "Diamond Operator"?
79. How does "Bridge Methods" work in Generics?
80. What is "Type Inference"?
81. Can we instantiate a generic type? (`new T()`).
82. Difference between `List<T>` and `List<?>`.
83. How do Generics interact with Legacy Code?
84. Can we overload methods using Generics?
85. What is "Recursive Type Bound"? (e.g., `Enum<E extends Enum<E>>`).

## 5. Persistence, Serialization & IO (20 Questions)
86. How does **Java Serialization** work?
87. What is `serialVersionUID`? What happens if it's missing?
88. Explain the `Externalizable` interface.
89. How to prevent serialization of specific fields?
90. What are the security vulnerabilities in Java Deserialization?
91. Explain `writeObject` and `readObject` methods.
92. How does the `transient` keyword behave during serialization?
93. What is "Deep Copy" using serialization?
94. Explain the difference between `File`, `Path`, and `Files` in Java.
95. What is the difference between `InputStream`/`OutputStream` and `Reader`/`Writer`?
96. Explain "Buffered Streams" and why they are faster.
97. What is "Character Encoding"? How does Java handle it?
98. Explain **NIO** (New IO). Channels vs Buffers vs Selectors.
99. What is a "MappedByteBuffer"?
100. Explain "Zero-copy" file transfer.
101. What is "WatchService" in NIO.2?
102. How to read a very large file (>10GB) in Java?
103. Explain "Scanner" vs "BufferedReader."
104. What is the "System.console()" object?
105. How to handle binary data in Java?

## 6. Reflection, Annotations & SPI (20 Questions)
106. What is the **Reflection API**?
107. Performance impact of Reflection.
108. How to access a `private` field using Reflection?
109. What is the `AccessibleObject.setAccessible(true)` method?
110. Explain **Annotations**. How to create a custom annotation?
111. What are "Retention Policies" (`Source`, `Class`, `Runtime`)?
112. What are "Annotation Targets"?
113. Explain **Annotation Processing** at compile-time (JSR 269).
114. How does Spring use Reflection/Annotations?
115. What is the **Service Provider Interface (SPI)**?
116. How does `java.util.ServiceLoader` work?
117. What is the difference between `Introspection` and `Reflection`?
118. Explain **Dynamic Proxies**.
119. What is for "Method Handles" (`java.lang.invoke`)?
120. How to prevent Reflection-based attacks on Singleton?
121. Explain "Type Erasure" reflection challenges.
122. What is "Class.forName()" vs "MyClass.class"?
123. Can we add/remove fields at runtime using reflection?
124. How to detect if a class is a Record or a Sealed class using reflection?
125. Impact of Java Modules on Reflection.

## 7. Java Ecosystem & Advanced Fundamentals (25 Questions)
126. Explain "Pass by Value" vs "Pass by Reference" in Java.
127. Why does Java not have "Pointers"?
128. What is the "Native Interface" (JNI)?
129. How to call a C/C++ function from Java?
130. Explain "Reference Collections" (Strong, Soft, Weak, Phantom).
131. When to use a `WeakReference`?
132. What is the purpose of `ReferenceQueue`?
133. Explain the "finalize()" method and why it's deprecated.
134. What are **Cleaner** and **Provider** (Java 9 replacement for `finalize`)?
135. What is the "Main" method signature? Can it be `private` or `static`? (Evolution in Java 21).
136. Explain the "CLASSPATH" vs "MODULEPATH."
137. Difference between `jar`, `war`, and `ear`.
138. What is a "Fat JAR" (Uber JAR)?
139. How does Java handle "Unicode" and "Emoji"?
140. Explain "Property" files and "ResourceBundles."
141. What is the "Currency" class in Java?
142. How to format numbers and dates based on "Locale"?
143. Explain "Runtime.getRuntime().exec()".
144. What is "ProcessBuilder"?
145. How does the JVM handle "Shifting" operators (`<<`, `>>`, `>>>`)?
146. Explain the "Bitwise" operations in Java.
147. What is "Endianness" and how does Java handle it?
148. Difference between `System.out`, `System.err`, and `System.in`.
149. How to profile memory using `Runtime` class?
150. Future of Java: Discussion on "Project Valhalla" and "Project Panama."

---
*Generated by Antigravity for SDE3 Interview Prep.*
