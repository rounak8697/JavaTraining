
# Set in Java

In Java, the `Set` interface is a part of the `java.util` package and extends the `Collection` interface. It represents an **unordered collection of unique elements**, meaning it does not allow duplicate values.

---

## ✅ Why Use Set?

- To store **unique items only**.
- To **remove duplicates** from a collection.
- To **perform set operations** like union, intersection, difference.

---

## ✅ Set Interface Hierarchy

```
java.util.Collection
   └── java.util.Set
         ├── HashSet
         ├── LinkedHashSet
         ├── TreeSet
         ├── EnumSet
         ├── CopyOnWriteArraySet
         └── ConcurrentSkipListSet
```

---

## ✅ Common Methods in Set

| Method                  | Description |
|-------------------------|-------------|
| `add(E e)`              | Adds the specified element to the set if it is not already present. |
| `remove(Object o)`      | Removes the specified element if it exists. |
| `contains(Object o)`    | Returns `true` if the element exists in the set. |
| `size()`                | Returns the number of elements. |
| `isEmpty()`             | Returns `true` if the set is empty. |
| `clear()`               | Removes all elements from the set. |
| `iterator()`            | Returns an iterator for traversing the elements. |

---

## ✅ HashSet

- **Unordered**
- **Allows one null**
- Fast performance for operations like add, remove, contains (O(1))

### 🔸 Example:

```java
import java.util.HashSet;

public class HashSetExample {
    public static void main(String[] args) {
        HashSet<String> set = new HashSet<>();
        set.add("Java");
        set.add("Python");
        set.add("Java"); // Duplicate ignored
        set.add(null);   // One null allowed

        for (String s : set) {
            System.out.println(s);
        }
    }
}
```

---

## ✅ LinkedHashSet

- Maintains **insertion order**
- **Allows one null**
- Slightly slower than `HashSet` due to ordering

### 🔸 Example:

```java
import java.util.LinkedHashSet;

public class LinkedHashSetExample {
    public static void main(String[] args) {
        LinkedHashSet<String> set = new LinkedHashSet<>();
        set.add("Java");
        set.add("Python");
        set.add("C++");
        set.add("Python"); // Duplicate ignored

        System.out.println(set); // Output: [Java, Python, C++]
    }
}
```

---

## ✅ TreeSet

- Maintains **sorted (natural or custom) order**
- **Does NOT allow null** if using natural order
- Backed by a Red-Black Tree

### 🔸 Example:

```java
import java.util.TreeSet;

public class TreeSetExample {
    public static void main(String[] args) {
        TreeSet<Integer> numbers = new TreeSet<>();
        numbers.add(30);
        numbers.add(10);
        numbers.add(20);

        System.out.println(numbers); // Output: [10, 20, 30]
    }
}
```

### 🔸 TreeSet with Custom Comparator Supporting Null

```java
import java.util.*;

public class CustomTreeSetWithNull {
    public static void main(String[] args) {
        TreeSet<String> set = new TreeSet<>(Comparator.nullsFirst(String::compareTo));
        set.add("Banana");
        set.add(null);
        set.add("Apple");

        System.out.println(set); // Output: [null, Apple, Banana]
    }
}
```

---

## ✅ EnumSet

- Designed only for use with enum types
- **Does NOT allow null**
- Very efficient (uses bit vectors)

### 🔸 Example:

```java
import java.util.EnumSet;

enum Day {
    MONDAY, TUESDAY, WEDNESDAY
}

public class EnumSetExample {
    public static void main(String[] args) {
        EnumSet<Day> days = EnumSet.of(Day.MONDAY, Day.WEDNESDAY);
        System.out.println(days); // Output: [MONDAY, WEDNESDAY]
    }
}
```

---

## ✅ CopyOnWriteArraySet

- Thread-safe variant
- Backed by `CopyOnWriteArrayList`
- Good for **read-heavy** scenarios
- Allows **one null**

### 🔸 Example:

```java
import java.util.concurrent.CopyOnWriteArraySet;

public class CopyOnWriteArraySetExample {
    public static void main(String[] args) {
        CopyOnWriteArraySet<String> set = new CopyOnWriteArraySet<>();
        set.add("One");
        set.add("Two");
        set.add(null);

        System.out.println(set); // Output: [One, Two, null]
    }
}
```

---

## ✅ ConcurrentSkipListSet

- Thread-safe and sorted (like TreeSet)
- **Does NOT allow null**
- Backed by skip list

### 🔸 Example:

```java
import java.util.concurrent.ConcurrentSkipListSet;

public class ConcurrentSkipListSetExample {
    public static void main(String[] args) {
        ConcurrentSkipListSet<String> set = new ConcurrentSkipListSet<>();
        set.add("A");
        set.add("B");
        // set.add(null); // Throws NullPointerException

        System.out.println(set); // Output: [A, B]
    }
}
```

---

## ✅ Null Support Comparison

| Set Implementation       | Allows `null`? |
|--------------------------|----------------|
| `HashSet`                | ✅ Yes (1 null) |
| `LinkedHashSet`          | ✅ Yes (1 null) |
| `TreeSet` (natural)      | ❌ No |
| `TreeSet` (with comparator)| ✅ Yes (if comparator handles null) |
| `EnumSet`                | ❌ No |
| `CopyOnWriteArraySet`    | ✅ Yes (1 null) |
| `ConcurrentSkipListSet`  | ❌ No |

---

## ✅ When to Use Which Set

| Use Case | Best Set Implementation |
|----------|-------------------------|
| Fast access, no order needed | `HashSet` |
| Maintain insertion order | `LinkedHashSet` |
| Need elements in sorted order | `TreeSet` |
| Enum-based collections | `EnumSet` |
| Thread-safe with low writes | `CopyOnWriteArraySet` |
| Thread-safe with sorted elements | `ConcurrentSkipListSet` |

---

## ✅ Set Operations

### 🔸 Union

```java
set1.addAll(set2);
```

### 🔸 Intersection

```java
set1.retainAll(set2);
```

### 🔸 Difference

```java
set1.removeAll(set2);
```

---

## ✅ Summary

- `Set` ensures **uniqueness**.
- Each implementation has a specific **strength** (order, performance, thread safety).
- Choose based on your **specific needs** (ordering, concurrency, performance).

