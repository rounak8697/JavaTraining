# 🗺️ Java Map Interface - Complete Guide

**Date:** 2025-08-04

## What is a Map in Java?
A **Map** is an object that maps **keys to values**. It cannot contain duplicate keys, and each key maps to at most one value.

---

## 🔢 Types of Maps in Java

### 1. HashMap
- **Order**: Unordered
- **Null**: Allows one `null` key and multiple `null` values
- **Thread Safe**: ❌
- **Performance**: Fast (`O(1)` for get/put)

**Use Case**: Storing configs, caching data

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 2);
```

---

### 2. LinkedHashMap
- **Order**: Maintains insertion order
- **Null**: Allows null keys and values
- **Special Feature**: Can be used to create LRU cache

**Use Case**: LRU cache, ordered configs

```java
Map<String, Integer> lruCache = new LinkedHashMap<>(16, 0.75f, true);
```

---

### 3. TreeMap
- **Order**: Sorted by natural order or custom comparator
- **Null**: Does not allow `null` keys
- **Performance**: `O(log n)` operations

**Use Case**: Sorted dictionary, range searches

```java
Map<Integer, String> sortedMap = new TreeMap<>();
```

---

### 4. ConcurrentHashMap
- **Thread Safe**: ✅ (no need to synchronize)
- **Null**: Does not allow `null` keys or values

**Use Case**: Thread-safe cache

```java
Map<String, String> concurrentMap = new ConcurrentHashMap<>();
```

---

### 5. Hashtable
- **Thread Safe**: ✅ (legacy, synchronized)
- **Null**: ❌ Does not allow `null` key or value

**Use Case**: Legacy codebases

```java
Map<String, String> legacyMap = new Hashtable<>();
```

---

### 6. WeakHashMap
- **Key Storage**: Weak references
- **GC Behavior**: Entries removed when key is no longer referenced

**Use Case**: Memory-sensitive caches

```java
Map<Object, String> weakMap = new WeakHashMap<>();
```

---

### 7. IdentityHashMap
- **Comparison**: Uses `==` instead of `.equals()`
- **Null**: Allows null keys and values

**Use Case**: Serialization frameworks, object identity

```java
Map<Object, String> idMap = new IdentityHashMap<>();
```

---

### 8. EnumMap
- **Key Type**: Only enum keys
- **Performance**: Very fast, internally array-based

**Use Case**: Efficient mapping for enums

```java
enum Day { MON, TUE, WED }
Map<Day, String> schedule = new EnumMap<>(Day.class);
```

---

### 9. NavigableMap
- **Extends**: `SortedMap`
- **Extra Methods**: `lowerKey()`, `floorKey()`, `ceilingKey()`, `higherKey()`

**Use Case**: Range-based queries

```java
NavigableMap<Integer, String> navMap = new TreeMap<>();
```

---

### 10. SortedMap
- **Order**: Maintains natural key order
- **Implemented By**: TreeMap

**Use Case**: Sorted views of data

```java
SortedMap<String, String> sortedMap = new TreeMap<>();
```

---

## 🔚 Summary Table

| Map Type            | Order Maintained     | Sorted | Thread Safe | Null Keys | Comparison     |
|---------------------|----------------------|--------|--------------|-----------|----------------|
| HashMap             | ❌                   | ❌     | ❌           | ✅        | `.equals()`    |
| LinkedHashMap       | ✅ (Insertion)        | ❌     | ❌           | ✅        | `.equals()`    |
| TreeMap             | ✅ (Sorted)           | ✅     | ❌           | ❌        | `.compareTo()` |
| ConcurrentHashMap   | ❌                   | ❌     | ✅           | ❌        | `.equals()`    |
| Hashtable           | ❌                   | ❌     | ✅           | ❌        | `.equals()`    |
| WeakHashMap         | ❌                   | ❌     | ❌           | ✅        | `.equals()`    |
| IdentityHashMap     | ❌                   | ❌     | ❌           | ✅        | `==`           |
| EnumMap             | ✅ (Enum Order)       | ✅     | ❌           | ❌        | Enum ordinal   |

---

📝 If you are dealing with multithreaded applications, prefer `ConcurrentHashMap`. For ordering, use `LinkedHashMap` or `TreeMap`. If your keys are enums, `EnumMap` is highly efficient.
