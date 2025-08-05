# 🗺️ Java Map Interface - Complete Guide

**Date:** 2025-08-05

## What is a Map in Java?

A **Map** is an object that maps **keys to values**. It cannot contain duplicate keys, and each key maps to at most one value. Maps are part of the Java Collections Framework and provide a way to store and retrieve data based on key-value pairs.

Key characteristics:
- No duplicate keys allowed
- Each key maps to exactly one value
- Values can be duplicated
- Keys and values can be null (depends on implementation)

---

## 🔢 Types of Maps in Java

### 1. HashMap

**Characteristics:**
- **Order**: No guaranteed order
- **Null**: Allows one `null` key and multiple `null` values
- **Thread Safe**: ❌ Not synchronized
- **Performance**: O(1) average time for get/put operations
- **Load Factor**: Default 0.75
- **Initial Capacity**: Default 16

**Use Cases:** General-purpose mapping, caching, configuration storage

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
        System.out.println("Apples: " + fruitCount.get("apple")); // Output: 5
        
        // Iterating
        for (Map.Entry<String, Integer> entry : fruitCount.entrySet()) {
            System.out.println(entry.getKey() + " = " + entry.getValue());
        }
        
        // Check if key exists
        if (fruitCount.containsKey("apple")) {
            System.out.println("Apple count: " + fruitCount.get("apple"));
        }
    }
}
```

---

### 2. LinkedHashMap

**Characteristics:**
- **Order**: Maintains insertion order or access order
- **Null**: Allows null keys and values
- **Thread Safe**: ❌ Not synchronized
- **Performance**: Slightly slower than HashMap due to maintaining order
- **Special Feature**: Can be configured for LRU (Least Recently Used) cache

**Use Cases:** LRU cache implementation, maintaining insertion order, ordered configurations

```java
import java.util.*;

public class LinkedHashMapExample {
    public static void main(String[] args) {
        // Insertion order LinkedHashMap
        Map<String, String> insertionOrder = new LinkedHashMap<>();
        insertionOrder.put("first", "A");
        insertionOrder.put("second", "B");
        insertionOrder.put("third", "C");
        
        System.out.println("Insertion Order: " + insertionOrder);
        // Output: {first=A, second=B, third=C}
        
        // Access order LinkedHashMap (LRU Cache)
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
        lruCache.put("D", "Value D"); // B gets removed (least recently used)
        
        System.out.println("LRU Cache: " + lruCache);
        // Output: {C=Value C, A=Value A, D=Value D}
    }
}
```

---

### 3. TreeMap

**Characteristics:**
- **Order**: Sorted by natural order or custom comparator
- **Null**: Does not allow `null` keys (but allows null values)
- **Thread Safe**: ❌ Not synchronized
- **Performance**: O(log n) for get/put operations
- **Implementation**: Red-Black tree (balanced BST)

**Use Cases:** Sorted dictionaries, range queries, navigation operations

```java
import java.util.*;

public class TreeMapExample {
    public static void main(String[] args) {
        // Natural ordering (ascending)
        TreeMap<Integer, String> naturalOrder = new TreeMap<>();
        naturalOrder.put(3, "Three");
        naturalOrder.put(1, "One");
        naturalOrder.put(4, "Four");
        naturalOrder.put(2, "Two");
        
        System.out.println("Natural Order: " + naturalOrder);
        // Output: {1=One, 2=Two, 3=Three, 4=Four}
        
        // Custom comparator (descending)
        TreeMap<String, Integer> reverseOrder = new TreeMap<>(Collections.reverseOrder());
        reverseOrder.put("apple", 5);
        reverseOrder.put("banana", 3);
        reverseOrder.put("cherry", 8);
        
        System.out.println("Reverse Order: " + reverseOrder);
        // Output: {cherry=8, banana=3, apple=5}
        
        // Navigation methods
        System.out.println("First key: " + naturalOrder.firstKey()); // 1
        System.out.println("Last key: " + naturalOrder.lastKey());   // 4
        System.out.println("Lower than 3: " + naturalOrder.lowerKey(3)); // 2
        System.out.println("Higher than 2: " + naturalOrder.higherKey(2)); // 3
        
        // SubMap operations
        SortedMap<Integer, String> subMap = naturalOrder.subMap(2, 4);
        System.out.println("SubMap [2,4): " + subMap); // {2=Two, 3=Three}
    }
}
```

---

### 4. ConcurrentHashMap

**Characteristics:**
- **Thread Safe**: ✅ Highly concurrent and lock-free for reads
- **Null**: Does not allow `null` keys or values
- **Performance**: Better than synchronized HashMap in concurrent scenarios
- **Segmentation**: Uses segment-based locking (Java 7) or CAS operations (Java 8+)

**Use Cases:** Multi-threaded applications, thread-safe caches, concurrent data access

```java
import java.util.concurrent.*;
import java.util.*;

