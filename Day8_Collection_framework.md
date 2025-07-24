# Day 8: The Collections Framework

## 🗺️ Learning Roadmap

```
Day 1-6 (Core OOP) → Day 7 (Exception Handling & Arrays) → Day 8 (Collections Framework)
                                                                 ↓
                                                            You are here
```

## 🎯 Learning Objectives

By the end of this session, you will be able to:

- Understand the Java Collections Framework hierarchy.
- Use `Map` implementations (`HashMap`, `LinkedHashMap`, `TreeMap`) for key-value storage.
- Use `Set` implementations (`HashSet`, `LinkedHashSet`, `TreeSet`) for unique element storage.
- Understand legacy collections like `Vector` and `Hashtable`.
- Explain the basics of Garbage Collection.
- Answer common interview questions related to collections.

## ⏱️ Time Allocation

- Maps & Legacy Collections (45 minutes)
- Sets & Other Concepts (45 minutes)

---

## Session 1: Maps & Legacy Collections (45 minutes)

### 1. Introduction to the Collections Framework

The Java Collections Framework provides a set of interfaces and classes to represent and manage groups of objects.

**Core Interfaces:**

- `Collection`: The root of the hierarchy.
  - `List`: An ordered collection (sequence). Allows duplicates.
  - `Set`: A collection that contains no duplicate elements.
  - `Queue`: A collection used to hold elements prior to processing.
- `Map`: An object that maps keys to values. Not a true "collection" as it doesn't extend `Collection`.

### 2. The `Map` Interface

A `Map` stores data in key-value pairs. Each key must be unique.

#### `HashMap`

`HashMap` is the most commonly used `Map` implementation.

- **Ordering**: Does not guarantee any specific order of elements.
- **Performance**: Provides constant-time performance (O(1)) for `get` and `put` operations.
- **Nulls**: Allows one `null` key and multiple `null` values.

```java
import java.util.HashMap;
import java.util.Map;

Map<String, Integer> studentScores = new HashMap<>();

// Add key-value pairs
studentScores.put("Alice", 95);
studentScores.put("Bob", 88);
studentScores.put("Charlie", 92);

// Get a value by key
int aliceScore = studentScores.get("Alice"); // 95

// Iterate over a map
for (Map.Entry<String, Integer> entry : studentScores.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}
```

#### `LinkedHashMap`

`LinkedHashMap` is a subclass of `HashMap` that maintains the insertion order of elements.

- **Ordering**: Iterates in the order in which keys were inserted.
- **Performance**: Slightly slower than `HashMap` due to the overhead of maintaining the linked list.

```java
import java.util.LinkedHashMap;
import java.util.Map;

Map<String, String> config = new LinkedHashMap<>();
config.put("db.url", "localhost");
config.put("db.user", "admin");
config.put("db.password", "pass");

// The output will be in the order of insertion
System.out.println(config); // {db.url=localhost, db.user=admin, db.password=pass}
```

#### `TreeMap`

`TreeMap` stores key-value pairs in a sorted order based on the natural ordering of the keys or a custom `Comparator`.

- **Ordering**: Sorted by key.
- **Performance**: Slower than `HashMap` for insertions and lookups (O(log n)).
- **Nulls**: Does not allow a `null` key.

```java
import java.util.TreeMap;
import java.util.Map;

Map<Integer, String> errorCodes = new TreeMap<>();
errorCodes.put(500, "Internal Server Error");
errorCodes.put(404, "Not Found");
errorCodes.put(200, "OK");

// The output will be sorted by key (200, 404, 500)
System.out.println(errorCodes); // {200=OK, 404=Not Found, 500=Internal Server Error}
```

### 3. Legacy Collections: `Vector` & `Hashtable`

These are older, synchronized collections from Java 1.0. They are generally not recommended for new code.

#### `Hashtable`

- **Analogy**: A synchronized version of `HashMap`.
- **Synchronization**: All methods are synchronized, making it thread-safe but slower in single-threaded environments.
- **Nulls**: Does not allow `null` keys or `null` values.

**Modern Alternative**: Use `HashMap` for single-threaded applications or `java.util.concurrent.ConcurrentHashMap` for multi-threaded applications.

#### `Vector`

- **Analogy**: A synchronized version of `ArrayList`.
- **Synchronization**: All methods are synchronized.

**Modern Alternative**: Use `ArrayList` and manage synchronization externally if needed, or use `CopyOnWriteArrayList`.

---

## Session 2: Sets & Other Concepts (45 minutes)

### 1. The `Set` Interface

A `Set` is a collection that contains no duplicate elements. It models the mathematical set abstraction.

#### `HashSet`

`HashSet` is the most commonly used `Set` implementation.

- **Ordering**: Does not guarantee any order.
- **Performance**: Offers constant-time performance (O(1)) for `add`, `remove`, and `contains`.
- **Internal Structure**: Backed by a `HashMap`.

```java
import java.util.HashSet;
import java.util.Set;

Set<String> uniqueNames = new HashSet<>();
uniqueNames.add("Alice");
uniqueNames.add("Bob");
uniqueNames.add("Alice"); // This will be ignored

System.out.println(uniqueNames); // [Bob, Alice] (order not guaranteed)
System.out.println("Contains Bob? " + uniqueNames.contains("Bob")); // true
```

