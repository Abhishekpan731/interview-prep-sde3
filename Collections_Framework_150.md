# 150 Collections Framework Interview Questions (SDE3)

## 1. Hierarchy & Basics (20 Questions)
1. Explain the Collections Framework hierarchy in Java.
2. What is the difference between `Collection` and `Collections`?
3. Why doesn't the `Map` interface extend the `Collection` interface?
4. Explain the difference between `List`, `Set`, and `Map`.
5. What is the "Contract" of the `Collection` interface?
6. Compare `Iterator` and `ListIterator`.
7. What is an `Enumeration`? Why is it considered legacy?
8. Explain "Fail-fast" vs "Fail-safe" iterators.
9. How does `ConcurrentModificationException` occur? How to avoid it?
10. What is the difference between `Iterable` and `Iterator`?
11. Describe the use of the `Spliterator` interface (Java 8).
12. How do you convert an Array to a List and vice-versa?
13. What is the significance of the `RandomAccess` interface?
14. Explain the "Diamond Operator" and its evolution in Collections.
15. How does the `default` method in the `Collection` interface help (e.g., `removeIf`, `forEach`)?
16. Compare `Collections.emptyList()` vs `new ArrayList()`.
17. What is the difference between `unmodifiableList` and `immutableList` (Guava/Java 9)?
18. Explain the "Utility methods" provided by the `Collections` class.
19. How to synchronize a collection using `Collections.synchronizedXXX()`?
20. What is the performance overhead of using synchronized wrappers?

## 2. List Interface (20 Questions)
21. Compare `ArrayList` vs `LinkedList`. When would you use one over the other?
22. Internal working of `ArrayList`. How does the resizing (capacity increment) work?
23. What is the default initial capacity of an `ArrayList`?
24. Explain the internal working of `LinkedList`. Is it a singly or doubly linked list?
25. What is the time complexity of `add(index, element)` in `ArrayList` vs `LinkedList`?
26. How does `ArrayList.remove(object)` work internally?
27. What is `Vector`? How does it differ from `ArrayList`?
28. Explain `Stack` class. Why is it recommended to use `Deque` instead of `Stack`?
29. What is the significance of the `modCount` field in `AbstractList`?
30. How to sort a `List` of objects? (Comparable vs Comparator).
31. How does `Collections.sort()` work internally? (TimSort).
32. What is the "SubList" view? What are its limitations?
33. Can we add an element to a `SubList`? Does it affect the original list?
34. How to reverse a `List`?
35. How to find the index of an element in a `List` efficiently?
36. What is the difference between `ArrayList` and `CopyOnWriteArrayList`?
37. When would you use `AttributeList` or `RoleList`?
38. Explain `ArrayList` memory consumption compared to `LinkedList`.
39. How to optimize `ArrayList` for large data sets? (`ensureCapacity`).
40. What happens if you call `trimToSize()` on an `ArrayList`?

## 3. Set Interface (20 Questions)
41. What is the difference between `Set` and `List`?
42. Internal working of `HashSet`. How does it ensure uniqueness?
43. What is the relationship between `HashSet` and `HashMap`?
44. Compare `HashSet`, `LinkedHashSet`, and `TreeSet`.
45. How does `LinkedHashSet` maintain insertion order?
46. Explain `TreeSet` internals. (TreeMap/Red-Black Tree).
47. What happens if you add a `null` element to `HashSet`? And `TreeSet`?
48. How do you sort elements in a `TreeSet`?
49. What is a `NavigableSet`? Explain methods like `lower`, `floor`, `ceiling`, `higher`.
50. What is `EnumSet`? Why is it more efficient than `HashSet`?
51. Internal representation of `EnumSet` (Bit-vectors).
52. Compare `HashSet.contains()` vs `List.contains()`.
53. How to find the intersection and union of two sets?
54. What is the "Contract" for objects used as elements in a `HashSet`?
55. Can we change the state of an object after adding it to a `HashSet`? What are the risks?
56. Explain `CopyOnWriteArraySet`.
57. What is `ConcurrentSkipListSet`?
58. How to convert a `Set` to a `List` keeping the order?
59. What is the default load factor of a `HashSet`?
60. How to implement a custom `Set`?