public class ConcurrentHashMapExample {
    public static void main(String[] args) throws InterruptedException {
        ConcurrentHashMap<String, Integer> concurrentMap = new ConcurrentHashMap<>();
        
        // Thread-safe operations
        concurrentMap.put("counter", 0);
        
        // Simulate concurrent access
        ExecutorService executor = Executors.newFixedThreadPool(10);
        
        for (int i = 0; i < 100; i++) {
            executor.submit(() -> {
                // Atomic increment operation
                concurrentMap.compute("counter", (key, val) -> val == null ? 1 : val + 1);
            });
        }
        
        executor.shutdown();
        executor.awaitTermination(1, TimeUnit.SECONDS);
        
        System.out.println("Final counter value: " + concurrentMap.get("counter"));
        // Output: Final counter value: 100
        
        // Concurrent iteration (doesn't throw ConcurrentModificationException)
        concurrentMap.put("item1", 1);
        concurrentMap.put("item2", 2);
        
        for (Map.Entry<String, Integer> entry : concurrentMap.entrySet()) {
            System.out.println(entry.getKey() + " = " + entry.getValue());
            // Safe to modify map during iteration
            if ("item1".equals(entry.getKey())) {
                concurrentMap.put("item3", 3);
            }
        }
    }
}
```

---

### 5. Hashtable

**Characteristics:**
- **Thread Safe**: ✅ All methods are synchronized (legacy approach)
- **Null**: Does not allow `null` keys or values
- **Performance**: Slower than HashMap due to synchronization overhead
- **Legacy**: Part of Java since JDK 1.0

**Use Cases:** Legacy codebases, simple thread-safe requirements (prefer ConcurrentHashMap)

```java
import java.util.*;

public class HashtableExample {
    public static void main(String[] args) {
        Hashtable<String, String> hashtable = new Hashtable<>();
        
        hashtable.put("key1", "value1");
        hashtable.put("key2", "value2");
        // hashtable.put(null, "value"); // Throws NullPointerException
        // hashtable.put("key", null);   // Throws NullPointerException
        
        // Thread-safe enumeration
        Enumeration<String> keys = hashtable.keys();
        while (keys.hasMoreElements()) {
            String key = keys.nextElement();
            System.out.println(key + " = " + hashtable.get(key));
        }
        
        // Properties (extends Hashtable)
        Properties props = new Properties();
        props.setProperty("database.url", "jdbc:mysql://localhost/mydb");
        props.setProperty("database.user", "admin");
        System.out.println("Database URL: " + props.getProperty("database.url"));
    }
}
```

---

### 6. WeakHashMap

**Characteristics:**
- **Key Storage**: Uses weak references for keys
- **GC Behavior**: Entries are automatically removed when keys are garbage collected
- **Null**: Allows null keys and values
- **Memory**: Helps prevent memory leaks in certain scenarios

**Use Cases:** Memory-sensitive caches, canonical mappings, avoiding memory leaks

```java
import java.util.*;

