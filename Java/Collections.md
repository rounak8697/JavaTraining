# Java Collections Framework - Complete Guide

## Table of Contents
1. [Overview](#overview)
2. [Collection Hierarchy](#collection-hierarchy)
3. [Core Interfaces](#core-interfaces)
4. [List Interface](#list-interface)
5. [Set Interface](#set-interface)
6. [Queue Interface](#queue-interface)
7. [Map Interface](#map-interface)
8. [Iterator Interfaces](#iterator-interfaces)
9. [Utility Classes](#utility-classes)
10. [Performance Comparison](#performance-comparison)
11. [When to Use What](#when-to-use-what)

## Overview

The Java Collections Framework is a unified architecture for representing and manipulating collections of objects. It provides:
- **Interfaces**: Abstract data types representing collections
- **Implementations**: Concrete implementations of collection interfaces
- **Algorithms**: Methods that perform useful computations on collections

## Collection Hierarchy

```
                    Iterable<E>
                        |
                   Collection<E>
                   /     |     \
                List<E> Set<E> Queue<E>
                  |       |       |
            ArrayList  HashSet  LinkedList
            LinkedList TreeSet  PriorityQueue
            Vector     LinkedHashSet  ArrayDeque
            Stack      EnumSet
                      
                    Map<E> (separate hierarchy)
                      |
                   HashMap
                   TreeMap
                   LinkedHashMap
                   Hashtable
                   EnumMap
```

## Core Interfaces

### Collection<E>
The root interface for all collections except Map.

**Key Methods:**
- `add(E e)` - Add element
- `remove(Object o)` - Remove element
- `contains(Object o)` - Check if contains element
- `size()` - Get size
- `isEmpty()` - Check if empty
- `clear()` - Remove all elements
- `iterator()` - Get iterator

## List Interface

Lists are **ordered collections** that allow **duplicate elements** and provide **indexed access**.

### ArrayList<E>
**Implementation:** Resizable array
**Thread-Safe:** No

```java
List<String> arrayList = new ArrayList<>();
arrayList.add("Apple");
arrayList.add("Banana");
arrayList.add("Apple"); // Duplicates allowed
arrayList.add(1, "Orange"); // Insert at index
System.out.println(arrayList.get(0)); // Access by index
```

**Characteristics:**
- Fast random access O(1)
- Slow insertion/deletion in middle O(n)
- Good for read-heavy operations
- Dynamic resizing

### LinkedList<E>
**Implementation:** Doubly-linked list
**Thread-Safe:** No

```java
List<String> linkedList = new LinkedList<>();
linkedList.add("First");
linkedList.addFirst("Zero"); // Add at beginning
linkedList.addLast("Last");  // Add at end
linkedList.remove(1); // Remove by index
```

**Characteristics:**
- Fast insertion/deletion O(1)
- Slow random access O(n)
- Implements both List and Deque
- Good for frequent insertions/deletions

### Vector<E>
**Implementation:** Synchronized resizable array
**Thread-Safe:** Yes

```java
List<String> vector = new Vector<>();
vector.add("Synchronized");
vector.add("Legacy");
// All methods are synchronized
```

**Characteristics:**
- Legacy class (Java 1.0)
- Synchronized methods
- Similar to ArrayList but thread-safe
- Performance overhead due to synchronization

### Stack<E>
**Implementation:** Extends Vector
**Thread-Safe:** Yes

```java
Stack<String> stack = new Stack<>();
stack.push("Bottom");
stack.push("Middle");
stack.push("Top");
String top = stack.pop(); // Returns "Top"
String peek = stack.peek(); // Returns "Middle" without removing
```

**Characteristics:**
- LIFO (Last In, First Out)
- Legacy class
- Extends Vector
- Use ArrayDeque instead for new code

## Set Interface

Sets are collections that **do not allow duplicate elements**.

### HashSet<E>
**Implementation:** Hash table
**Thread-Safe:** No
**Ordering:** No ordering guaranteed

```java
Set<String> hashSet = new HashSet<>();
hashSet.add("Apple");
hashSet.add("Banana");
hashSet.add("Apple"); // Duplicate ignored
System.out.println(hashSet.size()); // 2
```

**Characteristics:**
- O(1) average time for basic operations
- No ordering of elements
- Allows one null element
- Best performance for most operations

### LinkedHashSet<E>
**Implementation:** Hash table + linked list
**Thread-Safe:** No
**Ordering:** Insertion order maintained

```java
Set<String> linkedHashSet = new LinkedHashSet<>();
linkedHashSet.add("Third");
linkedHashSet.add("First");
linkedHashSet.add("Second");
// Iteration order: Third, First, Second
```

**Characteristics:**
- Maintains insertion order
- Slightly slower than HashSet
- Predictable iteration order
- Allows one null element

### TreeSet<E>
**Implementation:** Red-Black tree (balanced BST)
**Thread-Safe:** No
**Ordering:** Natural ordering or custom Comparator

```java
Set<String> treeSet = new TreeSet<>();
treeSet.add("Zebra");
treeSet.add("Apple");
treeSet.add("Banana");
// Iteration order: Apple, Banana, Zebra (sorted)

// Custom comparator
Set<String> customTreeSet = new TreeSet<>(String.CASE_INSENSITIVE_ORDER);
```

**Characteristics:**
- O(log n) for basic operations
- Elements stored in sorted order
- No null elements allowed
- Implements NavigableSet interface

### EnumSet<E>
**Implementation:** Bit vector
**Thread-Safe:** No
**Ordering:** Enum declaration order

```java
enum Day { MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY }

Set<Day> weekdays = EnumSet.range(Day.MONDAY, Day.FRIDAY);
Set<Day> weekend = EnumSet.of(Day.SATURDAY, Day.SUNDAY);
Set<Day> allDays = EnumSet.allOf(Day.class);
```

**Characteristics:**
- Extremely efficient for enum types
- Type-safe
- Compact representation
- Fast operations

## Queue Interface

Queues hold elements **prior to processing**, typically in **FIFO order**.

### LinkedList<E> (as Queue)
**Implementation:** Doubly-linked list
**Thread-Safe:** No

```java
Queue<String> queue = new LinkedList<>();
queue.offer("First");  // Add to rear
queue.offer("Second");
String head = queue.poll(); // Remove from front, returns "First"
String peek = queue.peek(); // Look at front without removing
```

### PriorityQueue<E>
**Implementation:** Binary heap
**Thread-Safe:** No
**Ordering:** Natural ordering or custom Comparator

```java
Queue<Integer> pq = new PriorityQueue<>();
pq.offer(5);
pq.offer(1);
pq.offer(3);
System.out.println(pq.poll()); // 1 (smallest first)

// Custom comparator for max heap
Queue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
```

**Characteristics:**
- Elements ordered by priority
- O(log n) for insertion and deletion
- Not FIFO - priority-based
- Unbounded queue

### ArrayDeque<E>
**Implementation:** Resizable array
**Thread-Safe:** No

```java
Deque<String> deque = new ArrayDeque<>();
deque.addFirst("Front");
deque.addLast("Back");
deque.offerFirst("NewFront");
deque.offerLast("NewBack");

// Can be used as Stack
deque.push("StackTop");
String popped = deque.pop();

// Can be used as Queue
deque.offer("QueueRear");
String polled = deque.poll();
```

**Characteristics:**
- Double-ended queue
- Can be used as stack or queue
- Better than Stack for stack operations
- Better than LinkedList for queue operations

## Map Interface

Maps store **key-value pairs** with **unique keys**.

### HashMap<K,V>
**Implementation:** Hash table
**Thread-Safe:** No
**Ordering:** No ordering guaranteed

```java
Map<String, Integer> hashMap = new HashMap<>();
hashMap.put("Apple", 5);
hashMap.put("Banana", 3);
hashMap.put("Apple", 7); // Overwrites previous value

Integer value = hashMap.get("Apple"); // 7
boolean hasKey = hashMap.containsKey("Orange"); // false
Set<String> keys = hashMap.keySet();
Collection<Integer> values = hashMap.values();
```

**Characteristics:**
- O(1) average time for basic operations
- Allows one null key and multiple null values
- No ordering of elements
- Best general-purpose Map implementation

### LinkedHashMap<K,V>
**Implementation:** Hash table + doubly-linked list
**Thread-Safe:** No
**Ordering:** Insertion order or access order

```java
Map<String, Integer> linkedHashMap = new LinkedHashMap<>();
linkedHashMap.put("Third", 3);
linkedHashMap.put("First", 1);
linkedHashMap.put("Second", 2);
// Iteration order: Third, First, Second

// Access-order LinkedHashMap (LRU cache)
Map<String, Integer> accessOrder = new LinkedHashMap<>(16, 0.75f, true);
```

**Characteristics:**
- Maintains insertion or access order
- Slightly slower than HashMap
- Useful for LRU cache implementation
- Predictable iteration order

### TreeMap<K,V>
**Implementation:** Red-Black tree
**Thread-Safe:** No
**Ordering:** Natural ordering of keys or custom Comparator

```java
Map<String, Integer> treeMap = new TreeMap<>();
treeMap.put("Zebra", 26);
treeMap.put("Apple", 1);
treeMap.put("Banana", 2);
// Iteration order: Apple, Banana, Zebra (sorted by keys)

// Navigation methods
String firstKey = treeMap.firstKey();
String lastKey = treeMap.lastKey();
Map<String, Integer> subMap = treeMap.subMap("B", "Z");
```

**Characteristics:**
- O(log n) for basic operations
- Keys stored in sorted order
- No null keys allowed (null values OK)
- Implements NavigableMap interface

### Hashtable<K,V>
**Implementation:** Hash table
**Thread-Safe:** Yes
**Ordering:** No ordering guaranteed

```java
Map<String, Integer> hashtable = new Hashtable<>();
hashtable.put("Key1", 1);
hashtable.put("Key2", 2);
// hashtable.put(null, 3); // NullPointerException
// hashtable.put("Key3", null); // NullPointerException
```

**Characteristics:**
- Legacy class (Java 1.0)
- Synchronized methods
- No null keys or values allowed
- Use ConcurrentHashMap for better performance

### EnumMap<K,V>
**Implementation:** Array
**Thread-Safe:** No
**Ordering:** Enum declaration order

```java
enum Color { RED, GREEN, BLUE }

Map<Color, String> enumMap = new EnumMap<>(Color.class);
enumMap.put(Color.RED, "Stop");
enumMap.put(Color.GREEN, "Go");
enumMap.put(Color.BLUE, "Caution");
```

**Characteristics:**
- Extremely efficient for enum keys
- Compact representation
- Type-safe
- Natural ordering of enum constants

## Iterator Interfaces

### Iterator<E>
```java
Collection<String> collection = Arrays.asList("A", "B", "C");
Iterator<String> it = collection.iterator();
while (it.hasNext()) {
    String element = it.next();
    if (element.equals("B")) {
        it.remove(); // Safe removal during iteration
    }
}
```

### ListIterator<E>
```java
List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
ListIterator<String> listIt = list.listIterator();

// Forward iteration
while (listIt.hasNext()) {
    String element = listIt.next();
    listIt.set(element.toLowerCase()); // Modify element
}

// Backward iteration
while (listIt.hasPrevious()) {
    String element = listIt.previous();
    System.out.println(element);
}
```

## Utility Classes

### Collections
Static utility methods for collections:

```java
List<String> list = new ArrayList<>(Arrays.asList("C", "A", "B"));

Collections.sort(list); // [A, B, C]
Collections.reverse(list); // [C, B, A]
Collections.shuffle(list); // Random order

String max = Collections.max(list);
String min = Collections.min(list);

List<String> unmodifiable = Collections.unmodifiableList(list);
List<String> synchronized = Collections.synchronizedList(list);
```

### Arrays
Static utility methods for arrays:

```java
String[] array = {"C", "A", "B"};
Arrays.sort(array); // [A, B, C]

List<String> list = Arrays.asList(array); // Fixed-size list
boolean equal = Arrays.equals(array1, array2);
String str = Arrays.toString(array); // "[A, B, C]"
```

## Performance Comparison

| Operation | ArrayList | LinkedList | HashSet | TreeSet | HashMap | TreeMap |
|-----------|-----------|------------|---------|---------|---------|---------|
| Add | O(1)* | O(1) | O(1)* | O(log n) | O(1)* | O(log n) |
| Remove | O(n) | O(1)** | O(1)* | O(log n) | O(1)* | O(log n) |
| Search | O(n) | O(n) | O(1)* | O(log n) | O(1)* | O(log n) |
| Access | O(1) | O(n) | N/A | N/A | O(1)* | O(log n) |

*Average case, O(n) worst case for hash-based structures  
**O(1) if you have reference to the node, O(n) if searching first

## When to Use What

### Use ArrayList when:
- You need indexed access to elements
- You do more reading than inserting/deleting
- You need a simple, general-purpose list

### Use LinkedList when:
- You frequently insert/delete at the beginning or middle
- You implement a queue or deque
- You don't need random access to elements

### Use HashSet when:
- You need to ensure uniqueness
- You don't care about ordering
- You need fast lookups

### Use TreeSet when:
- You need sorted elements
- You need navigational methods (first, last, subSet)
- You can sacrifice some performance for ordering

### Use HashMap when:
- You need key-value associations
- You need fast lookups by key
- You don't care about ordering

### Use TreeMap when:
- You need sorted keys
- You need navigational methods
- You need a sorted view of key-value pairs

### Use LinkedHashMap when:
- You need predictable iteration order
- You're implementing an LRU cache
- You want HashMap performance with ordering

### Use PriorityQueue when:
- You need elements processed in priority order
- You're implementing algorithms like Dijkstra's or A*
- You need a heap data structure

## Best Practices

1. **Program to interfaces**: Use `List<String>` instead of `ArrayList<String>`
2. **Initialize with capacity**: `new ArrayList<>(100)` for known sizes
3. **Use appropriate initial capacity**: Avoid frequent resizing
4. **Use enhanced for-loop**: When you don't need index or modification
5. **Use Collections utility methods**: For common operations
6. **Be careful with equals() and hashCode()**: When using custom objects as keys
7. **Use concurrent collections**: For multi-threaded environments
8. **Consider immutable collections**: For thread safety and clarity

## Thread-Safe Alternatives

| Non-Thread-Safe | Thread-Safe Alternative |
|----------------|------------------------|
| ArrayList | Vector, Collections.synchronizedList(), CopyOnWriteArrayList |
| HashMap | Hashtable, Collections.synchronizedMap(), ConcurrentHashMap |
| HashSet | Collections.synchronizedSet(), ConcurrentHashMap.newKeySet() |
| TreeMap | Collections.synchronizedSortedMap(), ConcurrentSkipListMap |

For modern applications, prefer `java.util.concurrent` collections over synchronized wrappers for better performance.