## 4. Map Interface & HashMap Internals (30 Questions)
61. Explain the `Map` hierarchy.
62. Internal working of `HashMap` in Java 8. (Buckets, Nodes, TreeNodes).
63. Why was the internal implementation of `HashMap` changed to use Red-Black trees for collisions?
64. What is the "Threshold" for converting a linked list to a tree in `HashMap`? (TREEIFY_THRESHOLD).
65. What is the "Threshold" for converting a tree back to a linked list? (UNTREEIFY_THRESHOLD).
66. How does the `hash()` function work in `HashMap`? (XOR and bit shifting).
67. Why must the capacity of `HashMap` always be a power of 2?
68. Explain "Hash Collision" and how `HashMap` handles it.
69. What is the "Load Factor"? How does it affect performance?
70. What happens during "Rehashing"? Is it thread-safe?
71. Difference between `HashMap` and `Hashtable`.
72. Why is `HashMap` preferred over `Hashtable`?
73. Compare `HashMap` vs `LinkedHashMap`.
74. How does `LinkedHashMap` facilitate the implementation of an LRU Cache?
75. Explain `TreeMap` internals. What is its time complexity for searching?
76. What are `WeakHashMap` use cases? How does it interact with Garbage Collection?
77. What is an `IdentityHashMap`? How does it compare objects?
78. What is `EnumMap`? Why is it better than `HashMap` for enum keys?
79. Explain `ConcurrentHashMap` internals. (Segment-based locking in Java 7 vs Node-based in Java 8).
80. How does `ConcurrentHashMap.size()` work?
81. Why doesn't `ConcurrentHashMap` allow `null` keys/values?
82. What is `ConcurrentSkipListMap`?
83. How to sort a `Map` by keys?
84. How to sort a `Map` by values?
85. What is the `Map.Entry` interface?
86. Explain `compute`, `computeIfPresent`, `computeIfAbsent`, and `merge` methods.
87. Difference between `put()` and `putIfAbsent()`.
88. How to iterate over a `Map` efficiently?
89. What is the "Contract" between `equals()` and `hashCode()`?
90. What happens if two different keys have the same hashCode?

## 5. Queue & Deque (20 Questions)
91. What is the `Queue` interface?
92. Compare `offer()`, `poll()`, and `peek()` with `add()`, `remove()`, and `element()`.
93. What is a `Deque` (Double Ended Queue)?
94. Compare `ArrayDeque` vs `LinkedList` as a stack/queue.
95. Why is `ArrayDeque` faster than `LinkedList`?
96. Internal working of `PriorityQueue`.
97. How do you change the sorting order in `PriorityQueue`?
98. Why doesn't `PriorityQueue` allow `null` elements?
99. What is the time complexity of `PriorityQueue` operations?
100. Explain `BlockingQueue` and its implementations.
101. What is the difference between `Bounded` and `Unbounded` queues?
102. Explain `DelayQueue`. Give a use case.
103. What is `SynchronousQueue`?
104. Explain `TransferQueue`.
105. How does `ArrayBlockingQueue` differ from `LinkedBlockingQueue`?
106. What is the use of `PriorityBlockingQueue`?
107. Explain `Deque` as a LIFO structure.
108. How to implement a circular buffer using collections?
109. What is `ConcurrentLinkedQueue`?
110. Explain the "Work Stealing" queue (`Deque`).

## 6. Java 8+ Collections Enhancements (20 Questions)
111. How to use `Collection.removeIf()`?
112. Explain `Map.forEach()` and `Map.replaceAll()`.
113. What is `List.of()`, `Set.of()`, `Map.of()` (Java 9)?
114. Are collections created by `List.of()` mutable?
115. How do these "Factory Methods" handle `null`?
116. Difference between `Collections.unmodifiableList()` and `List.of()`.
117. Explain `Collector` interface and its usages in `Stream.collect()`.
118. Common `Collectors` methods (`toList`, `toSet`, `toMap`, `groupingBy`, `partitioningBy`).
119. Explain `Collectors.joining()`.
120. How to use `Collectors.collectingAndThen()`?
121. What is `Stream.toList()` (Java 16)?
122. Explain `SequencedCollection`, `SequencedSet`, and `SequencedMap` (Java 21).
123. How does `SequencedCollection` provide access to first and last elements?
124. What are the methods added in Java 21 for reversed views? (`reversed()`).
125. How to convert a `Stream` to a `Map` where keys might collide?
126. Explain `Collectors.summarizingInt()`.
127. What is `Collectors.mapping()`?
128. Explain `Stream.distinct()` and how it uses `equals()` and `hashCode()`.
129. How to parallelize collection processing safely?
130. Effect of "Short-circuiting" operations on collections.

## 7. Advanced Scenarios & Performance (20 Questions)
131. How to choose the right collection for a given problem?
132. Performance comparison of all `List` implementations.
133. Performance comparison of all `Map` implementations.
134. Memory overhead of a `HashMap` node.
135. How to handle "Large Collections" (millions of objects)?
136. What is "Object Pooling" vs Collections?
137. How does the JVM optimize collection access (Scalar replacement)?
138. Discuss "Primitive Collections" (e.g., Trove, fastutil, Eclipse Collections). Why are they needed?
139. Explain the "Cache-friendliness" of `ArrayList` vs `LinkedList`.
140. How to implement a thread-safe LRU Cache using `LinkedHashMap`?
141. Discuss "Persistent Data Structures" in the context of Java.
142. How to detect a memory leak in a collection? (Analysis of stale references).
143. Impact of `final` keyword on collection references.
144. How to use `Collections.checkedCollection()`?
145. What is `Collections.singletonList()`?
146. How to efficiently find duplicates in a large list?
147. How to compare two collections for equality (independent of order)?
148. Impact of Serialization on Collections.
149. How to create a custom "Immutable" collection?
150. Future of Collections: Project Valhalla and specialized generic collections for primitives.

---
*Generated by Antigravity for SDE3 Interview Prep.*