public class WeakHashMapExample {
    public static void main(String[] args) {
        WeakHashMap<Object, String> weakMap = new WeakHashMap<>();
        
        Object key1 = new String("key1"); // Create objects that can be GC'd
        Object key2 = new String("key2");
        
        weakMap.put(key1, "value1");
        weakMap.put(key2, "value2");
        
        System.out.println("Before GC: " + weakMap.size()); // 2
        
        key1 = null; // Remove strong reference
        System.gc(); // Suggest garbage collection
        
        // Give GC time to work
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        
        System.out.println("After GC: " + weakMap.size()); // Likely 1
        System.out.println("Remaining entries: " + weakMap);
        
        // Canonical mapping example
        WeakHashMap<String, String> canonicalMap = new WeakHashMap<>();
        String intern1 = new String("hello").intern();
        String intern2 = new String("hello").intern();
        
        canonicalMap.put(intern1, "first");
        System.out.println("Same reference: " + (intern1 == intern2)); // true
    }
}
```

---

### 7. IdentityHashMap

**Characteristics:**
- **Comparison**: Uses `==` (reference equality) instead of `.equals()`
- **Null**: Allows null keys and values
- **Hash Function**: Uses `System.identityHashCode()`
- **Performance**: Linear probing for collision resolution

**Use Cases:** Serialization frameworks, object identity tracking, graph algorithms

```java
import java.util.*;

public class IdentityHashMapExample {
    public static void main(String[] args) {
        IdentityHashMap<String, String> identityMap = new IdentityHashMap<>();
        
        String key1 = new String("hello");
        String key2 = new String("hello");
        String key3 = "hello"; // String literal
        
        identityMap.put(key1, "value1");
        identityMap.put(key2, "value2"); // Different object, even though content is same
        identityMap.put(key3, "value3");
        
        System.out.println("Size: " + identityMap.size()); // 3 (or 2 if key3 is same reference as key1/key2)
        System.out.println("key1 == key2: " + (key1 == key2)); // false
        System.out.println("key1.equals(key2): " + key1.equals(key2)); // true
        
        // Object identity tracking example
        List<Object> objects = Arrays.asList(new Object(), new Object(), new Object());
        IdentityHashMap<Object, Integer> objectIds = new IdentityHashMap<>();
        
        int id = 1;
        for (Object obj : objects) {
            objectIds.put(obj, id++);
        }
        
        for (Map.Entry<Object, Integer> entry : objectIds.entrySet()) {
            System.out.println("Object " + entry.getValue() + 
                             " hash: " + System.identityHashCode(entry.getKey()));
        }
    }
}
```

---

### 8. EnumMap

**Characteristics:**
- **Key Type**: Only enum keys allowed
- **Performance**: Very fast, internally uses array
- **Order**: Maintains natural enum order
- **Null**: Does not allow null keys, allows null values

**Use Cases:** Efficient mapping for enums, state machines, configuration by enum

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
        
        System.out.println("Schedule:");
        for (Map.Entry<Day, String> entry : schedule.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
        
        // Task priority mapping
        EnumMap<Priority, List<String>> tasksByPriority = new EnumMap<>(Priority.class);
        tasksByPriority.put(Priority.HIGH, Arrays.asList("Fix critical bug", "Deploy hotfix"));
        tasksByPriority.put(Priority.MEDIUM, Arrays.asList("Code review", "Update docs"));
        tasksByPriority.put(Priority.LOW, Arrays.asList("Refactor code", "Clean up"));
        
        System.out.println("\nTasks by Priority:");
        for (Map.Entry<Priority, List<String>> entry : tasksByPriority.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
        
        // State machine example
        EnumMap<Priority, Integer> priorityCounts = new EnumMap<>(Priority.class);
        for (Priority p : Priority.values()) {
            priorityCounts.put(p, 0);
        }
        
        // Increment counts
        priorityCounts.compute(Priority.HIGH, (k, v) -> v + 1);
        priorityCounts.compute(Priority.MEDIUM, (k, v) -> v + 2);
        
        System.out.println("\nPriority Counts: " + priorityCounts);
    }
}
```

---

### 9. NavigableMap Interface

