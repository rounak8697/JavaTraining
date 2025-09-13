# 🗺️ Complete Java Map Interface Guide

**Date:** 2025-08-07

## Table of Contents
1. [What is a Map in Java?](#what-is-a-map-in-java)
2. [Map Internal Architecture](#map-internal-architecture)
3. [Types of Maps](#types-of-maps)
4. [Hashtable Deep Dive](#hashtable-deep-dive)
5. [Comparable vs Comparator](#comparable-vs-comparator)
6. [Performance Comparison](#performance-comparison)
7. [Best Practices](#best-practices)

---

## What is a Map in Java?

A **Map** is an object that maps **keys to values**. It cannot contain duplicate keys, and each key maps to at most one value. Maps are part of the Java Collections Framework and provide a way to store and retrieve data based on key-value pairs.

### Key Characteristics:
- No duplicate keys allowed
- Each key maps to exactly one value
- Values can be duplicated
- Keys and values can be null (depends on implementation)
- Part of `java.util` package

### Map Hierarchy:
```
Map<K,V> (Interface)
├── HashMap<K,V>
├── LinkedHashMap<K,V>
├── TreeMap<K,V> (implements NavigableMap)
├── ConcurrentHashMap<K,V>
├── Hashtable<K,V> (Legacy)
├── WeakHashMap<K,V>
├── IdentityHashMap<K,V>
└── EnumMap<K,V>

Sub-interfaces:
├── SortedMap<K,V>
└── NavigableMap<K,V> (extends SortedMap)
```

---

## Map Internal Architecture

### HashMap Internal Structure

```
HashMap Internal Structure:
┌─────────────────────────────────────┐
│          HashMap                     │
├─────────────────────────────────────┤
│  Node<K,V>[] table                  │ ← Array of buckets (default size: 16)
│  int size                           │ ← Number of key-value pairs
│  int threshold                      │ ← Resize threshold (capacity * loadFactor)
│  float loadFactor                   │ ← Load factor (default: 0.75f)
│  int modCount                       │ ← Modification count for fail-fast iteration
└─────────────────────────────────────┘

Each Node contains:
┌─────────────────┐
│ Node<K,V>       │
├─────────────────┤
│ int hash        │ ← Cached hash value
│ K key           │ ← Key object
│ V value         │ ← Value object  
│ Node<K,V> next  │ ← Reference to next node (for chaining)
└─────────────────┘

Hash Collision Handling (Java 8+):
┌─────────────────────────────────────┐
│ Bucket[i]                           │
├─────────────────────────────────────┤
│ If collision count < 8:             │
│   Linked List (Node)                │
│ If collision count >= 8:            │
│   Red-Black Tree (TreeNode)         │
│ If tree size < 6 after removal:     │
│   Convert back to Linked List       │
└─────────────────────────────────────┘
```

### HashMap Operations Flow

**PUT Operation:**
```
1. Calculate hash: hash(key) = key.hashCode() ^ (key.hashCode() >>> 16)
2. Find bucket index: index = hash & (capacity - 1)
3. Check if bucket is empty:
   - If empty: Create new node
   - If not empty: Handle collision
     - If key exists: Update value
     - If key doesn't exist: Add to chain/tree
4. Check if resize needed (size > threshold)
```

**GET Operation:**
```
1. Calculate hash of key
2. Find bucket index
3. Search in bucket:
   - If single node: Compare key
   - If linked list: Traverse and compare
   - If tree: Use tree search
4. Return value or null
```

### TreeMap Internal Structure

```
TreeMap Internal Structure:
┌─────────────────────────────────────┐
│          TreeMap                     │
├─────────────────────────────────────┤
│  Entry<K,V> root                    │ ← Root of Red-Black Tree
│  int size                           │ ← Number of entries
│  Comparator<K> comparator           │ ← Custom comparator (can be null)
│  int modCount                       │ ← Modification count
└─────────────────────────────────────┘

Red-Black Tree Properties:
1. Every node is either red or black
2. Root is always black
3. No two adjacent red nodes
4. Every path from root to leaf has same number of black nodes
5. New nodes are inserted as red

Entry Structure:
┌─────────────────┐
│ Entry<K,V>      │
├─────────────────┤
│ K key           │
│ V value         │
│ Entry left      │ ← Left child
│ Entry right     │ ← Right child  
│ Entry parent    │ ← Parent node
│ boolean color   │ ← RED or BLACK
└─────────────────┘
```

---

## Types of Maps

### 1. HashMap

**Internal Working:**
- Uses array of Node objects (buckets)
- Hash function determines bucket index
- Collision resolution: Chaining → Balanced Tree (Java 8+)
- Load factor triggers resizing

**Characteristics:**
- **Order**: No guaranteed order
- **Null**: Allows one `null` key and multiple `null` values
- **Thread Safe**: ❌ Not synchronized
- **Performance**: O(1) average time for get/put operations
- **Load Factor**: Default 0.75
- **Initial Capacity**: Default 16

```java
import java.util.*;

public class HashMapExample {
    public static void main(String[] args) {
        Map<String, Integer> fruitCount = new HashMap<>();
        
        // Adding elements
        fruitCount.put("apple", 5);
        fruitCount.put("banana", 3);
        fruitCount.put("orange", 8);
        fruitCount.put(null, 1); // null key allowed
        
        // Accessing elements
        System.out.println("Apples: " + fruitCount.get("apple"));
        
        // Iterating
        for (Map.Entry<String, Integer> entry : fruitCount.entrySet()) {
            System.out.println(entry.getKey() + " = " + entry.getValue());
        }
        
        // Advanced operations (Java 8+)
        fruitCount.compute("apple", (k, v) -> v == null ? 1 : v + 1);
        fruitCount.computeIfAbsent("grape", k -> 0);
        fruitCount.merge("banana", 2, Integer::sum);
    }
}
```

### 2. LinkedHashMap

**Internal Working:**
- Extends HashMap with doubly-linked list
- Maintains insertion order or access order
- Each entry has before/after pointers

**Structure:**
```
LinkedHashMap Structure:
HashMap buckets + Doubly Linked List

header ↔ entry1 ↔ entry2 ↔ entry3 ↔ ... ↔ tail

Each Entry extends HashMap.Node:
┌─────────────────┐
│ Entry<K,V>      │
├─────────────────┤
│ HashMap fields  │
│ Entry before    │ ← Previous in insertion order
│ Entry after     │ ← Next in insertion order
└─────────────────┘
```

```java
import java.util.*;

public class LinkedHashMapExample {
    public static void main(String[] args) {
        // Insertion order LinkedHashMap
        Map<String, String> insertionOrder = new LinkedHashMap<>();
        insertionOrder.put("first", "A");
        insertionOrder.put("second", "B");
        insertionOrder.put("third", "C");
        
        // LRU Cache implementation
        Map<String, String> lruCache = new LinkedHashMap<String, String>(16, 0.75f, true) {
            @Override
            protected boolean removeEldestEntry(Map.Entry<String, String> eldest) {
                return size() > 3; // Max 3 entries
            }
        };
        
        lruCache.put("A", "Value A");
        lruCache.put("B", "Value B");
        lruCache.put("C", "Value C");
        lruCache.get("A"); // Access A, moves it to end
        lruCache.put("D", "Value D"); // B gets removed
        
        System.out.println("LRU Cache: " + lruCache);
    }
}
```

### 3. TreeMap

**Internal Working:**
- Implements Red-Black Tree (self-balancing BST)
- Keys must be Comparable or use Comparator
- Maintains sorted order of keys

```java
import java.util.*;

public class TreeMapExample {
    public static void main(String[] args) {
        // Natural ordering
        TreeMap<Integer, String> naturalOrder = new TreeMap<>();
        naturalOrder.put(3, "Three");
        naturalOrder.put(1, "One");
        naturalOrder.put(4, "Four");
        naturalOrder.put(2, "Two");
        
        System.out.println("Natural Order: " + naturalOrder);
        
        // Custom comparator
        TreeMap<String, Integer> reverseOrder = new TreeMap<>(Collections.reverseOrder());
        reverseOrder.put("apple", 5);
        reverseOrder.put("banana", 3);
        reverseOrder.put("cherry", 8);
        
        // Navigation methods
        System.out.println("First key: " + naturalOrder.firstKey());
        System.out.println("Last key: " + naturalOrder.lastKey());
        System.out.println("Lower than 3: " + naturalOrder.lowerKey(3));
        System.out.println("Higher than 2: " + naturalOrder.higherKey(2));
        
        // SubMap operations
        SortedMap<Integer, String> subMap = naturalOrder.subMap(2, 4);
        System.out.println("SubMap [2,4): " + subMap);
    }
}
```

### 4. ConcurrentHashMap

**Internal Working (Java 8+):**
- Segment-based locking replaced with CAS operations
- Uses volatile variables and atomic operations
- Lock-free for most read operations

**Structure:**
```
ConcurrentHashMap Structure:
┌─────────────────────────────────────┐
│       ConcurrentHashMap              │
├─────────────────────────────────────┤
│  volatile Node<K,V>[] table         │ ← Volatile array
│  volatile Node<K,V>[] nextTable     │ ← For resizing
│  volatile int sizeCtl               │ ← Size control
│  AtomicLong baseCount               │ ← Base counter
│  CounterCell[] counterCells         │ ← Counter cells for contention
└─────────────────────────────────────┘

Node types:
- Node: Regular node
- TreeBin: Tree root node
- TreeNode: Tree node
- ForwardingNode: Used during resizing
```

```java
import java.util.concurrent.*;

public class ConcurrentHashMapExample {
    public static void main(String[] args) throws InterruptedException {
        ConcurrentHashMap<String, Integer> concurrentMap = new ConcurrentHashMap<>();
        
        concurrentMap.put("counter", 0);
        
        ExecutorService executor = Executors.newFixedThreadPool(10);
        
        for (int i = 0; i < 100; i++) {
            executor.submit(() -> {
                // Atomic operations
                concurrentMap.compute("counter", (key, val) -> val == null ? 1 : val + 1);
            });
        }
        
        executor.shutdown();
        executor.awaitTermination(1, TimeUnit.SECONDS);
        
        System.out.println("Final counter: " + concurrentMap.get("counter"));
        
        // Parallel operations (Java 8+)
        concurrentMap.put("item1", 10);
        concurrentMap.put("item2", 20);
        concurrentMap.put("item3", 30);
        
        // Parallel forEach
        concurrentMap.forEach(1, (key, value) -> {
            System.out.println(key + " = " + value);
        });
        
        // Parallel search
        String result = concurrentMap.search(1, (key, value) -> 
            value > 15 ? key : null);
        System.out.println("First key with value > 15: " + result);
    }
}
```

### 5. WeakHashMap

**Internal Working:**
- Keys are stored as WeakReference objects
- Garbage collector can collect keys when no strong references exist
- Uses ReferenceQueue to clean up stale entries

```java
import java.util.*;

public class WeakHashMapExample {
    public static void main(String[] args) {
        WeakHashMap<Object, String> weakMap = new WeakHashMap<>();
        
        Object key1 = new String("key1");
        Object key2 = new String("key2");
        
        weakMap.put(key1, "value1");
        weakMap.put(key2, "value2");
        
        System.out.println("Before GC: " + weakMap.size());
        
        key1 = null; // Remove strong reference
        System.gc();
        
        // Wait for GC
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        
        System.out.println("After GC: " + weakMap.size());
        
        // Canonical mapping example
        WeakHashMap<String, String> canonicalMap = new WeakHashMap<>();
        // Use for object interning patterns
    }
}
```

### 6. IdentityHashMap

**Internal Working:**
- Uses linear probing instead of chaining
- Comparison using `==` instead of `.equals()`
- Hash function: `System.identityHashCode()`

```java
import java.util.*;

public class IdentityHashMapExample {
    public static void main(String[] args) {
        IdentityHashMap<String, String> identityMap = new IdentityHashMap<>();
        
        String key1 = new String("hello");
        String key2 = new String("hello");
        String key3 = "hello"; // String literal
        
        identityMap.put(key1, "value1");
        identityMap.put(key2, "value2");
        identityMap.put(key3, "value3");
        
        System.out.println("Size: " + identityMap.size());
        System.out.println("key1 == key2: " + (key1 == key2));
        System.out.println("key1.equals(key2): " + key1.equals(key2));
        
        // Object tracking example
        List<Object> objects = Arrays.asList(new Object(), new Object());
        IdentityHashMap<Object, Integer> objectIds = new IdentityHashMap<>();
        
        int id = 1;
        for (Object obj : objects) {
            objectIds.put(obj, id++);
        }
    }
}
```

### 7. EnumMap

**Internal Working:**
- Internally uses arrays for maximum efficiency
- Array index corresponds to enum ordinal
- Maintains natural enum declaration order

**Structure:**
```
EnumMap Structure:
┌─────────────────────────────────────┐
│          EnumMap                     │
├─────────────────────────────────────┤
│  Class<K> keyType                   │ ← Enum class
│  K[] keyUniverse                    │ ← All enum constants
│  Object[] vals                      │ ← Values array
│  int size                           │ ← Number of mappings
└─────────────────────────────────────┘

Access: O(1) - Direct array access using enum.ordinal()
```

```java
import java.util.*;

enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}

enum Priority {
    LOW, MEDIUM, HIGH, CRITICAL
}

public class EnumMapExample {
    public static void main(String[] args) {
        // Schedule mapping
        EnumMap<Day, String> schedule = new EnumMap<>(Day.class);
        schedule.put(Day.MONDAY, "Team Meeting");
        schedule.put(Day.WEDNESDAY, "Code Review");
        schedule.put(Day.FRIDAY, "Sprint Planning");
        
        // Task priority mapping
        EnumMap<Priority, List<String>> tasksByPriority = new EnumMap<>(Priority.class);
        tasksByPriority.put(Priority.HIGH, Arrays.asList("Fix critical bug", "Deploy hotfix"));
        tasksByPriority.put(Priority.MEDIUM, Arrays.asList("Code review", "Update docs"));
        
        // State machine using EnumMap
        EnumMap<Priority, Integer> priorityCounts = new EnumMap<>(Priority.class);
        for (Priority p : Priority.values()) {
            priorityCounts.put(p, 0);
        }
        
        // Efficient enum-based operations
        priorityCounts.compute(Priority.HIGH, (k, v) -> v + 1);
    }
}
```

---

## Hashtable Deep Dive

### What is Hashtable?

**Hashtable** is a legacy implementation of the Map interface that has been part of Java since JDK 1.0. It's similar to HashMap but with key differences in thread safety and null handling.

### Internal Architecture

```
Hashtable Internal Structure:
┌─────────────────────────────────────┐
│          Hashtable                   │
├─────────────────────────────────────┤
│  Entry<K,V>[] table                 │ ← Array of entries
│  int count                          │ ← Number of entries
│  int threshold                      │ ← Rehash threshold
│  float loadFactor                   │ ← Load factor (default: 0.75)
│  int modCount                       │ ← Modification count
└─────────────────────────────────────┘

Entry Structure:
┌─────────────────┐
│ Entry<K,V>      │
├─────────────────┤
│ int hash        │ ← Hash value
│ K key           │ ← Key (cannot be null)
│ V value         │ ← Value (cannot be null)
│ Entry<K,V> next │ ← Next entry in chain
└─────────────────┘

Synchronization: All methods are synchronized
```

### Key Characteristics

| Feature | Hashtable | HashMap |
|---------|-----------|---------|
| **Thread Safety** | ✅ Synchronized | ❌ Not synchronized |
| **Null Keys** | ❌ Not allowed | ✅ One null key allowed |
| **Null Values** | ❌ Not allowed | ✅ Multiple null values |
| **Inheritance** | Extends Dictionary | Extends AbstractMap |
| **Iteration** | Fail-fast + Enumerator | Fail-fast only |
| **Performance** | Slower (synchronization overhead) | Faster |
| **Initial Capacity** | 11 | 16 |

### Hashtable vs HashMap vs ConcurrentHashMap

```java
import java.util.*;
import java.util.concurrent.*;

public class HashtableComparison {
    public static void main(String[] args) {
        // Hashtable - Legacy thread-safe
        Hashtable<String, String> hashtable = new Hashtable<>();
        hashtable.put("key1", "value1");
        // hashtable.put(null, "value"); // Throws NullPointerException
        // hashtable.put("key", null);   // Throws NullPointerException
        
        // HashMap - Not thread-safe, allows nulls
        HashMap<String, String> hashMap = new HashMap<>();
        hashMap.put("key1", "value1");
        hashMap.put(null, "nullKeyValue");
        hashMap.put("nullValue", null);
        
        // ConcurrentHashMap - Modern thread-safe
        ConcurrentHashMap<String, String> concurrentMap = new ConcurrentHashMap<>();
        concurrentMap.put("key1", "value1");
        // concurrentMap.put(null, "value"); // Throws NullPointerException
        // concurrentMap.put("key", null);   // Throws NullPointerException
        
        // Enumeration vs Iterator
        System.out.println("Hashtable enumeration:");
        Enumeration<String> keys = hashtable.keys();
        while (keys.hasMoreElements()) {
            String key = keys.nextElement();
            System.out.println(key + " = " + hashtable.get(key));
        }
        
        // Properties extends Hashtable
        Properties props = new Properties();
        props.setProperty("database.url", "jdbc:mysql://localhost/mydb");
        props.setProperty("database.user", "admin");
        props.setProperty("database.password", "secret");
        
        System.out.println("Properties:");
        props.forEach((key, value) -> System.out.println(key + " = " + value));
    }
}
```

### When to Use Hashtable

**Use Hashtable when:**
- Working with legacy code that requires it
- Need simple synchronization without complexity
- Working with Properties files
- Compatibility with older Java versions

**Prefer alternatives:**
- **ConcurrentHashMap** for modern concurrent applications
- **HashMap** with external synchronization
- **Collections.synchronizedMap()** wrapper

---

## Comparable vs Comparator

### Comparable Interface

**Definition:** `Comparable` is an interface that allows objects to define their natural ordering.

```java
public interface Comparable<T> {
    public int compareTo(T o);
}
```

**Key Points:**
- **Natural Ordering**: Defines the default sorting order
- **Single Sorting Logic**: Only one way to compare
- **Self-Comparison**: Object compares itself with another
- **Consistency**: Must be consistent with `equals()`

**Return Values:**
- **Negative**: This object is less than the specified object
- **Zero**: This object equals the specified object  
- **Positive**: This object is greater than the specified object

### Comparable Example

```java
import java.util.*;

class Student implements Comparable<Student> {
    private String name;
    private int age;
    private double gpa;
    
    public Student(String name, int age, double gpa) {
        this.name = name;
        this.age = age;
        this.gpa = gpa;
    }
    
    // Natural ordering by GPA (descending)
    @Override
    public int compareTo(Student other) {
        return Double.compare(other.gpa, this.gpa); // Reverse order for descending
    }
    
    // Must be consistent with compareTo
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Student student = (Student) obj;
        return Double.compare(student.gpa, gpa) == 0 &&
               age == student.age &&
               Objects.equals(name, student.name);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(name, age, gpa);
    }
    
    @Override
    public String toString() {
        return String.format("Student{name='%s', age=%d, gpa=%.2f}", name, age, gpa);
    }
    
    // Getters
    public String getName() { return name; }
    public int getAge() { return age; }
    public double getGpa() { return gpa; }
}

public class ComparableExample {
    public static void main(String[] args) {
        List<Student> students = Arrays.asList(
            new Student("Alice", 20, 3.8),
            new Student("Bob", 19, 3.9),
            new Student("Charlie", 21, 3.7),
            new Student("David", 20, 3.9)
        );
        
        System.out.println("Original list:");
        students.forEach(System.out::println);
        
        // Natural sorting using Comparable
        Collections.sort(students);
        System.out.println("\nSorted by GPA (natural ordering):");
        students.forEach(System.out::println);
        
        // Using TreeMap with Comparable
        Map<Student, String> studentGrades = new TreeMap<>();
        for (Student s : students) {
            studentGrades.put(s, "Grade for " + s.getName());
        }
        
        System.out.println("\nTreeMap with Comparable:");
        studentGrades.forEach((student, grade) -> 
            System.out.println(student + " -> " + grade));
    }
}
```

### Comparator Interface

**Definition:** `Comparator` is a functional interface that allows external comparison logic.

```java
@FunctionalInterface
public interface Comparator<T> {
    int compare(T o1, T o2);
    
    // Default methods (Java 8+)
    default Comparator<T> reversed() { ... }
    default Comparator<T> thenComparing(Comparator<? super T> other) { ... }
    static <T extends Comparable<? super T>> Comparator<T> naturalOrder() { ... }
    static <T extends Comparable<? super T>> Comparator<T> reverseOrder() { ... }
    // ... many more utility methods
}
```

**Key Points:**
- **External Logic**: Comparison logic is separate from the class
- **Multiple Sorting**: Can have multiple different comparators
- **Flexibility**: Can sort objects that don't implement Comparable
- **Method References**: Works well with lambda expressions

### Comparator Examples

```java
import java.util.*;
import java.util.function.*;

public class ComparatorExample {
    public static void main(String[] args) {
        List<Student> students = Arrays.asList(
            new Student("Alice", 20, 3.8),
            new Student("Bob", 19, 3.9),
            new Student("Charlie", 21, 3.7),
            new Student("David", 20, 3.9),
            new Student("Eve", 19, 3.8)
        );
        
        System.out.println("Original list:");
        students.forEach(System.out::println);
        
        // 1. Sort by name (ascending)
        students.sort(Comparator.comparing(Student::getName));
        System.out.println("\nSorted by name:");
        students.forEach(System.out::println);
        
        // 2. Sort by age (descending)
        students.sort(Comparator.comparing(Student::getAge).reversed());
        System.out.println("\nSorted by age (descending):");
        students.forEach(System.out::println);
        
        // 3. Complex sorting: GPA descending, then by name ascending
        students.sort(Comparator.comparing(Student::getGpa).reversed()
                     .thenComparing(Student::getName));
        System.out.println("\nSorted by GPA desc, then name asc:");
        students.forEach(System.out::println);
        
        // 4. Custom lambda comparator
        students.sort((s1, s2) -> {
            int ageCompare = Integer.compare(s1.getAge(), s2.getAge());
            if (ageCompare != 0) return ageCompare;
            return Double.compare(s2.getGpa(), s1.getGpa()); // GPA descending if same age
        });
        System.out.println("\nCustom sorting (age asc, then GPA desc):");
        students.forEach(System.out::println);
        
        // 5. Using TreeMap with custom Comparator
        Map<Student, String> studentMap = new TreeMap<>(
            Comparator.comparing(Student::getGpa).reversed()
                     .thenComparing(Student::getName)
        );
        
        for (Student s : students) {
            studentMap.put(s, "Info for " + s.getName());
        }
        
        System.out.println("\nTreeMap with custom Comparator:");
        studentMap.forEach((student, info) -> 
            System.out.println(student + " -> " + info));
    }
}
```

### Advanced Comparator Operations

```java
import java.util.*;
import java.util.stream.*;

public class AdvancedComparatorExample {
    public static void main(String[] args) {
        List<Student> students = Arrays.asList(
            new Student("Alice", 20, 3.8),
            new Student("Bob", 19, 3.9),
            new Student("Charlie", 21, 3.7),
            new Student("David", 20, 3.9)
        );
        
        // 1. Null-safe comparator
        List<Student> studentsWithNull = new ArrayList<>(students);
        studentsWithNull.add(null);
        
        Comparator<Student> nullSafeComparator = Comparator
            .nullsLast(Comparator.comparing(Student::getName));
        
        studentsWithNull.sort(nullSafeComparator);
        System.out.println("Null-safe sorting:");
        studentsWithNull.forEach(System.out::println);
        
        // 2. Multiple criteria with different orders
        Comparator<Student> multiCriteria = Comparator
            .comparing(Student::getGpa).reversed()           // GPA descending
            .thenComparing(Student::getAge)                  // Age ascending
            .thenComparing(Student::getName);                // Name ascending
        
        students.sort(multiCriteria);
        System.out.println("\nMultiple criteria sorting:");
        students.forEach(System.out::println);
        
        // 3. Comparator with custom key extractor
        Comparator<Student> customExtractor = Comparator.comparing(
            student -> student.getName().length()           // Sort by name length
        ).thenComparing(Student::getName);                   // Then by name
        
        students.sort(customExtractor);
        System.out.println("\nSort by name length, then name:");
        students.forEach(System.out::println);
        
        // 4. Using Comparator in Stream operations
        Optional<Student> topStudent = students.stream()
            .max(Comparator.comparing(Student::getGpa));
        
        List<Student> topTwoByGPA = students.stream()
            .sorted(Comparator.comparing(Student::getGpa).reversed())
            .limit(2)
            .collect(Collectors.toList());
        
        System.out.println("\nTop student by GPA: " + topStudent.orElse(null));
        System.out.println("Top 2 students by GPA:");
        topTwoByGPA.forEach(System.out::println);
        
        // 5. Grouping with custom comparator
        Map<Integer, List<Student>> groupedByAge = students.stream()
            .collect(Collectors.groupingBy(
                Student::getAge,
                () -> new TreeMap<>(Integer::compareTo), // Sorted map
                Collectors.toList()
            ));
        
        System.out.println("\nGrouped by age (sorted):");
        groupedByAge.forEach((age, studentList) -> {
            System.out.println("Age " + age + ": " + studentList);
        });
    }
}
```

### Comparable vs Comparator Summary

| Aspect | Comparable | Comparator |
|--------|------------|------------|
| **Location** | Inside the class being compared | External to the class |
| **Method** | `compareTo(T o)` | `compare(T o1, T o2)` |
| **Sorting Logic** | Single natural ordering | Multiple custom orderings |
| **Modification** | Requires modifying the class | No class modification needed |
| **Usage** | `Collections.sort(list)` | `Collections.sort(list, comparator)` |
| **Interface Type** | Regular interface | Functional interface (Java 8+) |
| **Lambda Support** | ❌ Cannot use lambdas | ✅ Supports lambdas and method references |
| **Flexibility** | Less flexible | More flexible |
| **Performance** | Slightly faster (direct method call) | Slightly slower (additional object) |

### When to Use Which?

**Use Comparable when:**
- There's a clear, natural ordering for the class
- You control the source code of the class
- Most sorting will use this default order
- Example: Numbers, Strings, Dates

**Use Comparator when:**
- Need multiple different sorting orders
- Cannot modify the class (third-party classes)
- Sorting logic is context-dependent
- Want to use lambda expressions
- Example: Sorting by different criteria in different situations

---

## Performance Comparison

### Time Complexity Analysis

| Operation | HashMap | LinkedHashMap | TreeMap | ConcurrentHashMap | Hashtable | EnumMap |
|-----------|---------|---------------|---------|-------------------|-----------|---------|
| **get()** | O(1) avg, O(n) worst | O(1) avg, O(n) worst | O(log n) | O(1) avg, O(n) worst | O(1) avg, O(n) worst | O(1) |
| **put()** | O(1) avg, O(n) worst | O(1) avg, O(n) worst | O(log n) | O(1) avg, O(n) worst | O(1) avg, O(n) worst | O(1) |
| **remove()** | O(1) avg, O(n) worst | O(1) avg, O(n) worst | O(log n) | O(1) avg, O(n) worst | O(1) avg, O(n) worst | O(1) |
| **containsKey()** | O(1) avg, O(n) worst | O(1) avg, O(n) worst | O(log n) | O(1) avg, O(n) worst | O(1) avg, O(n) worst | O(1) |
| **Iteration** | O(capacity + n) | O(n) | O(n) | O(capacity + n) | O(capacity + n) | O(n) |

### Memory Usage Comparison

```java
import java.util.*;

public class MemoryUsageExample {
    public static void main(String[] args) {
        int size = 100000;
        
        // Test different map implementations
        testMapMemory("HashMap", new HashMap<>(), size);
        testMapMemory("LinkedHashMap", new LinkedHashMap<>(), size);
        testMapMemory("TreeMap", new TreeMap<>(), size);
        testMapMemory("ConcurrentHashMap", new ConcurrentHashMap<>(), size);
        testMapMemory("EnumMap", createEnumMap(), 1000); // Smaller size for enum
    }
    
    private static void testMapMemory(String mapType, Map<Object, String> map, int size) {
        long startMemory = getUsedMemory();
        
        // Populate map
        for (int i = 0; i < size; i++) {
            map.put(i, "Value" + i);
        }
        
        long endMemory = getUsedMemory();
        long memoryUsed = endMemory - startMemory;
        
        System.out.printf("%s with %d entries uses approximately %d bytes%n", 
                         mapType, size, memoryUsed);
    }
    
    private static long getUsedMemory() {
        Runtime runtime = Runtime.getRuntime();
        System.gc(); // Suggest garbage collection
        return runtime.totalMemory() - runtime.freeMemory();
    }
    
    private static EnumMap<TestEnum, String> createEnumMap() {
        return new EnumMap<>(TestEnum.class);
    }
    
    enum TestEnum {
        VALUE1, VALUE2, VALUE3, VALUE4, VALUE5;
    }
}
```

### Performance Benchmarking

```java
import java.util.*;
import java.util.concurrent.*;

public class MapPerformanceBenchmark {
    private static final int OPERATIONS = 1_000_000;
    
    public static void main(String[] args) {
        System.out.println("Map Performance Benchmark (" + OPERATIONS + " operations)");
        System.out.println("=" * 60);
        
        // Test different implementations
        benchmarkMap("HashMap", new HashMap<>());
        benchmarkMap("LinkedHashMap", new LinkedHashMap<>());
        benchmarkMap("TreeMap", new TreeMap<>());
        benchmarkMap("ConcurrentHashMap", new ConcurrentHashMap<>());
        benchmarkMap("Hashtable", new Hashtable<>());
    }
    
    private static void benchmarkMap(String mapType, Map<Integer, String> map) {
        System.out.printf("%nTesting %s:%n", mapType);
        
        // PUT operations
        long startTime = System.nanoTime();
        for (int i = 0; i < OPERATIONS; i++) {
            map.put(i, "Value" + i);
        }
        long putTime = System.nanoTime() - startTime;
        
        // GET operations
        startTime = System.nanoTime();
        for (int i = 0; i < OPERATIONS; i++) {
            map.get(i);
        }
        long getTime = System.nanoTime() - startTime;
        
        // ITERATION
        startTime = System.nanoTime();
        for (Map.Entry<Integer, String> entry : map.entrySet()) {
            // Just iterate
        }
        long iterTime = System.nanoTime() - startTime;
        
        // REMOVE operations
        startTime = System.nanoTime();
        for (int i = 0; i < OPERATIONS / 2; i++) {
            map.remove(i);
        }
        long removeTime = System.nanoTime() - startTime;
        
        System.out.printf("  PUT:    %6.2f ms%n", putTime / 1_000_000.0);
        System.out.printf("  GET:    %6.2f ms%n", getTime / 1_000_000.0);
        System.out.printf("  ITER:   %6.2f ms%n", iterTime / 1_000_000.0);
        System.out.printf("  REMOVE: %6.2f ms%n", removeTime / 1_000_000.0);
    }
}
```

### Concurrent Performance

```java
import java.util.*;
import java.util.concurrent.*;

public class ConcurrentMapBenchmark {
    private static final int THREADS = 10;
    private static final int OPERATIONS_PER_THREAD = 100_000;
    
    public static void main(String[] args) throws InterruptedException {
        System.out.println("Concurrent Map Performance Benchmark");
        System.out.println("Threads: " + THREADS + ", Operations per thread: " + OPERATIONS_PER_THREAD);
        System.out.println("=" * 60);
        
        // Test thread-safe implementations
        benchmarkConcurrentMap("ConcurrentHashMap", new ConcurrentHashMap<>());
        benchmarkConcurrentMap("Hashtable", new Hashtable<>());
        benchmarkConcurrentMap("SynchronizedHashMap", 
                              Collections.synchronizedMap(new HashMap<>()));
    }
    
    private static void benchmarkConcurrentMap(String mapType, Map<Integer, String> map) 
            throws InterruptedException {
        
        System.out.printf("%nTesting %s:%n", mapType);
        
        ExecutorService executor = Executors.newFixedThreadPool(THREADS);
        CountDownLatch latch = new CountDownLatch(THREADS);
        
        long startTime = System.nanoTime();
        
        // Start concurrent operations
        for (int t = 0; t < THREADS; t++) {
            final int threadId = t;
            executor.submit(() -> {
                try {
                    int start = threadId * OPERATIONS_PER_THREAD;
                    int end = start + OPERATIONS_PER_THREAD;
                    
                    // Mixed operations: 50% put, 30% get, 20% remove
                    for (int i = start; i < end; i++) {
                        int operation = i % 10;
                        if (operation < 5) {
                            map.put(i, "Value" + i);
                        } else if (operation < 8) {
                            map.get(i);
                        } else {
                            map.remove(i);
                        }
                    }
                } finally {
                    latch.countDown();
                }
            });
        }
        
        latch.await();
        long endTime = System.nanoTime();
        
        executor.shutdown();
        
        double totalTime = (endTime - startTime) / 1_000_000.0;
        int totalOperations = THREADS * OPERATIONS_PER_THREAD;
        
        System.out.printf("  Total time: %8.2f ms%n", totalTime);
        System.out.printf("  Throughput: %8.0f ops/sec%n", 
                         totalOperations / (totalTime / 1000));
        System.out.printf("  Final size: %8d entries%n", map.size());
    }
}
```

---

## Best Practices and Guidelines

### 1. Choosing the Right Map Implementation

```java
// Decision flow for choosing Map implementation
public class MapSelectionGuide {
    public static <K, V> Map<K, V> selectMap(MapRequirements requirements) {
        // Thread safety first
        if (requirements.needsThreadSafety()) {
            if (requirements.needsHighConcurrency()) {
                return new ConcurrentHashMap<>();
            } else {
                // Legacy or simple synchronization
                return new Hashtable<>();
                // or Collections.synchronizedMap(new HashMap<>());
            }
        }
        
        // Key type considerations
        if (requirements.getKeyType().isEnum()) {
            return new EnumMap<>((Class<Enum>) requirements.getKeyType());
        }
        
        // Ordering requirements
        if (requirements.needsSorting()) {
            if (requirements.hasCustomComparator()) {
                return new TreeMap<>(requirements.getComparator());
            } else {
                return new TreeMap<>(); // Natural ordering
            }
        }
        
        if (requirements.needsInsertionOrder() || requirements.needsAccessOrder()) {
            return new LinkedHashMap<>(16, 0.75f, requirements.needsAccessOrder());
        }
        
        // Special cases
        if (requirements.needsWeakReferences()) {
            return new WeakHashMap<>();
        }
        
        if (requirements.needsIdentityComparison()) {
            return new IdentityHashMap<>();
        }
        
        // Default choice
        return new HashMap<>();
    }
    
    // Helper class for requirements
    static class MapRequirements {
        private boolean threadSafety = false;
        private boolean highConcurrency = false;
        private boolean sorting = false;
        private boolean insertionOrder = false;
        private boolean accessOrder = false;
        private boolean weakReferences = false;
        private boolean identityComparison = false;
        private Class<?> keyType = Object.class;
        private Comparator<?> comparator = null;
        
        // Builder pattern methods
        public MapRequirements threadSafe() { this.threadSafety = true; return this; }
        public MapRequirements highConcurrency() { this.highConcurrency = true; return this; }
        public MapRequirements sorted() { this.sorting = true; return this; }
        public MapRequirements insertionOrder() { this.insertionOrder = true; return this; }
        public MapRequirements accessOrder() { this.accessOrder = true; return this; }
        public MapRequirements weakReferences() { this.weakReferences = true; return this; }
        public MapRequirements identityComparison() { this.identityComparison = true; return this; }
        public MapRequirements keyType(Class<?> type) { this.keyType = type; return this; }
        public <T> MapRequirements comparator(Comparator<T> comp) { this.comparator = comp; return this; }
        
        // Getters
        public boolean needsThreadSafety() { return threadSafety; }
        public boolean needsHighConcurrency() { return highConcurrency; }
        public boolean needsSorting() { return sorting; }
        public boolean needsInsertionOrder() { return insertionOrder; }
        public boolean needsAccessOrder() { return accessOrder; }
        public boolean needsWeakReferences() { return weakReferences; }
        public boolean needsIdentityComparison() { return identityComparison; }
        public Class<?> getKeyType() { return keyType; }
        public boolean hasCustomComparator() { return comparator != null; }
        public Comparator getComparator() { return comparator; }
    }
}
```

### 2. Optimization Techniques

```java
import java.util.*;
import java.util.concurrent.*;

public class MapOptimizationTechniques {
    
    // 1. Proper sizing to avoid rehashing
    public static void properSizing() {
        // Bad: Default size causes multiple rehashes
        Map<String, String> badMap = new HashMap<>();
        
        // Good: Pre-size based on expected load
        int expectedSize = 1000;
        int initialCapacity = (int) (expectedSize / 0.75f) + 1;
        Map<String, String> goodMap = new HashMap<>(initialCapacity);
        
        // For ConcurrentHashMap
        Map<String, String> concurrentMap = new ConcurrentHashMap<>(initialCapacity);
    }
    
    // 2. Efficient key objects
    public static void efficientKeys() {
        // Bad: Mutable keys
        class MutableKey {
            private String value;
            public void setValue(String value) { this.value = value; }
            // hashCode and equals based on value
        }
        
        // Good: Immutable keys
        final class ImmutableKey {
            private final String value;
            private final int hashCode; // Cache hash code
            
            public ImmutableKey(String value) {
                this.value = value;
                this.hashCode = Objects.hashCode(value);
            }
            
            @Override
            public int hashCode() {
                return hashCode; // Return cached value
            }
            
            @Override
            public boolean equals(Object obj) {
                if (this == obj) return true;
                if (!(obj instanceof ImmutableKey)) return false;
                return Objects.equals(value, ((ImmutableKey) obj).value);
            }
        }
    }
    
    // 3. Batch operations for better performance
    public static void batchOperations() {
        Map<String, String> source = new HashMap<>();
        Map<String, String> destination = new HashMap<>();
        
        // Bad: Individual puts
        for (Map.Entry<String, String> entry : source.entrySet()) {
            destination.put(entry.getKey(), entry.getValue());
        }
        
        // Good: Batch operation
        destination.putAll(source);
        
        // Java 8+ bulk operations for ConcurrentHashMap
        ConcurrentHashMap<String, Integer> concurrentMap = new ConcurrentHashMap<>();
        
        // Parallel forEach
        concurrentMap.forEach(1, (key, value) -> {
            // Process in parallel
        });
        
        // Parallel search
        String result = concurrentMap.search(1, (key, value) -> 
            value > 100 ? key : null);
        
        // Parallel reduction
        int sum = concurrentMap.reduce(1, (key, value) -> value, 0, Integer::sum);
    }
    
    // 4. Memory-efficient patterns
    public static void memoryEfficient() {
        // Use EnumMap for enum keys
        enum Status { ACTIVE, INACTIVE, PENDING }
        Map<Status, String> statusMap = new EnumMap<>(Status.class); // Most efficient
        
        // Use primitive collections if available (external libraries)
        // TroveMap, Eclipse Collections, etc.
        
        // Consider using arrays for small, known sets
        // Instead of Map<Integer, String> for indices 0-10
        String[] smallMap = new String[11]; // More memory efficient
    }
    
    // 5. Thread-safe patterns
    public static void threadSafePatterns() {
        ConcurrentHashMap<String, Integer> counter = new ConcurrentHashMap<>();
        
        // Atomic increment
        counter.compute("key", (k, v) -> v == null ? 1 : v + 1);
        
        // Atomic put-if-absent
        counter.putIfAbsent("key", 0);
        
        // Atomic replace
        counter.replace("key", 0, 1);
        
        // Compute if absent for expensive operations
        Map<String, List<String>> cache = new ConcurrentHashMap<>();
        List<String> result = cache.computeIfAbsent("key", k -> {
            // Expensive computation here
            return Arrays.asList("computed", "values");
        });
    }
}
```

### 3. Common Pitfalls and Solutions

```java
import java.util.*;
import java.util.concurrent.*;

public class MapPitfallsAndSolutions {
    
    // Pitfall 1: ConcurrentModificationException
    public static void concurrentModificationPitfall() {
        Map<String, String> map = new HashMap<>();
        map.put("key1", "value1");
        map.put("key2", "value2");
        map.put("key3", "value3");
        
        // Bad: Modifying map during iteration
        try {
            for (Map.Entry<String, String> entry : map.entrySet()) {
                if (entry.getKey().equals("key2")) {
                    map.remove("key2"); // Throws ConcurrentModificationException
                }
            }
        } catch (ConcurrentModificationException e) {
            System.out.println("ConcurrentModificationException caught");
        }
        
        // Solution 1: Use iterator
        Iterator<Map.Entry<String, String>> iterator = map.entrySet().iterator();
        while (iterator.hasNext()) {
            Map.Entry<String, String> entry = iterator.next();
            if (entry.getKey().equals("key2")) {
                iterator.remove(); // Safe removal
            }
        }
        
        // Solution 2: Collect keys to remove first
        Set<String> keysToRemove = new HashSet<>();
        for (Map.Entry<String, String> entry : map.entrySet()) {
            if (shouldRemove(entry)) {
                keysToRemove.add(entry.getKey());
            }
        }
        keysToRemove.forEach(map::remove);
        
        // Solution 3: Use ConcurrentHashMap for concurrent modifications
        ConcurrentHashMap<String, String> concurrentMap = new ConcurrentHashMap<>(map);
        for (Map.Entry<String, String> entry : concurrentMap.entrySet()) {
            if (shouldRemove(entry)) {
                concurrentMap.remove(entry.getKey()); // Safe in ConcurrentHashMap
            }
        }
    }
    
    // Pitfall 2: Incorrect hashCode/equals implementation
    public static void hashCodeEqualsPitfall() {
        class BadKey {
            private String value;
            
            public BadKey(String value) { this.value = value; }
            
            // Bad: Only overrides equals, not hashCode
            @Override
            public boolean equals(Object obj) {
                return obj instanceof BadKey && 
                       Objects.equals(value, ((BadKey) obj).value);
            }
            // Missing hashCode() override!
        }
        
        class GoodKey {
            private final String value;
            
            public GoodKey(String value) { this.value = value; }
            
            @Override
            public boolean equals(Object obj) {
                return obj instanceof GoodKey && 
                       Objects.equals(value, ((GoodKey) obj).value);
            }
            
            @Override
            public int hashCode() {
                return Objects.hashCode(value);
            }
        }
        
        // Demonstrate the issue
        Map<BadKey, String> badMap = new HashMap<>();
        BadKey key1 = new BadKey("test");
        BadKey key2 = new BadKey("test");
        
        badMap.put(key1, "value1");
        System.out.println("Bad map get with equal key: " + badMap.get(key2)); // null!
        
        Map<GoodKey, String> goodMap = new HashMap<>();
        GoodKey goodKey1 = new GoodKey("test");
        GoodKey goodKey2 = new GoodKey("test");
        
        goodMap.put(goodKey1, "value1");
        System.out.println("Good map get with equal key: " + goodMap.get(goodKey2)); // value1
    }
    
    // Pitfall 3: Memory leaks with static maps
    public static class MemoryLeakExample {
        private static final Map<String, Object> STATIC_CACHE = new HashMap<>();
        
        // Bad: No cleanup mechanism
        public static void addToCache(String key, Object value) {
            STATIC_CACHE.put(key, value);
        }
        
        // Good: Size-limited cache
        private static final Map<String, Object> LIMITED_CACHE = 
            new LinkedHashMap<String, Object>(100, 0.75f, true) {
                @Override
                protected boolean removeEldestEntry(Map.Entry<String, Object> eldest) {
                    return size() > 100;
                }
            };
        
        // Better: Use proper cache library or WeakHashMap
        private static final Map<Object, String> WEAK_CACHE = new WeakHashMap<>();
    }
    
    // Pitfall 4: Null handling inconsistencies
    public static void nullHandlingPitfall() {
        // Different null policies
        Map<String, String> hashMap = new HashMap<>();
        hashMap.put(null, "nullKey");
        hashMap.put("nullValue", null);
        System.out.println("HashMap allows nulls: " + hashMap);
        
        try {
            Map<String, String> treeMap = new TreeMap<>();
            treeMap.put(null, "nullKey"); // Throws NullPointerException
        } catch (NullPointerException e) {
            System.out.println("TreeMap doesn't allow null keys");
        }
        
        try {
            Map<String, String> concurrentMap = new ConcurrentHashMap<>();
            concurrentMap.put(null, "nullKey"); // Throws NullPointerException
        } catch (NullPointerException e) {
            System.out.println("ConcurrentHashMap doesn't allow nulls");
        }
    }
    
    private static boolean shouldRemove(Map.Entry<String, String> entry) {
        return entry.getKey().startsWith("remove");
    }
}
```

---

## Final Summary and Quick Reference

### Map Selection Decision Tree

```
Need thread safety?
├─ Yes → High concurrency?
│  ├─ Yes → ConcurrentHashMap
│  └─ No → Hashtable or Collections.synchronizedMap()
└─ No → Key type?
   ├─ Enum → EnumMap
   ├─ Need sorting?
   │  ├─ Yes → TreeMap
   │  └─ No → Need insertion order?
   │     ├─ Yes → LinkedHashMap
   │     └─ No → Special requirements?
   │        ├─ Weak references → WeakHashMap
   │        ├─ Identity comparison → IdentityHashMap
   │        └─ Default → HashMap
```

### Quick Reference Table

| Map Type | Best Use Case | Key Features |
|----------|---------------|--------------|
| **HashMap** | General purpose mapping | Fast, allows nulls, no ordering |
| **LinkedHashMap** | Ordered maps, LRU caches | Maintains insertion/access order |
| **TreeMap** | Sorted maps, range queries | Red-black tree, O(log n) operations |
| **ConcurrentHashMap** | High-concurrency applications | Thread-safe, lock-free reads |
| **Hashtable** | Legacy thread-safe maps | Synchronized, no nulls allowed |
| **EnumMap** | Enum-keyed mappings | Most efficient for enum keys |
| **WeakHashMap** | Memory-sensitive caches | Entries removed when keys are GC'd |
| **IdentityHashMap** | Object identity tracking | Uses == instead of equals() |

### Performance Guidelines

1. **HashMap**: Default choice for single-threaded applications
2. **TreeMap**: When you need sorted keys (O(log n) operations)
3. **LinkedHashMap**: When insertion order matters
4. **ConcurrentHashMap**: For concurrent access without synchronization overhead
5. **EnumMap**: Always use for enum keys (fastest and most memory-efficient)

### Thread Safety Summary

| Map | Thread Safe | Synchronization Method | Performance |
|-----|-------------|----------------------|-------------|
| HashMap | ❌ | None | Fastest |
| LinkedHashMap | ❌ | None | Fast |
| TreeMap | ❌ | None | O(log n) |
| ConcurrentHashMap | ✅ | Lock-free/CAS | High concurrent performance |
| Hashtable | ✅ | Method-level synchronization | Lower performance |
| Collections.synchronizedMap() | ✅ | Block-level synchronization | Moderate performance |

This comprehensive guide covers all aspects of Java Maps, their internal workings, and best practices for choosing and using them effectively.
