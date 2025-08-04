# Java Cursors - Complete Guide

## Table of Contents
1. [Overview](#overview)
2. [Cursor Hierarchy](#cursor-hierarchy)
3. [Enumeration Interface](#enumeration-interface)
4. [Iterator Interface](#iterator-interface)
5. [ListIterator Interface](#listiterator-interface)
6. [Spliterator Interface](#spliterator-interface)
7. [Performance Comparison](#performance-comparison)
8. [When to Use What](#when-to-use-what)
9. [Best Practices](#best-practices)
10. [Common Pitfalls](#common-pitfalls)

## Overview

A **Cursor** in Java is an interface that allows traversal of collections. Cursors provide a way to iterate through collections one element at a time. Java provides several types of cursors, each with different capabilities and use cases.

**Key Benefits of Cursors:**
- Safe iteration through collections
- Ability to remove elements during iteration
- Support for both forward and backward traversal (in some cases)
- Lazy evaluation and memory efficiency

## Cursor Hierarchy

```
Cursors in Java (Not actual inheritance hierarchy)

1. Enumeration<E> (Legacy - Java 1.0)
   - hasMoreElements()
   - nextElement()

2. Iterator<E> (Java 1.2)
   - hasNext()
   - next()
   - remove()
   - forEachRemaining() (Java 8)

3. ListIterator<E> extends Iterator<E> (Java 1.2)
   - All Iterator methods +
   - hasPrevious()
   - previous()
   - nextIndex()
   - previousIndex()
   - set()
   - add()

4. Spliterator<E> (Java 8)
   - tryAdvance()
   - forEachRemaining()
   - trySplit()
   - estimateSize()
   - characteristics()
```

## Enumeration Interface

**Package:** `java.util.Enumeration<E>`  
**Since:** Java 1.0  
**Thread-Safe:** Depends on underlying collection  
**Fail-Fast:** No  

The **Enumeration** interface is a legacy cursor introduced in Java 1.0. It provides basic iteration capabilities but lacks modern features.

### Methods

```java
public interface Enumeration<E> {
    boolean hasMoreElements();  // Check if more elements exist
    E nextElement();           // Get next element
}
```

### Example Usage

```java
import java.util.*;

public class EnumerationExample {
    public static void main(String[] args) {
        // Enumeration is mainly used with legacy classes
        Vector<String> vector = new Vector<>();
        vector.add("Apple");
        vector.add("Banana");
        vector.add("Cherry");
        
        // Get enumeration from Vector
        Enumeration<String> enumeration = vector.elements();
        
        // Iterate using enumeration
        while (enumeration.hasMoreElements()) {
            String element = enumeration.nextElement();
            System.out.println(element);
        }
        
        // Hashtable enumeration
        Hashtable<String, Integer> hashtable = new Hashtable<>();
        hashtable.put("One", 1);
        hashtable.put("Two", 2);
        hashtable.put("Three", 3);
        
        // Enumerate keys
        Enumeration<String> keys = hashtable.keys();
        while (keys.hasMoreElements()) {
            String key = keys.nextElement();
            System.out.println(key + " = " + hashtable.get(key));
        }
        
        // Enumerate values
        Enumeration<Integer> values = hashtable.elements();
        while (values.hasMoreElements()) {
            Integer value = values.nextElement();
            System.out.println("Value: " + value);
        }
    }
}
```

### Characteristics
- **Read-only**: Cannot modify collection during iteration
- **Unidirectional**: Only forward traversal
- **Legacy**: Mainly used with Vector and Hashtable
- **Not fail-fast**: Doesn't detect concurrent modifications
- **Thread-safe**: Depends on underlying collection

### Collections Supporting Enumeration
- `Vector.elements()`
- `Hashtable.keys()`
- `Hashtable.elements()`
- `StringTokenizer` (implements Enumeration)

## Iterator Interface

**Package:** `java.util.Iterator<E>`  
**Since:** Java 1.2  
**Thread-Safe:** No  
**Fail-Fast:** Yes  

The **Iterator** interface is the standard cursor for most Java collections. It replaced Enumeration with additional functionality.

### Methods

```java
public interface Iterator<E> {
    boolean hasNext();                    // Check if more elements exist
    E next();                            // Get next element
    default void remove();               // Remove current element (optional)
    default void forEachRemaining(Consumer<? super E> action); // Java 8
}
```

### Example Usage

```java
import java.util.*;

public class IteratorExample {
    public static void main(String[] args) {
        // Basic iteration
        List<String> list = new ArrayList<>();
        list.add("Apple");
        list.add("Banana");
        list.add("Cherry");
        list.add("Date");
        
        Iterator<String> iterator = list.iterator();
        
        // Standard iteration
        while (iterator.hasNext()) {
            String element = iterator.next();
            System.out.println(element);
            
            // Safe removal during iteration
            if (element.equals("Banana")) {
                iterator.remove(); // Removes "Banana"
            }
        }
        
        System.out.println("After removal: " + list);
        
        // Using forEachRemaining (Java 8+)
        Set<Integer> set = new HashSet<>();
        set.addAll(Arrays.asList(1, 2, 3, 4, 5));
        
        Iterator<Integer> setIterator = set.iterator();
        
        // Process first element manually
        if (setIterator.hasNext()) {
            System.out.println("First: " + setIterator.next());
        }
        
        // Process remaining elements
        setIterator.forEachRemaining(element -> 
            System.out.println("Remaining: " + element)
        );
        
        // Iterator with Map
        Map<String, Integer> map = new HashMap<>();
        map.put("One", 1);
        map.put("Two", 2);
        map.put("Three", 3);
        
        // Iterate over keys
        Iterator<String> keyIterator = map.keySet().iterator();
        while (keyIterator.hasNext()) {
            String key = keyIterator.next();
            System.out.println(key + " = " + map.get(key));
        }
        
        // Iterate over entries
        Iterator<Map.Entry<String, Integer>> entryIterator = 
            map.entrySet().iterator();
        while (entryIterator.hasNext()) {
            Map.Entry<String, Integer> entry = entryIterator.next();
            System.out.println(entry.getKey() + " -> " + entry.getValue());
        }
    }
}
```

### Fail-Fast Behavior

```java
import java.util.*;
import java.util.concurrent.ConcurrentModificationException;

public class FailFastExample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.addAll(Arrays.asList("A", "B", "C", "D"));
        
        Iterator<String> iterator = list.iterator();
        
        try {
            while (iterator.hasNext()) {
                String element = iterator.next();
                System.out.println(element);
                
                // This will cause ConcurrentModificationException
                if (element.equals("B")) {
                    list.add("E"); // Direct modification
                }
            }
        } catch (ConcurrentModificationException e) {
            System.out.println("ConcurrentModificationException caught!");
        }
        
        // Correct way - use iterator.remove()
        Iterator<String> safeIterator = list.iterator();
        while (safeIterator.hasNext()) {
            String element = safeIterator.next();
            if (element.equals("C")) {
                safeIterator.remove(); // Safe removal
            }
        }
    }
}
```

### Characteristics
- **Fail-fast**: Throws ConcurrentModificationException on concurrent modification
- **Unidirectional**: Only forward traversal
- **Remove capability**: Can safely remove elements during iteration
- **Universal**: Works with all Collection implementations
- **Not thread-safe**: Requires external synchronization

## ListIterator Interface

**Package:** `java.util.ListIterator<E>`  
**Since:** Java 1.2  
**Thread-Safe:** No  
**Fail-Fast:** Yes  
**Extends:** Iterator<E>

The **ListIterator** interface extends Iterator and provides bidirectional traversal capabilities. It's specifically designed for List implementations.

### Methods

```java
public interface ListIterator<E> extends Iterator<E> {
    // Iterator methods
    boolean hasNext();
    E next();
    void remove();
    
    // Additional ListIterator methods
    boolean hasPrevious();        // Check if previous element exists
    E previous();                // Get previous element
    int nextIndex();             // Get next element index
    int previousIndex();         // Get previous element index
    void set(E e);               // Replace current element
    void add(E e);               // Add element at current position
}
```

### Example Usage

```java
import java.util.*;

public class ListIteratorExample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.addAll(Arrays.asList("Apple", "Banana", "Cherry", "Date"));
        
        // Get ListIterator
        ListIterator<String> listIterator = list.listIterator();
        
        System.out.println("=== Forward Iteration ===");
        while (listIterator.hasNext()) {
            int index = listIterator.nextIndex();
            String element = listIterator.next();
            System.out.println("Index " + index + ": " + element);
            
            // Modify element
            if (element.equals("Banana")) {
                listIterator.set("Blueberry"); // Replace current element
            }
            
            // Add element after current
            if (element.equals("Cherry")) {
                listIterator.add("Coconut"); // Add after current
            }
        }
        
        System.out.println("List after forward iteration: " + list);
        
        System.out.println("\n=== Backward Iteration ===");
        while (listIterator.hasPrevious()) {
            int index = listIterator.previousIndex();
            String element = listIterator.previous();
            System.out.println("Index " + index + ": " + element);
        }
        
        // Start from specific position
        System.out.println("\n=== Starting from specific position ===");
        ListIterator<String> positionIterator = list.listIterator(2);
        System.out.println("Starting from index 2:");
        while (positionIterator.hasNext()) {
            String element = positionIterator.next();
            System.out.println(element);
        }
        
        // Bidirectional traversal example
        System.out.println("\n=== Bidirectional Traversal ===");
        ListIterator<String> biIterator = list.listIterator();
        
        // Move forward 2 steps
        for (int i = 0; i < 2 && biIterator.hasNext(); i++) {
            System.out.println("Forward: " + biIterator.next());
        }
        
        // Move backward 1 step
        if (biIterator.hasPrevious()) {
            System.out.println("Backward: " + biIterator.previous());
        }
        
        // Move forward again
        if (biIterator.hasNext()) {
            System.out.println("Forward again: " + biIterator.next());
        }
    }
}
```

### Practical Example: Replacing Elements

```java
import java.util.*;

public class ListIteratorReplacement {
    public static void main(String[] args) {
        List<Integer> numbers = new ArrayList<>();
        numbers.addAll(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
        
        ListIterator<Integer> iterator = numbers.listIterator();
        
        // Replace even numbers with their squares
        while (iterator.hasNext()) {
            Integer number = iterator.next();
            if (number % 2 == 0) {
                iterator.set(number * number); // Replace with square
            }
        }
        
        System.out.println("After squaring even numbers: " + numbers);
        
        // Insert elements at specific positions
        ListIterator<Integer> insertIterator = numbers.listIterator();
        while (insertIterator.hasNext()) {
            Integer current = insertIterator.next();
            if (current > 10) { // For squared numbers
                insertIterator.add(-1); // Insert -1 after large numbers
            }
        }
        
        System.out.println("After inserting -1: " + numbers);
    }
}
```

### Characteristics
- **Bidirectional**: Forward and backward traversal
- **Index access**: Provides current position indices
- **Modification**: Can add, remove, and replace elements
- **List-specific**: Only available for List implementations
- **Fail-fast**: Detects concurrent modifications

## Spliterator Interface

**Package:** `java.util.Spliterator<T>`  
**Since:** Java 8  
**Thread-Safe:** Depends on implementation  
**Fail-Fast:** Depends on implementation  

The **Spliterator** (splittable iterator) interface is designed for parallel processing and Stream API support.

### Methods

```java
public interface Spliterator<T> {
    boolean tryAdvance(Consumer<? super T> action);  // Process one element
    void forEachRemaining(Consumer<? super T> action); // Process remaining
    Spliterator<T> trySplit();                       // Split for parallel processing
    long estimateSize();                             // Estimate remaining elements
    int characteristics();                           // Get characteristics flags
    
    // Default methods
    default long getExactSizeIfKnown();
    default boolean hasCharacteristics(int characteristics);
    default Comparator<? super T> getComparator();
}
```

### Characteristics Constants

```java
public interface Spliterator<T> {
    int ORDERED     = 0x00000010; // Elements have defined order
    int DISTINCT    = 0x00000001; // No duplicate elements
    int SORTED      = 0x00000004; // Elements are sorted
    int SIZED       = 0x00000040; // Size is known and finite
    int NONNULL     = 0x00000100; // No null elements
    int IMMUTABLE   = 0x00000400; // Elements cannot be modified
    int CONCURRENT  = 0x00001000; // Safe for concurrent access
    int SUBSIZED    = 0x00004000; // Split spliterators are SIZED
}
```

### Example Usage

```java
import java.util.*;
import java.util.function.Consumer;

public class SpliteratorExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("Apple", "Banana", "Cherry", "Date", "Elderberry");
        
        // Basic Spliterator usage
        System.out.println("=== Basic Spliterator Usage ===");
        Spliterator<String> spliterator = list.spliterator();
        
        // Process elements one by one
        Consumer<String> action = element -> System.out.println("Processing: " + element);
        
        // Process first element
        boolean hasMore = spliterator.tryAdvance(action);
        System.out.println("Has more elements: " + hasMore);
        
        // Process remaining elements
        spliterator.forEachRemaining(action);
        
        // Characteristics example
        System.out.println("\n=== Spliterator Characteristics ===");
        Spliterator<String> charSpliterator = list.spliterator();
        
        System.out.println("Estimated size: " + charSpliterator.estimateSize());
        System.out.println("Exact size: " + charSpliterator.getExactSizeIfKnown());
        
        int characteristics = charSpliterator.characteristics();
        System.out.println("Is ORDERED: " + charSpliterator.hasCharacteristics(Spliterator.ORDERED));
        System.out.println("Is SIZED: " + charSpliterator.hasCharacteristics(Spliterator.SIZED));
        System.out.println("Is DISTINCT: " + charSpliterator.hasCharacteristics(Spliterator.DISTINCT));
        System.out.println("Is SORTED: " + charSpliterator.hasCharacteristics(Spliterator.SORTED));
        
        // Splitting example
        System.out.println("\n=== Spliterator Splitting ===");
        demonstrateSplitting();
        
        // Custom Spliterator
        System.out.println("\n=== Custom Spliterator ===");
        demonstrateCustomSpliterator();
    }
    
    private static void demonstrateSplitting() {
        List<Integer> numbers = new ArrayList<>();
        for (int i = 1; i <= 100; i++) {
            numbers.add(i);
        }
        
        Spliterator<Integer> spliterator = numbers.spliterator();
        System.out.println("Original size: " + spliterator.estimateSize());
        
        // Try to split
        Spliterator<Integer> split1 = spliterator.trySplit();
        if (split1 != null) {
            System.out.println("Split1 size: " + split1.estimateSize());
            System.out.println("Remaining size: " + spliterator.estimateSize());
            
            // Try to split again
            Spliterator<Integer> split2 = spliterator.trySplit();
            if (split2 != null) {
                System.out.println("Split2 size: " + split2.estimateSize());
                System.out.println("Final remaining size: " + spliterator.estimateSize());
            }
        }
    }
    
    private static void demonstrateCustomSpliterator() {
        // Custom Spliterator for range of numbers
        RangeSpliterator rangeSpliterator = new RangeSpliterator(1, 10);
        
        rangeSpliterator.forEachRemaining(num -> System.out.print(num + " "));
        System.out.println();
    }
    
    // Custom Spliterator implementation
    static class RangeSpliterator implements Spliterator<Integer> {
        private int current;
        private final int end;
        
        public RangeSpliterator(int start, int end) {
            this.current = start;
            this.end = end;
        }
        
        @Override
        public boolean tryAdvance(Consumer<? super Integer> action) {
            if (current < end) {
                action.accept(current++);
                return true;
            }
            return false;
        }
        
        @Override
        public Spliterator<Integer> trySplit() {
            int remaining = end - current;
            if (remaining < 2) {
                return null; // Too small to split
            }
            
            int splitPoint = current + remaining / 2;
            RangeSpliterator split = new RangeSpliterator(current, splitPoint);
            current = splitPoint;
            return split;
        }
        
        @Override
        public long estimateSize() {
            return end - current;
        }
        
        @Override
        public int characteristics() {
            return ORDERED | SIZED | SUBSIZED | IMMUTABLE | NONNULL | DISTINCT;
        }
    }
}
```

### Spliterator with Streams

```java
import java.util.*;
import java.util.stream.Stream;
import java.util.stream.StreamSupport;

public class SpliteratorStreamExample {
    public static void main(String[] args) {
        // Create stream from custom Spliterator
        Spliterator<Integer> spliterator = new RangeSpliterator(1, 100);
        Stream<Integer> stream = StreamSupport.stream(spliterator, true); // parallel=true
        
        // Use parallel processing
        long sum = stream.parallel()
                        .filter(n -> n % 2 == 0)
                        .mapToLong(Integer::longValue)
                        .sum();
                        
        System.out.println("Sum of even numbers 1-99: " + sum);
        
        // Sequential processing
        Spliterator<Integer> seqSpliterator = new RangeSpliterator(1, 10);
        StreamSupport.stream(seqSpliterator, false)
                    .forEach(System.out::println);
    }
    
    static class RangeSpliterator implements Spliterator<Integer> {
        private int current;
        private final int end;
        
        public RangeSpliterator(int start, int end) {
            this.current = start;
            this.end = end;
        }
        
        @Override
        public boolean tryAdvance(java.util.function.Consumer<? super Integer> action) {
            if (current < end) {
                action.accept(current++);
                return true;
            }
            return false;
        }
        
        @Override
        public Spliterator<Integer> trySplit() {
            int remaining = end - current;
            if (remaining < 2) return null;
            
            int splitPoint = current + remaining / 2;
            RangeSpliterator split = new RangeSpliterator(current, splitPoint);
            current = splitPoint;
            return split;
        }
        
        @Override
        public long estimateSize() {
            return end - current;
        }
        
        @Override
        public int characteristics() {
            return ORDERED | SIZED | SUBSIZED | IMMUTABLE | NONNULL | DISTINCT;
        }
    }
}
```

### Characteristics
- **Parallel processing**: Designed for splitting and parallel execution
- **Stream integration**: Powers the Stream API
- **Lazy evaluation**: Elements processed on demand
- **Characteristics**: Provides metadata about the iteration
- **Splittable**: Can be divided for parallel processing

## Performance Comparison

| Feature | Enumeration | Iterator | ListIterator | Spliterator |
|---------|-------------|----------|--------------|-------------|
| **Direction** | Forward only | Forward only | Bidirectional | Forward only |
| **Modification** | No | Remove only | Add/Remove/Set | No |
| **Fail-Fast** | No | Yes | Yes | Depends |
| **Thread-Safe** | Depends | No | No | Depends |
| **Parallel** | No | No | No | Yes |
| **Memory** | Low | Low | Low | Variable |
| **Performance** | Fast | Fast | Fast | Optimized for parallel |

## When to Use What

### Use Enumeration when:
- Working with legacy code (Vector, Hashtable)
- Simple read-only iteration needed
- Compatibility with older Java versions required
- Thread safety is handled by underlying collection

### Use Iterator when:
- General-purpose iteration needed
- Need to remove elements during iteration
- Working with any Collection implementation
- Want fail-fast behavior for debugging

### Use ListIterator when:
- Working specifically with List implementations
- Need bidirectional traversal
- Need to modify elements during iteration (add/remove/set)
- Need index information during iteration
- Implementing algorithms that require backward movement

### Use Spliterator when:
- Building custom Stream sources
- Need parallel processing capabilities
- Working with very large datasets
- Implementing custom collection types
- Need lazy evaluation with characteristics metadata

## Best Practices

### 1. Choose the Right Cursor
```java
// Good: Use appropriate cursor for the task
List<String> list = new ArrayList<>();
ListIterator<String> listIter = list.listIterator(); // For List operations

Set<String> set = new HashSet<>();
Iterator<String> iter = set.iterator(); // For general collections

// Avoid: Using wrong cursor type
// Iterator<String> wrongChoice = list.listIterator(); // Loses functionality
```

### 2. Handle Exceptions Properly
```java
try {
    Iterator<String> iter = collection.iterator();
    while (iter.hasNext()) {
        String element = iter.next();
        // Process element
    }
} catch (ConcurrentModificationException e) {
    // Handle concurrent modification
    System.err.println("Collection was modified during iteration");
}
```

### 3. Use Enhanced For-Loop When Appropriate
```java
// Good: Simple iteration without modification
for (String element : collection) {
    System.out.println(element);
}

// Good: When you need to remove elements
Iterator<String> iter = collection.iterator();
while (iter.hasNext()) {
    String element = iter.next();
    if (shouldRemove(element)) {
        iter.remove();
    }
}
```

### 4. Prefer Iterator over Index-based Iteration
```java
// Good: Works with all Collection types
Iterator<String> iter = collection.iterator();
while (iter.hasNext()) {
    process(iter.next());
}

// Avoid: Only works with Lists, less efficient for LinkedList
for (int i = 0; i < list.size(); i++) {
    process(list.get(i)); // O(n) for LinkedList
}
```

### 5. Use forEachRemaining for Functional Style
```java
// Java 8+ functional approach
Iterator<String> iter = collection.iterator();
iter.forEachRemaining(element -> {
    // Process element
    System.out.println(element.toUpperCase());
});
```

## Common Pitfalls

### 1. ConcurrentModificationException
```java
// Wrong: Direct modification during iteration
List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
for (String element : list) {
    if (element.equals("B")) {
        list.remove(element); // Throws ConcurrentModificationException
    }
}

// Correct: Use iterator for removal
Iterator<String> iter = list.iterator();
while (iter.hasNext()) {
    String element = iter.next();
    if (element.equals("B")) {
        iter.remove(); // Safe removal
    }
}
```

### 2. NoSuchElementException
```java
// Wrong: Not checking hasNext()
Iterator<String> iter = collection.iterator();
String first = iter.next(); // May throw NoSuchElementException
String second = iter.next(); // May throw NoSuchElementException

// Correct: Always check hasNext()
if (iter.hasNext()) {
    String first = iter.next();
}
if (iter.hasNext()) {
    String second = iter.next();
}
```

### 3. IllegalStateException with remove()
```java
// Wrong: Calling remove() without next()
Iterator<String> iter = collection.iterator();
iter.remove(); // IllegalStateException - no current element

// Wrong: Calling remove() twice
iter.next();
iter.remove(); // OK
iter.remove(); // IllegalStateException - element already removed

// Correct: Call next() before each remove()
while (iter.hasNext()) {
    String element = iter.next();
    if (shouldRemove(element)) {
        iter.remove(); // OK - removes the element returned by next()
    }
}
```

### 4. Modifying During Enhanced For-Loop
```java
// Wrong: Cannot modify collection in enhanced for-loop
List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
for (String element : list) {
    if (element.equals("B")) {
        list.add("D"); // ConcurrentModificationException
    }
}

// Correct: Use traditional iterator
Iterator<String> iter = list.iterator();
while (iter.hasNext()) {
    String element = iter.next();
    // Can safely use iter.remove() here
}
```

### 5. Thread Safety Issues
```java
// Wrong: Sharing iterator across threads
Iterator<String> sharedIter = collection.iterator();
// Thread 1 and Thread 2 both use sharedIter - race conditions

// Correct: Each thread gets its own iterator
// Thread 1
Iterator<String> iter1 = collection.iterator();

// Thread 2  
Iterator<String> iter2 = collection.iterator();

// Or use thread-safe collections
ConcurrentLinkedQueue<String> threadSafeQueue = new ConcurrentLinkedQueue<>();
```

## Summary

Java provides multiple cursor interfaces, each designed for specific use cases:

- **Enumeration**: Legacy, simple, read-only iteration
- **Iterator**: Standard, fail-fast, supports removal
- **ListIterator**: Bidirectional, full modification support for Lists
- **Spliterator**: Modern, parallel-processing capable, Stream integration

Choose the appropriate cursor based on your specific requirements for traversal direction, modification needs, thread safety, and performance considerations.