**Characteristics:**
- **Extends**: SortedMap interface
- **Navigation Methods**: `lowerKey()`, `floorKey()`, `ceilingKey()`, `higherKey()`
- **Implemented By**: TreeMap
- **Reverse Navigation**: Supports reverse iteration

**Use Cases:** Range-based queries, navigation operations, time-series data

```java
import java.util.*;

public class NavigableMapExample {
    public static void main(String[] args) {
        NavigableMap<Integer, String> navMap = new TreeMap<>();
        
        navMap.put(1, "One");
        navMap.put(3, "Three");
        navMap.put(5, "Five");
        navMap.put(7, "Seven");
        navMap.put(9, "Nine");
        
        System.out.println("Original map: " + navMap);
        
        // Navigation methods
        System.out.println("Lower than 5: " + navMap.lowerKey(5));     // 3
        System.out.println("Floor of 4: " + navMap.floorKey(4));       // 3
        System.out.println("Ceiling of 4: " + navMap.ceilingKey(4));   // 5
        System.out.println("Higher than 5: " + navMap.higherKey(5));   // 7
        
        // First and last
        System.out.println("First entry: " + navMap.firstEntry());     // 1=One
        System.out.println("Last entry: " + navMap.lastEntry());       // 9=Nine
        
        // Poll (remove and return)
        System.out.println("Poll first: " + navMap.pollFirstEntry());  // 1=One
        System.out.println("Poll last: " + navMap.pollLastEntry());    // 9=Nine
        System.out.println("After polling: " + navMap);
        
        // Sub maps
        NavigableMap<Integer, String> subMap = navMap.subMap(3, true, 7, true);
        System.out.println("SubMap [3,7]: " + subMap); // {3=Three, 5=Five, 7=Seven}
        
        // Reverse navigation
        NavigableMap<Integer, String> reverseMap = navMap.descendingMap();
        System.out.println("Reverse map: " + reverseMap);
    }
}
```

---

### 10. SortedMap Interface

**Characteristics:**
- **Order**: Maintains natural key order or custom comparator order
- **Range Views**: Provides subMap, headMap, tailMap methods
- **Implemented By**: TreeMap, ConcurrentSkipListMap
- **First/Last**: Access to first and last keys

**Use Cases:** Sorted views of data, range operations, ordered collections

```java
import java.util.*;

public class SortedMapExample {
    public static void main(String[] args) {
        SortedMap<String, Integer> sortedMap = new TreeMap<>();
        
        sortedMap.put("charlie", 3);
        sortedMap.put("alice", 1);
        sortedMap.put("bob", 2);
        sortedMap.put("david", 4);
        
        System.out.println("Sorted map: " + sortedMap);
        // Output: {alice=1, bob=2, charlie=3, david=4}
        
        // Range operations
        System.out.println("First key: " + sortedMap.firstKey());      // alice
        System.out.println("Last key: " + sortedMap.lastKey());        // david
        
        // Sub maps
        SortedMap<String, Integer> headMap = sortedMap.headMap("charlie");
        System.out.println("Head map (< charlie): " + headMap);        // {alice=1, bob=2}
        
        SortedMap<String, Integer> tailMap = sortedMap.tailMap("charlie");
        System.out.println("Tail map (>= charlie): " + tailMap);       // {charlie=3, david=4}
        
        SortedMap<String, Integer> subMap = sortedMap.subMap("alice", "david");
        System.out.println("Sub map [alice, david): " + subMap);       // {alice=1, bob=2, charlie=3}
        
        // Custom comparator
        SortedMap<String, Integer> lengthSorted = new TreeMap<>(
            Comparator.comparing(String::length).thenComparing(String::compareTo)
        );
        
        lengthSorted.put("a", 1);
        lengthSorted.put("bb", 2);
        lengthSorted.put("ccc", 3);
        lengthSorted.put("dd", 4);
        
        System.out.println("Length sorted: " + lengthSorted);
        // Output: {a=1, bb=2, dd=4, ccc=3}
    }
}
```

---

## 🔚 Comprehensive Comparison Table

