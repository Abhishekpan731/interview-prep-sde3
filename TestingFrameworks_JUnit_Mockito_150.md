# 150 Testing Frameworks (JUnit & Mockito) Interview Questions (SDE3)

## 1. JUnit Fundamentals & Logic (40 Questions)
1. What is **JUnit** and why is it the industry standard for Java unit testing?
2. Explain the difference between **JUnit 4** and **JUnit 5 (Jupiter)**. Why was the architecture split into Platform, Jupiter, and Vintage?
3. Describe the **JUnit 5 Lifecycle Annotations**: `@BeforeAll`, `@BeforeEach`, `@Test`, `@AfterEach`, and `@AfterAll`.
4. What is the execution order of these annotations?
5. How does JUnit 5 handle **Test Instance Lifecycle** (`@TestInstance`)? What is the difference between `PER_METHOD` and `PER_CLASS`?
6. Explain **Assertions** in JUnit 5. How do they differ from JUnit 4 (`Assertions` vs `Assert`)?
7. What is **AssertAll (Grouped Assertions)**? Why is it useful?
8. Explain **Assumptions** (`Assumptions.assumeTrue`). How do they differ from Assertions?
9. How to test for **Exceptions** in JUnit 5 using `assertThrows`?
10. What is a **Parameterized Test** (`@ParameterizedTest`)?
11. List the common **Source Annotations** for parameterized tests: `@ValueSource`, `@CsvSource`, `@MethodSource`, `@EnumSource`.
12. How to use a **Custom ArgumentConverter** in parameterized tests?
13. Explain **Repeated Tests** (`@RepeatedTest`).
14. What are **Dynamic Tests** (`@TestFactory`)? When should you use them over standard tests?
15. Explain **Nested Tests** (`@Nested`). How do they improve test organization?
16. What is the purpose of `@Tag` and `@DisplayName`?
17. How to **Disable/Ignore** tests (`@Disabled`)?
18. Explain **Timeout Testing** (`@Timeout` vs `assertTimeout` vs `assertTimeoutPreemptively`).
19. What are **JUnit 5 Extensions**? How do they replace JUnit 4 `@Rule` and `@ClassRule`?
20. Explain the **InvocationInterceptor** extension point.
21. How to implement a custom **TestWatcher** in JUnit 5?
22. Explain **Conditional Test Execution** (`@EnabledOnOs`, `@EnabledIfSystemProperty`, `@EnabledIf`).
23. What is the **JUnit Platform**? How does it allow running non-JUnit tests?
24. How to run JUnit tests from the **Command Line** or **Maven/Gradle**?
25. Explain **Parallel Test Execution** in JUnit 5. How to configure it?
26. What is the difference between `assertEquals` and `assertSame`?
27. How to test **Private Methods**? (Discussion on design vs. reflection utilities).
28. What is the **Test Order** (`@TestMethodOrder`)? When should you enforce it?
29. How to use **Templates** in JUnit 5?
30. Explain **Constructor-based Dependency Injection** in JUnit 5 test classes.
31. What is the role of `junit-platform.properties`?
32. How to use **Meta-annotations** to create custom test annotations?
33. Explain **Discovery Selectors** and **Filters** in the JUnit Platform.
34. What is **Test Hierarchy**?
35. How to implement **Slow/Fast Test Suites** using Tags?
36. What is the difference between **Unit Tests** and **Integration Tests** in the context of JUnit?
37. How to test **Abstract Classes**?
38. Explain **Exception swallowing** in tests.
39. What is **Test Coverage** and how to measure it (JaCoCo)?
40. Why should you avoid **System.out.println** in JUnit tests?

## 2. Mockito: Mocking and Stubbing (50 Questions)
41. What is **Mocking**? Why is it essential for true "Unit" tests?
42. Explain the difference between a **Mock**, **Spy**, **Stub**, and **Fake**.
43. How to create a mock using **`Mockito.mock()`** vs. **`@Mock`** annotation?
44. What is the purpose of **`MockitoAnnotations.openMocks(this)`** or **`@ExtendWith(MockitoExtension.class)`**?
45. Explain **Stubbing** using `when(...).thenReturn(...)`.
46. How to stub **Consecutive Calls** (e.g., return A first, then B)?
47. What is **`doReturn(...).when(...)`**? When is it mandatory (especially for Spies)?
48. Explain **Argument Matchers** (`any()`, `anyString()`, `eq()`).
49. What is the **Rule of Matchers**: if you use one matcher for a method, you must use matchers for all arguments?
50. How to create a **Custom Argument Matcher**?
51. Explain **Verification** (`verify(...)`).
52. How to verify that a method was called a **Specific Number of Times** (`times()`, `atLeast()`, `never()`)?
53. What is **`verifyNoInteractions(mock)`** and **`verifyNoMoreInteractions(mock)`**?
54. Explain **InOrder Verification**.
55. How to verify **Argument Values** using an **`ArgumentCaptor`**?
56. What are **Spies (`@Spy`)**? How do they differ from mocks?
57. Explain **`doNothing()`** for stubbing void methods.
58. How to stub **Exceptions** (`thenThrow()`, `doThrow()`)?
59. What is the **`Answer`** interface? When should you use it to provide dynamic responses?
60. Explain **BDDMockito** (`given`, `willReturn`, `then`, `should`). Why is it preferred in some teams?
61. What is **Mocking of Static Methods** (Mockito 3.4+)?
62. How to mock **Final Classes and Methods**?
63. Explain **`Mockito.reset(mock)`**. Why is it considered a "code smell"?
64. What is **Strictness** in Mockito (`@MockitoSettings(strictness = Strictness.STRICT_STUBS)`)?
65. How to mock **Constructor Calls** (`mockConstruction`)?
66. Explain the impact of **Deep Stubs** (`RETURNS_DEEP_STUBS`). Why is it often a violation of the Law of Demeter?
67. How to mock **Generics** with Mockito?
68. What is the difference between `verify(mock).method()` and `verify(mock, times(1)).method()`?
69. How to handle **Asynchronous Methods** in Mockito?
70. What are **Mocking Limitations** in Mockito?
71. Explain how Mockito uses **ByteBuddy** under the hood.
72. How to test a **Method that calls another method in the same class**? (Mocking vs. Spying).
73. What is **Capturing standard output** in Mockito?
74. How to share **Mocks across multiple test classes**?
75. Explain **`Mockito.validateMockitoUsage()`**.
76. What is the **`MockSettings`** interface?
77. How to mock a **Functional Interface** (Lambda)?
78. What is the difference between `spy(MyClass.class)` and `spy(new MyClass())`?
79. How to verify a **Void Method with side effects**?
80. Explain **`additionalAnswers`** and **`delegatesTo`**.
81. How to mock **Abstract Classes**?
82. What happens if you call a real method on a Mock?
83. What happens if you don't stub a method on a Mock? (Default return values).
84. How to change the **Default Return Value** of a mock?
85. Explain **Partial Mocking**.
86. How to verify **Interactions with exact arguments**?
87. What is **`ArgumentMatcher` vs. `ArgumentCaptor`**?
88. How to mock **Primitive return types**?
89. How to handle **`NullPointerException`** in Mockito stubbing?
90. Describe a scenario where Mocking actually makes the tests **Less Reliable**.

