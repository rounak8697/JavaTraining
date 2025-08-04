
# 🔍 What is a Cursor in Java?

In Java, a **Cursor** is an **interface** used to **traverse or iterate** elements of a **collection** (like `List`, `Set`, etc.). It is not an actual data structure, but a **mechanism to visit each element**, one at a time.

Java provides **three types of Cursors** to iterate collections:

---

## 🔁 1. Enumeration (Legacy Cursor)

- **Introduced in:** Java 1.0  
- **Applicable to:** Legacy classes like `Vector`, `Stack`, `Hashtable`
- **Direction:** Forward only

### 👉 Methods:
```java
boolean hasMoreElements()
Object nextElement()
```

### ✅ Example:
```java
import java.util.*;

public class EnumExample {
    public static void main(String[] args) {
        Vector<String> vec = new Vector<>();
        vec.add("Java");
        vec.add("Python");

        Enumeration<String> e = vec.elements();
        while (e.hasMoreElements()) {
            System.out.println(e.nextElement());
        }
    }
}
```

---

## 🔁 2. Iterator

- **Introduced in:** Java 1.2  
- **Applicable to:** All Collection classes
- **Direction:** Forward only
- **Allows:** Safe element removal during iteration

### 👉 Methods:
```java
boolean hasNext()
Object next()
void remove()
```

### ✅ Example:
```java
import java.util.*;

public class IteratorExample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
        Iterator<String> it = list.iterator();
        
        while (it.hasNext()) {
            String val = it.next();
            if (val.equals("B")) {
                it.remove(); // safe removal
            }
        }
        System.out.println(list); // [A, C]
    }
}
```

---

## 🔁 3. ListIterator

- **Introduced in:** Java 1.2  
- **Applicable to:** List implementations only
- **Direction:** Bidirectional (forward & backward)
- **Allows:** Add, remove, and modify elements during iteration

### 👉 Methods:
```java
boolean hasNext()
Object next()
boolean hasPrevious()
Object previous()
void remove()
void add(E e)
void set(E e)
```

### ✅ Example:
```java
import java.util.*;

public class ListIterExample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>(Arrays.asList("X", "Y", "Z"));
        ListIterator<String> it = list.listIterator();
        
        while (it.hasNext()) {
            String val = it.next();
            if (val.equals("Y")) {
                it.set("YY");
                it.add("NEW");
            }
        }
        System.out.println(list); // [X, YY, NEW, Z]
    }
}
```

---

## 📌 Summary Table

| Cursor Type     | Direction | Modify Elements | Add | Remove | Backward Traversal | Applicable Collections |
|-----------------|-----------|-----------------|-----|--------|---------------------|------------------------|
| Enumeration     | Forward   | ❌              | ❌  | ❌     | ❌                  | Vector, Hashtable      |
| Iterator        | Forward   | ❌              | ❌  | ✅     | ❌                  | All Collections        |
| ListIterator    | Both      | ✅              | ✅  | ✅     | ✅                  | List only              |

---

## ✅ Use Cases

- **Enumeration** → Read-only access on legacy collections.
- **Iterator** → Traverse and remove elements from any collection.
- **ListIterator** → Navigate in both directions, modify list on-the-go.