| Map Type            | Order Maintained     | Sorted | Thread Safe | Null Keys | Null Values | Comparison     | Performance     | Use Case |
|---------------------|----------------------|--------|-------------|-----------|-------------|----------------|-----------------|----------|
| HashMap             | ❌                   | ❌     | ❌          | ✅        | ✅          | `.equals()`    | O(1) avg        | General purpose |
| LinkedHashMap       | ✅ (Insertion/Access) | ❌     | ❌          | ✅        | ✅          | `.equals()`    | O(1) avg        | LRU cache, ordered data |
| TreeMap             | ✅ (Sorted)           | ✅     | ❌          | ❌        | ✅          | `.compareTo()` | O(log n)        | Sorted data, range queries |
| ConcurrentHashMap   | ❌                   | ❌     | ✅          | ❌        | ❌          | `.equals()`    | O(1) avg        | Concurrent access |
| Hashtable           | ❌                   | ❌     | ✅          | ❌        | ❌          | `.equals()`    | O(1) avg        | Legacy thread-safe |
| WeakHashMap         | ❌                   | ❌     | ❌          | ✅        | ✅          | `.equals()`    | O(1) avg        | Memory-sensitive cache |
| IdentityHashMap     | ❌                   | ❌     | ❌          | ✅        | ✅          | `==`           | O(1) avg        | Object identity |
| EnumMap             | ✅ (Enum Order)       | ✅     | ❌          | ❌        | ✅          | Enum ordinal   | O(1)            | Enum-based mapping |

---

## 🎯 Best Practices and Tips

### 1. Choosing the Right Map
```java
// General purpose: HashMap
Map<String, Object> config = new HashMap<>();

// Need ordering: LinkedHashMap or TreeMap
Map<String, String> orderedConfig = new LinkedHashMap<>();
Map<Integer, String> sortedData = new TreeMap<>();

// Thread safety: ConcurrentHashMap
Map<String, Object> cache = new ConcurrentHashMap<>();

// Enum keys: EnumMap
Map<Status, String> statusMessages = new EnumMap<>(Status.class);
```

### 2. Performance Considerations
```java
// Pre-size maps when you know the approximate size
Map<String, String> map = new HashMap<>(1000); // Initial capacity

// Use appropriate load factor
Map<String, String> map = new HashMap<>(16, 0.9f); // Higher load factor for memory efficiency
```

### 3. Safe Iteration
```java
// Safe concurrent iteration with ConcurrentHashMap
ConcurrentHashMap<String, String> map = new ConcurrentHashMap<>();
for (Map.Entry<String, String> entry : map.entrySet()) {
    // Safe to modify map during iteration
    if (someCondition) {
        map.put("newKey", "newValue");
    }
}

// For other maps, use iterator for safe removal
Map<String, String> map = new HashMap<>();
Iterator<Map.Entry<String, String>> iterator = map.entrySet().iterator();
while (iterator.hasNext()) {
    Map.Entry<String, String> entry = iterator.next();
    if (entry.getKey().startsWith("remove")) {
        iterator.remove(); // Safe removal
    }
}
```

### 4. Common Patterns
```java
// Compute if absent pattern
map.computeIfAbsent(key, k -> new ArrayList<>()).add(value);

// Merge pattern for counting
map.merge(key, 1, Integer::sum);

// Replace pattern
map.replace(key, oldValue, newValue);
```

---

## 📚 Summary

Choose your Map implementation based on your specific requirements:

- **HashMap**: Default choice for general-purpose key-value mapping
- **LinkedHashMap**: When you need to maintain insertion order or implement LRU cache
- **TreeMap**: When you need sorted keys or range operations
- **ConcurrentHashMap**: For thread-safe operations without synchronization overhead
- **EnumMap**: When keys are enum values (most efficient)
- **WeakHashMap**: For memory-sensitive caches where keys can be garbage collected
- **IdentityHashMap**: When you need reference equality instead of object equality

Remember to consider thread safety, performance requirements, ordering needs, and null value handling when making your choice.
