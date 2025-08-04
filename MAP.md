
# Is `Map` part of the Java Collection Framework?

## ✅ Short Answer:
- `Map` is **part of the Java Collections Framework**
- But it does **not extend** the `Collection` interface

---

## 📦 Java Collections Framework Overview

Java Collections Framework is a **unified architecture** for representing and manipulating collections (groups of objects). It includes:

- **Interfaces** like `Collection`, `List`, `Set`, `Queue`, and `Map`
- **Implementations** like `ArrayList`, `HashSet`, `LinkedList`, `HashMap`
- **Algorithms** and utility classes (`Collections`, `Arrays`)

---

## 🧩 Interface Hierarchy

### Collection Interface:
```text
              Collection
                / | \
      List     Set  Queue
```

- `List`, `Set`, `Queue` are subinterfaces of `Collection`
- Examples:
  - `ArrayList` implements `List`
  - `HashSet` implements `Set`
  - `PriorityQueue` implements `Queue`

---

### Map Interface:
```text
              Map  ← NOT a subinterface of Collection
```

- `Map` is **not** derived from `Collection`
- It is a **separate root interface**
- It is still part of the Java Collections Framework

---

## ❓ Why `Map` Is Separate?

| Feature | Collection | Map |
|--------|------------|-----|
| Represents | Group of elements | Key-value pairs |
| Typical methods | `add(E)`, `remove(E)`, `iterator()` | `put(K,V)`, `get(K)`, `keySet()` |
| Example | List of students | Student roll number → Student name |

Because a `Map` stores entries (`key → value` pairs), it does **not behave like a Collection of individual elements**, hence it is separate.

---

## ✅ Included in Java Collections Framework?

| Interface | Part of Collection Framework? | Subinterface of `Collection`? |
|----------|-------------------------------|-------------------------------|
| `List`   | ✅ Yes                        | ✅ Yes                        |
| `Set`    | ✅ Yes                        | ✅ Yes                        |
| `Queue`  | ✅ Yes                        | ✅ Yes                        |
| `Map`    | ✅ Yes                        | ❌ No                         |

---

## 🧪 Example Code:

```java
import java.util.*;

public class MapDemo {
    public static void main(String[] args) {
        Map<Integer, String> map = new HashMap<>();
        map.put(101, "Java");
        map.put(102, "Python");
        map.put(103, "C++");

        System.out.println("Course Code to Name:");
        for (Map.Entry<Integer, String> entry : map.entrySet()) {
            System.out.println(entry.getKey() + " → " + entry.getValue());
        }
    }
}
```

---

## 🔚 Conclusion:
- `Map` is **included in the Java Collections Framework**
- But it does **not extend** the `Collection` interface
- It serves a different purpose: storing **key-value pairs**

So yes — **`Map` is in the Collection Framework, but not a subtype of Collection.**