## 3. Advanced Testing & Best Practices (30 Questions)
91. What is **TDD (Test Driven Development)**? Walk through the Red-Green-Refactor cycle.
92. What is **BDD (Behavior Driven Development)**?
93. Explain the **Testing Pyramid**. Where do JUnit and Mockito fit in?
94. How to implementation **Mutation Testing** (PITest) in Java?
95. What is the difference between **Unit Tests, Integration Tests, and Contract Tests**?
96. How to test **Concurrency and Multi-threading** code?
97. What is **AOP (Aspect-Oriented Programming)** impact on unit testing?
98. How to use **Testcontainers** for database or Kafka integration tests?
99. Explain **Consumer-Driven Contract Testing** (Pact).
100. How to handle **Testing of Legacy Code** with no existing tests?
101. What is **Flaky Test**? How to identify and fix them?
102. How do you decide **What to Mock** and **What not to Mock**?
103. Explain the **"Single Assertion per Test"** rule and its trade-offs.
104. What is **FIRST** principle of Unit Testing (Fast, Independent, Repeatable, Self-validating, Timely)?
105. How to test **Spring Boot Applications**? (JUnit 5 + `@SpringBootTest` + `@MockBean`).
106. Difference between **`@Mock`** (Mockito) and **`@MockBean`** (Spring Boot).
107. Difference between **`@Spy`** (Mockito) and **`@SpyBean`** (Spring Boot).
108. How to use **`@JsonTest`** and **`@DataJpaTest`** for slice testing?
109. Explain **MockMvc** for testing REST Controllers.
110. How to use **WireMock** for stubbing external REST APIs?
111. What is **Property-based Testing** (jqwik)?
112. How to perform **Visual Regression Testing** for UIs?
113. What is the **"Arrange-Act-Assert" (AAA)** pattern?
114. How to balance **Test Speed** vs. **Test Thoroughness**?
115. Explain **Code Coverage** vs. **Mutation Coverage**.
116. How to handle **Testing of Randomness** (e.g., `Math.random`)?
117. What is **Snapshot Testing**?
118. How to test **File I/O** without creating actual files?
119. Explain **Test Data Builders** vs. **Object Mother** patterns.
120. How to avoid **Slow Tests**?

## 4. Integration & Real-world Scenarios (30 Questions)
121. How to test a **Method that sends a message to Kafka**?
122. How to test a **Transactional Service** in Spring?
123. How to verify that a **Transaction was rolled back** on exception?
124. How to test **Hibernate/JPA entity mapping**?
125. How to mock a **Database Connection**? (Spoiler: generally don't, use H2 or Testcontainers).
126. How to test **Security Filters** and **Authorization** rules?
127. How to test **Asynchronous execution** using `@Async`?
128. How to mock **External Services (e.g., S3, SendGrid)**?
129. How to test **Scheduled Tasks** (`@Scheduled`)?
130. How to mock a **WebClient** or **RestTemplate**?
131. How to test **Custom Annotations** logic?
132. How to test **Error Handling** mapping in REST APIs?
133. How to verify **Log Messages** in unit tests?
134. How to test **Reflection-based** utilities?
135. How to handle **Testing of Time-dependent logic**? (Java 8 `Clock`).
136. How to test **Memory leaks** in unit tests?
137. How to test **Large Data Streams**?
138. How to mock **OAuth2 Authentication** in integration tests?
139. How to test **API Versioning**?
140. How to handle **Testing of Multi-tenant** applications?
141. How to maintain **Test Data Independence**?
142. How to perform **Load Testing** using JUnit?
143. How to document tests using **Allure** or other reporting tools?
144. How to integrate **Unit Tests into a CI/CD Pipeline**?
145. What is the impact of **Java Modules (Project Jigsaw)** on visibility in tests?
146. How to debug a **Failing Test in a remote CI environment**?
147. How to handle **Big JSON/XML payloads** in tests?
148. How to perform **Stubbing of Static Factories**?
149. How to handle **Cyclic Dependencies** during mocking?
150. Summary: What is the most complex unit test you've ever written and why was it difficult?

---
*Generated by Antigravity for SDE3 Interview Prep.*
