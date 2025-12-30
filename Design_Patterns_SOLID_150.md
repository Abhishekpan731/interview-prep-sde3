# 150 Design Patterns & SOLID Interview Questions (SDE3)

## 1. SOLID Principles (25 Questions)
1. Explain the **Single Responsibility Principle (SRP)**. Give a Java example.
2. How to detect an SRP violation in a "Manager" class?
3. What is the **Open/Closed Principle (OCP)**? How do interfaces support this?
4. Explain the **Liskov Substitution Principle (LSP)**.
5. Why is the "Square-Rectangle" problem the classic example of LSP violation?
6. How does LSP relate to inheritance and method overriding?
7. Explain the **Interface Segregation Principle (ISP)**.
8. Compare "Fat Interfaces" vs "Lean Interfaces."
9. What is the **Dependency Inversion Principle (DIP)**?
10. Difference between DIP, Dependency Injection (DI), and Inversion of Control (IoC).
11. How does Spring Framework implement DIP?
12. Why should high-level modules not depend on low-level modules?
13. How do SOLID principles improve maintainability?
14. Can you have OCP without using interfaces/abstract classes?
15. Relationship between SRP and "Cohesion."
16. Relationship between DIP and "Coupling."
17. What is "Polymorphism" and how does it relate to SOLID?
18. How to refactor a legacy code that violates all SOLID principles?
19. Discuss the trade-off between SOLID principles and code complexity/verbosity.
20. How does "Composition over Inheritance" help in adhering to SOLID?
21. What is the "Law of Demeter" (Principle of Least Knowledge)?
22. Explain the **DRY** (Don't Repeat Yourself) principle.
23. Explain **KISS** (Keep It Simple, Stupid).
24. What is **YAGNI** (You Ain't Gonna Need It)?
25. How do SOLID principles apply to Functional Programming in Java?

## 2. Creational Design Patterns (20 Questions)
26. What are Creational Patterns?
27. Explain the **Singleton Pattern**.
28. How to implement a **Thread-Safe Singleton**? (Eager vs Lazy vs Bill Pugh).
29. How can Reflection and Serialization break a Singleton? How to prevent it?
30. Why is `enum` the best way to implement a Singleton in Java?
31. What is the **Factory Method Pattern**? Give an example using a Logger or Database connection.
32. Explain the **Abstract Factory Pattern**. How does it differ from a Factory Method?
33. When to use **Builder Pattern**? (Effective Java recommendation).
34. How does the Builder Pattern handle optional parameters?
35. What is the **Prototype Pattern**? Explain `Cloneable` and its pitfalls.
36. Deep Copy vs Shallow Copy in the Prototype pattern.
37. What is the **Object Pool Pattern**? (Example: ThreadPool, Connection Pool).
38. When should you use a Static Factory Method instead of a Constructor?
39. How does the "Dependency Injection" pattern act as a creational pattern?
40. Compare Singleton vs Static Class.
41. Can a Singleton have more than one instance? (e.g., per thread, per classloader).
42. How to implement a "Multiton" pattern?
43. What is a "Service Locator" pattern? Is it an anti-pattern?
44. Explain the **Lazy Initialization** pattern.
45. How does the "Record" feature in Java influence creational patterns?

## 3. Structural Design Patterns (25 Questions)
46. What are Structural Patterns?
47. Explain the **Adapter Pattern**. (Class Adapter vs Object Adapter).
48. Real-world example of Adapter Pattern in Java (e.g., `Arrays.asList()`).
49. What is the **Bridge Pattern**? How does it decouple abstraction from implementation?
50. Explain the **Composite Pattern**. (Uniformity vs Type Safety).
51. Use case for Composite Pattern: File system or Organization hierarchy.
52. What is the **Decorator Pattern**?
53. How does the Decorator Pattern follow the OCP?
54. Java I/O: Give examples of Decorator pattern (`BufferedReader`, `InputStreamReader`).
55. Compare Decorator vs Inheritance.
56. What is the **Facade Pattern**? How does it promote loose coupling?
57. Explain the **Flyweight Pattern**. How does it save memory?
58. Java's `String Pool` and `Integer.valueOf()`: How do they use the Flyweight pattern?
59. What is the **Proxy Pattern**?
60. Types of Proxies: Virtual Proxy, Remote Proxy, Protection Proxy, Smart Reference.
61. How does **Dynamic Proxy** work in Java?
62. Compare Proxy vs Decorator vs Adapter.
63. What is the **Private Class Data** pattern?
64. Explain the **Exposed Object** pattern.
65. How does the "Module System" in Java change the way we use structural patterns?
66. Use case for the **Twin Pattern**.
67. What is the **Marker Interface** pattern? (e.g., `Serializable`, `Remote`).
68. Explain the **Front Controller** pattern.
69. What is the **Data Transfer Object (DTO)** pattern?
70. Difference between DTO and DAO.

## 4. Behavioral Design Patterns (30 Questions)
71. What are Behavioral Patterns?
72. Explain the **Chain of Responsibility** pattern.
73. Real-world example: Filter chain in Web Servlets or Logging levels.
74. What is the **Command Pattern**?
75. Implementation of "Undo/Redo" using Command Pattern.
76. Explain the **Interpreter Pattern**.
77. What is the **Iterator Pattern**? How does Java's `Iterator` implement this?
78. What is the **Mediator Pattern**? How does it reduce dependencies?
79. Explain the **Memento Pattern**. How to implement "Checkpoints"?
80. What is the **Observer Pattern**?
81. Compare Observer Pattern vs Publish-Subscribe model.
82. Java's `PropertyChangeListener` and `Flow API`: How do they relate to Observer?
83. Explain the **State Pattern**.
84. Use case: TCP Connection states or Vending Machine.
85. Compare State Pattern vs Strategy Pattern.
86. What is the **Strategy Pattern**?
87. How do Java 8 Lambdas simplify the Strategy Pattern?
88. Explain the **Template Method Pattern**.
89. How to use a "Hook" in Template Method?
90. Compare Template Method vs Strategy.
91. What is the **Visitor Pattern**?
92. Explain "Double Dispatch" in the context of Visitor Pattern.
93. What is the **Null Object Pattern**? How does `Optional` serve as a modern replacement?
94. Explain the **Interpreter Pattern**.
95. Use case for **State Machine** in Java.
96. Compare **Action** command vs **Navigation** command.
97. What is the **Specification Pattern**?
98. Explain the **Balking Pattern**.
99. What is the **Double Buffer** pattern?
100. Explain the **Guarded Suspension** pattern.

## 5. Architectural & Microservices Patterns (20 Questions)
101. What is the **Layered Architecture** (Presentation, Logic, Data)?
102. Explain **Microservices Architecture**.
103. What is the **SAGA Pattern** for distributed transactions? (Choreography vs Orchestration).
104. Explain **CQRS** (Command Query Responsibility Segregation).
105. What is **Event Sourcing**?
106. Explain the **API Gateway** pattern.
107. What is the **Circuit Breaker** pattern? (Resilience4j).
108. Explain the **Bulkhead Pattern**.
109. What is the **Sidecar Pattern**?
110. Explain **Service Discovery** (Eureka, Consul).
111. What is **Externalized Configuration** (Config Server)?
112. Explain **Distributed Tracing** (Sleuth, Zipkin).
113. What is the **Strangler Fig** pattern for legacy migration?
114. Explain **Database Per Service** vs **Shared Database**.
115. What is **BFF** (Backend For Frontend) pattern?
116. Explain **Blue-Green Deployment** vs **Canary Release**.
117. What is **Shadow Deployment**?
118. Explain **12-Factor App** methodology.
119. What is **Clean Architecture** (Uncle Bob)?
120. Compare **Monolith** vs **Serverless**.

## 6. Anti-Patterns & Code Smells (15 Questions)
121. What is an **Anti-Pattern**?
122. Explain the **God Object** anti-pattern.
123. What is **Spaghetti Code**?
124. What is **Lasagna Code** (Too many layers)?
125. Explain the **Golden Hammer** anti-pattern.
126. What is **Premature Optimization**?
127. What is **Cargo Cult Programming**?
128. Explain **Magic Strings/Numbers**.
129. What is **Copy-Paste Programming**?
130. Explain **Soft Coding** (Over-configuration).
131. What is the **Inner Platform Effect**?
132. Explain **Blind Faith** (No error checking).
133. What is a **Dead Lock** in code design?
134. Explain the **Singleton Abuse** anti-pattern.
135. What is **Reinventing the Wheel**?

## 7. Practical Exercises & Thinking (15 Questions)
136. Design a "Notification System" (Email, SMS, Push) using Design Patterns.
137. Design an "Order Processing System" with different payment gateways.
138. How to implement a "Rate Limiter" using the Token Bucket algorithm?
139. Design a "File Compressor" that supports Zip, Gzip, and 7z.
140. How would you design a "Log Framework" from scratch?
141. Design a "Dynamic Pricing Engine" for an e-commerce platform.
142. How to implement "Multitenancy" at the design level?
143. Design an "Excel Parser" that handles .xls and .xlsx using Bridge.
144. How would you design a "Cashing Layer" for a database?
145. Design a "Message Broker" (simplified version) using Observer.
146. How to refactor a switch-case block containing business logic into the Strategy pattern?
147. Design a "WorkFlow Engine".
148. How would you model a "Stock Market" price update system?
149. Design a "URL Shortener" high-level architecture.
150. Summary: How do patterns evolve with language features (like Records and Sealed Classes)?

---
*Generated by Antigravity for SDE3 Interview Prep.*