#### `LinkedHashSet`

`LinkedHashSet` maintains the insertion order of elements.

- **Ordering**: Iterates in the order elements were inserted.
- **Performance**: Slightly slower than `HashSet`.

```java
import java.util.LinkedHashSet;
import java.util.Set;

Set<Integer> numbers = new LinkedHashSet<>();
numbers.add(30);
numbers.add(10);
numbers.add(20);

System.out.println(numbers); // [30, 10, 20] (insertion order)
```

#### `TreeSet`

`TreeSet` stores elements in a sorted order.

- **Ordering**: Sorted according to natural ordering or a `Comparator`.
- **Performance**: Slower (O(log n)) for `add`, `remove`, and `contains`.

```java
import java.util.TreeSet;
import java.util.Set;

Set<String> sortedFruits = new TreeSet<>();
sortedFruits.add("Banana");
sortedFruits.add("Apple");
sortedFruits.add("Orange");

System.out.println(sortedFruits); // [Apple, Banana, Orange] (sorted order)
```

### 2. Garbage Collection (GC)

Garbage Collection is the process by which Java programs perform automatic memory management.

#### How it Works (Simplified)

1.  **Identification**: The Garbage Collector identifies which objects are in use and which are not. An object is considered "in use" if it's reachable from any active part of the program.
2.  **Reclamation**: The memory used by unreachable objects is reclaimed.

#### Key Points

- **Automatic**: You don't need to manually deallocate memory like in C/C++.
- **Non-deterministic**: You cannot predict exactly when the GC will run.
- **`System.gc()`**: This method _suggests_ that the JVM run the garbage collector, but it's not a guarantee. It's generally not recommended to call it.
- **Best Practice**: To make an object eligible for garbage collection, remove all references to it (e.g., set the reference variable to `null`).

```java
public class GcExample {
    public static void main(String[] args) {
        String s1 = new String("hello");
        // s1 is now pointing to an object

        s1 = null;
        // The "hello" object is now eligible for garbage collection
        // because there are no more references to it.
    }
}
```

### 3. Common Collection Interview Questions

#### 1. Difference between `HashMap` and `Hashtable`?

<details>
<summary>Answer</summary>

| Feature             | `HashMap`                                        | `Hashtable`                            |
| ------------------- | ------------------------------------------------ | -------------------------------------- |
| **Synchronization** | Not synchronized (not thread-safe)               | Synchronized (thread-safe)             |
| **Nulls**           | Allows one `null` key and multiple `null` values | Does not allow `null` keys or values   |
| **Performance**     | Faster                                           | Slower due to synchronization overhead |
| **Introduced**      | Java 1.2 (Collections Framework)                 | Java 1.0 (Legacy)                      |
| **Iteration**       | Uses `Iterator`                                  | Uses `Iterator` and `Enumeration`      |

</details>

#### 2. How does `HashMap` work internally?

<details>
<summary>Answer</summary>

`HashMap` works on the principle of hashing. It uses an array of "buckets" (nodes).

1.  When you `put(key, value)`, it calculates the `hashCode()` of the key.
2.  This hash code is used to find an index in the array (a bucket).
3.  If the bucket is empty, a new `Node(key, value)` is stored there.
4.  If the bucket is not empty (a "hash collision"), it checks if the key already exists using the `equals()` method.
    - If the key exists, it updates the value.
    - If the key doesn't exist, it adds the new node to the end of a linked list (or a balanced tree if the list gets too long) in that bucket.

</details>

#### 3. Difference between `ArrayList` and `LinkedList`?

<details>
<summary>Answer</summary>

This was covered in Day 7. `ArrayList` is an index-based dynamic array, fast for random access. `LinkedList` is a node-based list (each element points to the next and previous), fast for insertions/deletions in the middle.

</details>

#### 4. Difference between `Set` and `List`?

<details>
<summary>Answer</summary>

| Feature            | `List`                                        | `Set`                                                                                  |
| ------------------ | --------------------------------------------- | -------------------------------------------------------------------------------------- |
| **Duplicates**     | Allows duplicate elements                     | Does not allow duplicate elements                                                      |
| **Order**          | Maintains insertion order (e.g., `ArrayList`) | Generally unordered (e.g., `HashSet`), but can be ordered (`LinkedHashSet`, `TreeSet`) |
| **Element Access** | Can access elements by index (`get(index)`)   | Cannot access by index; must iterate                                                   |

</details>

---

## 🔄 Recap

In this session, we covered:

1.  **The Collections Framework**

    - The core interfaces: `Map`, `Set`, `List`.

2.  **Maps**

    - `HashMap` for fast, unordered key-value storage.
    - `LinkedHashMap` for insertion-ordered key-value storage.
    - `TreeMap` for key-sorted key-value storage.

3.  **Sets**

    - `HashSet` for fast, unordered unique element storage.
    - `LinkedHashSet` for insertion-ordered unique elements.
    - `TreeSet` for sorted unique elements.

4.  **Other Concepts**
    - Legacy collections (`Vector`, `Hashtable`) and their modern alternatives.
    - The basics of automatic Garbage Collection in Java.

## 🏆 Congratulations!

You have completed the 8-day Comprehensive Java Training Program. You now have a strong foundation in core Java, from the basics to advanced OOP concepts and the Collections Framework. Keep practicing to solidify your skills!
