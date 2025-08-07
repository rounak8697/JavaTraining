# 🔢 Java Set Interface - Complete Guide

**Date:** 2025-08-05

## What is a Set in Java?

A **Set** is a collection that contains **no duplicate elements**. It models the mathematical set abstraction and is one of the core interfaces in the Java Collections Framework. Sets are used when you need to ensure uniqueness of elements and perform set operations like union, intersection, and difference.

Key characteristics:
- No duplicate elements allowed
- At most one null element (depends on implementation)
- Models mathematical set operations
- Extends the Collection interface
- Order may or may not be maintained (depends on implementation)

---

## 🔢 Types of Sets in Java

### 1. HashSet

**Characteristics:**
- **Order**: No guaranteed order
- **Null**: Allows one `null` element
- **Thread Safe**: ❌ Not synchronized
- **Performance**: O(1) average time for add/remove/contains
- **Implementation**: Hash table (HashMap internally)
- **Load Factor**: Default 0.75

**Use Cases:** General-purpose set operations, removing duplicates, membership testing

```java
import java.util.*;

public class HashSetExample {
    public static void main(String[] args) {
        Set<String> fruits = new HashSet<>();
        
        // Adding elements
        fruits.add("apple");
        fruits.add("banana");
        fruits.add("orange");
        fruits.add("apple"); // Duplicate - won't be added
        fruits.add(null); // null allowed
        
        System.out.println("Fruits: " + fruits);
        // Output: [null, banana, orange, apple] (order not guaranteed)
        System.out.println("Size: " + fruits.size()); // 4
        
        // Checking membership
        if (fruits.contains("apple")) {
            System.out.println("Apple is in the set");
        }
        
        // Removing duplicates from a list
        List<Integer> numbersWithDuplicates = Arrays.asList(1, 2, 2, 3, 3, 3, 4, 5, 5);
        Set<Integer> uniqueNumbers = new HashSet<>(numbersWithDuplicates);
        System.out.println("Original: " + numbersWithDuplicates);
        System.out.println("Unique: " + uniqueNumbers);
        
        // Set operations
        Set<String> set1 = new HashSet<>(Arrays.asList("A", "B", "C"));
        Set<String> set2 = new HashSet<>(Arrays.asList("B", "C", "D"));
        
        // Union
        Set<String> union = new HashSet<>(set1);
        union.addAll(set2);
        System.out.println("Union: " + union); // [A, B, C, D]
        
        // Intersection
        Set<String> intersection = new HashSet<>(set1);
        intersection.retainAll(set2);
        System.out.println("Intersection: " + intersection); // [B, C]
        
        // Difference
        Set<String> difference = new HashSet<>(set1);
        difference.removeAll(set2);
        System.out.println("Difference: " + difference); // [A]
        
        // Iteration
        for (String fruit : fruits) {
            if (fruit != null) {
                System.out.println("Processing: " + fruit);
            }
        }
    }
}
```

---

### 2. LinkedHashSet

**Characteristics:**
- **Order**: Maintains insertion order
- **Null**: Allows one `null` element
- **Thread Safe**: ❌ Not synchronized
- **Performance**: Slightly slower than HashSet due to maintaining order
- **Implementation**: Hash table with linked list (LinkedHashMap internally)

**Use Cases:** Preserving insertion order while ensuring uniqueness, ordered set operations

```java
import java.util.*;

public class LinkedHashSetExample {
    public static void main(String[] args) {
        Set<String> orderedSet = new LinkedHashSet<>();
        
        // Adding elements in specific order
        orderedSet.add("first");
        orderedSet.add("second");
        orderedSet.add("third");
        orderedSet.add("second"); // Duplicate - won't be added
        
        System.out.println("Ordered Set: " + orderedSet);
        // Output: [first, second, third] (insertion order maintained)
        
        // Preserving order while removing duplicates
        String[] words = {"apple", "banana", "apple", "cherry", "banana", "date"};
        Set<String> uniqueWords = new LinkedHashSet<>(Arrays.asList(words));
        System.out.println("Original: " + Arrays.toString(words));
        System.out.println("Unique (ordered): " + uniqueWords);
        // Output: [apple, banana, cherry, date]
        
        // Building ordered unique collections
        LinkedHashSet<Integer> buildOrder = new LinkedHashSet<>();
        int[] dependencies = {3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5};
        
        for (int dep : dependencies) {
            buildOrder.add(dep);
        }
        
        System.out.println("Build order: " + buildOrder);
        // Output: [3, 1, 4, 5, 9, 2, 6] (insertion order, no duplicates)
        
        // Converting to array while preserving order
        String[] uniqueArray = uniqueWords.toArray(new String[0]);
        System.out.println("Unique array: " + Arrays.toString(uniqueArray));
        
        // Set operations with order preservation
        LinkedHashSet<String> set1 = new LinkedHashSet<>(Arrays.asList("A", "B", "C"));
        LinkedHashSet<String> set2 = new LinkedHashSet<>(Arrays.asList("C", "D", "E"));
        
        LinkedHashSet<String> orderedUnion = new LinkedHashSet<>(set1);
        orderedUnion.addAll(set2);
        System.out.println("Ordered Union: " + orderedUnion); // [A, B, C, D, E]
    }
}
```

---

### 3. TreeSet

**Characteristics:**
- **Order**: Sorted order (natural or custom comparator)
- **Null**: Does not allow `null` elements
- **Thread Safe**: ❌ Not synchronized
- **Performance**: O(log n) for add/remove/contains operations
- **Implementation**: Red-Black tree (TreeMap internally)
- **Navigation**: Supports range operations and navigation methods

**Use Cases:** Sorted sets, range queries, ordered data processing, priority-based operations

```java
import java.util.*;

public class TreeSetExample {
    public static void main(String[] args) {
        // Natural ordering
        TreeSet<Integer> numbers = new TreeSet<>();
        numbers.addAll(Arrays.asList(5, 2, 8, 1, 9, 3));
        
        System.out.println("Sorted numbers: " + numbers);
        // Output: [1, 2, 3, 5, 8, 9]
        
        // Custom comparator (reverse order)
        TreeSet<String> reverseWords = new TreeSet<>(Collections.reverseOrder());
        reverseWords.addAll(Arrays.asList("apple", "banana", "cherry", "date"));
        
        System.out.println("Reverse order: " + reverseWords);
        // Output: [date, cherry, banana, apple]
        
        // Navigation methods
        System.out.println("First: " + numbers.first()); // 1
        System.out.println("Last: " + numbers.last());   // 9
        System.out.println("Lower than 5: " + numbers.lower(5)); // 3
        System.out.println("Higher than 5: " + numbers.higher(5)); // 8
        System.out.println("Floor of 4: " + numbers.floor(4)); // 3
        System.out.println("Ceiling of 4: " + numbers.ceiling(4)); // 5
        
        // Range operations
        SortedSet<Integer> headSet = numbers.headSet(5);
        System.out.println("Numbers < 5: " + headSet); // [1, 2, 3]
        
        SortedSet<Integer> tailSet = numbers.tailSet(5);
        System.out.println("Numbers >= 5: " + tailSet); // [5, 8, 9]
        
        NavigableSet<Integer> subSet = numbers.subSet(2, true, 8, false);
        System.out.println("Numbers [2, 8): " + subSet); // [2, 3, 5]
        
        // Poll operations (remove and return)
        TreeSet<Integer> workingSet = new TreeSet<>(numbers);
        System.out.println("Poll first: " + workingSet.pollFirst()); // 1
        System.out.println("Poll last: " + workingSet.pollLast());   // 9
        System.out.println("After polling: " + workingSet); // [2, 3, 5, 8]
        
        // Custom object sorting
        TreeSet<Person> people = new TreeSet<>(Comparator.comparing(Person::getName));
        people.add(new Person("Alice", 30));
        people.add(new Person("Bob", 25));
        people.add(new Person("Charlie", 35));
        
        System.out.println("People sorted by name:");
        for (Person person : people) {
            System.out.println(person);
        }
        
        // Multiple criteria sorting
        TreeSet<Person> peopleByAgeAndName = new TreeSet<>(
            Comparator.comparing(Person::getAge).thenComparing(Person::getName)
        );
        peopleByAgeAndName.addAll(people);
        peopleByAgeAndName.add(new Person("David", 25)); // Same age as Bob
        
        System.out.println("People sorted by age then name:");
        for (Person person : peopleByAgeAndName) {
            System.out.println(person);
        }
    }
    
    static class Person {
        private String name;
        private int age;
        
        public Person(String name, int age) {
            this.name = name;
            this.age = age;
        }
        
        public String getName() { return name; }
        public int getAge() { return age; }
        
        @Override
        public String toString() {
            return name + " (" + age + ")";
        }
    }
}
```

---

### 4. EnumSet

**Characteristics:**
- **Element Type**: Only enum elements
- **Performance**: Extremely fast, internally uses bit vectors
- **Order**: Natural enum order
- **Null**: Does not allow `null` elements
- **Memory**: Very memory efficient

**Use Cases:** Working with enum flags, permissions, states, configuration options

```java
import java.util.*;

enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}

enum Permission {
    READ, WRITE, EXECUTE, DELETE, ADMIN
}

public class EnumSetExample {
    public static void main(String[] args) {
        // Creating EnumSet with specific elements
        EnumSet<Day> weekdays = EnumSet.of(Day.MONDAY, Day.TUESDAY, Day.WEDNESDAY, 
                                          Day.THURSDAY, Day.FRIDAY);
        System.out.println("Weekdays: " + weekdays);
        
        // Creating EnumSet with range
        EnumSet<Day> weekend = EnumSet.range(Day.SATURDAY, Day.SUNDAY);
        System.out.println("Weekend: " + weekend);
        
        // All elements
        EnumSet<Day> allDays = EnumSet.allOf(Day.class);
        System.out.println("All days: " + allDays);
        
        // Complement (opposite)
        EnumSet<Day> notWeekdays = EnumSet.complementOf(weekdays);
        System.out.println("Not weekdays: " + notWeekdays); // [SATURDAY, SUNDAY]
        
        // Permission system example
        EnumSet<Permission> userPermissions = EnumSet.of(Permission.READ, Permission.WRITE);
        EnumSet<Permission> adminPermissions = EnumSet.allOf(Permission.class);
        
        System.out.println("User permissions: " + userPermissions);
        System.out.println("Admin permissions: " + adminPermissions);
        
        // Checking permissions
        if (userPermissions.contains(Permission.READ)) {
            System.out.println("User can read");
        }
        
        if (userPermissions.contains(Permission.ADMIN)) {
            System.out.println("User has admin access");
        } else {
            System.out.println("User does not have admin access");
        }
        
        // Set operations with permissions
        EnumSet<Permission> requiredPermissions = EnumSet.of(Permission.READ, Permission.WRITE, Permission.EXECUTE);
        EnumSet<Permission> hasAllRequired = EnumSet.copyOf(userPermissions);
        hasAllRequired.retainAll(requiredPermissions);
        
        if (hasAllRequired.equals(requiredPermissions)) {
            System.out.println("User has all required permissions");
        } else {
            EnumSet<Permission> missing = EnumSet.copyOf(requiredPermissions);
            missing.removeAll(userPermissions);
            System.out.println("Missing permissions: " + missing);
        }
        
        // Feature flags example
        EnumSet<Feature> enabledFeatures = EnumSet.noneOf(Feature.class);
        enabledFeatures.add(Feature.DARK_MODE);
        enabledFeatures.add(Feature.NOTIFICATIONS);
        
        System.out.println("Enabled features: " + enabledFeatures);
        
        // Bulk operations
        EnumSet<Day> workingDays = EnumSet.copyOf(weekdays);
        workingDays.remove(Day.FRIDAY); // Early weekend!
        System.out.println("Working days: " + workingDays);
        
        // Performance comparison
        long startTime = System.nanoTime();
        for (int i = 0; i < 1000000; i++) {
            EnumSet<Day> testSet = EnumSet.of(Day.MONDAY, Day.TUESDAY);
            testSet.contains(Day.MONDAY);
        }
        long enumSetTime = System.nanoTime() - startTime;
        
        startTime = System.nanoTime();
        for (int i = 0; i < 1000000; i++) {
            HashSet<Day> testSet = new HashSet<>(Arrays.asList(Day.MONDAY, Day.TUESDAY));
            testSet.contains(Day.MONDAY);
        }
        long hashSetTime = System.nanoTime() - startTime;
        
        System.out.println("EnumSet time: " + enumSetTime + " ns");
        System.out.println("HashSet time: " + hashSetTime + " ns");
        System.out.println("EnumSet is " + (hashSetTime / enumSetTime) + "x faster");
    }
    
    enum Feature {
        DARK_MODE, NOTIFICATIONS, AUTO_SAVE, BACKUP, SYNC
    }
}
```

---

### 5. ConcurrentSkipListSet

**Characteristics:**
- **Thread Safe**: ✅ Concurrent and lock-free
- **Order**: Sorted order (natural or custom comparator)
- **Null**: Does not allow `null` elements
- **Performance**: O(log n) for most operations
- **Implementation**: Skip list data structure

**Use Cases:** Concurrent sorted sets, thread-safe priority queues, concurrent range operations

```java
import java.util.concurrent.*;
import java.util.*;

public class ConcurrentSkipListSetExample {
    public static void main(String[] args) throws InterruptedException {
        ConcurrentSkipListSet<Integer> concurrentSet = new ConcurrentSkipListSet<>();
        
        // Concurrent additions
        ExecutorService executor = Executors.newFixedThreadPool(10);
        
        // Multiple threads adding elements concurrently
        for (int i = 0; i < 100; i++) {
            final int value = i;
            executor.submit(() -> {
                concurrentSet.add(value);
                concurrentSet.add(value + 1000); // Add some higher numbers too
            });
        }
        
        executor.shutdown();
        executor.awaitTermination(1, TimeUnit.SECONDS);
        
        System.out.println("Set size after concurrent additions: " + concurrentSet.size());
        System.out.println("First 10 elements: " + 
            concurrentSet.stream().limit(10).collect(Collectors.toList()));
        System.out.println("Last 10 elements: " + 
            concurrentSet.descendingSet().stream().limit(10).collect(Collectors.toList()));
        
        // Navigation methods (thread-safe)
        System.out.println("First: " + concurrentSet.first());
        System.out.println("Last: " + concurrentSet.last());
        System.out.println("Lower than 50: " + concurrentSet.lower(50));
        System.out.println("Higher than 50: " + concurrentSet.higher(50));
        
        // Concurrent iteration (doesn't throw ConcurrentModificationException)
        ConcurrentSkipListSet<String> words = new ConcurrentSkipListSet<>();
        words.addAll(Arrays.asList("apple", "banana", "cherry", "date", "elderberry"));
        
        // Safe concurrent modification during iteration
        for (String word : words) {
            System.out.println("Processing: " + word);
            if ("cherry".equals(word)) {
                // Safe to modify during iteration
                words.add("fig");
                words.add("grape");
            }
        }
        
        System.out.println("Final words set: " + words);
        
        // Range operations
        NavigableSet<String> subSet = words.subSet("banana", true, "fig", false);
        System.out.println("SubSet [banana, fig): " + subSet);
        
        // Descending iteration
        NavigableSet<String> descendingWords = words.descendingSet();
        System.out.println("Descending order: " + descendingWords);
        
        // Custom comparator example
        ConcurrentSkipListSet<String> lengthSorted = new ConcurrentSkipListSet<>(
            Comparator.comparing(String::length).thenComparing(String::compareTo)
        );
        
        lengthSorted.addAll(Arrays.asList("a", "bb", "ccc", "dd", "e", "ffff"));
        System.out.println("Length sorted: " + lengthSorted);
        
        // Polling operations (thread-safe)
        System.out.println("Poll first: " + concurrentSet.pollFirst());
        System.out.println("Poll last: " + concurrentSet.pollLast());
        
        // Producer-Consumer example with priority
        ConcurrentSkipListSet<Task> taskQueue = new ConcurrentSkipListSet<>(
            Comparator.comparing(Task::getPriority).reversed().thenComparing(Task::getId)
        );
        
        // Producer thread
        Thread producer = new Thread(() -> {
            for (int i = 0; i < 20; i++) {
                Task task = new Task(i, (int) (Math.random() * 10));
                taskQueue.add(task);
                System.out.println("Produced: " + task);
                try {
                    Thread.sleep(50);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                    break;
                }
            }
        });
        
        // Consumer thread
        Thread consumer = new Thread(() -> {
            while (!Thread.currentThread().isInterrupted()) {
                Task task = taskQueue.pollFirst();
                if (task != null) {
                    System.out.println("Consumed: " + task);
                }
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                    break;
                }
            }
        });
        
        producer.start();
        consumer.start();
        
        Thread.sleep(2000); // Let them run for 2 seconds
        
        producer.interrupt();
        consumer.interrupt();
        
        producer.join();
        consumer.join();
        
        System.out.println("Remaining tasks: " + taskQueue.size());
    }
    
    static class Task {
        private int id;
        private int priority;
        
        public Task(int id, int priority) {
            this.id = id;
            this.priority = priority;
        }
        
        public int getId() { return id; }
        public int getPriority() { return priority; }
        
        @Override
        public String toString() {
            return "Task{id=" + id + ", priority=" + priority + "}";
        }
        
        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (!(o instanceof Task)) return false;
            Task task = (Task) o;
            return id == task.id;
        }
        
        @Override
        public int hashCode() {
            return Objects.hash(id);
        }
    }
}
```

---

### 6. CopyOnWriteArraySet

**Characteristics:**
- **Thread Safe**: ✅ Thread-safe for concurrent read operations
- **Write Performance**: Expensive writes (copies entire array)
- **Read Performance**: Very fast reads (no synchronization needed)
- **Order**: Maintains insertion order
- **Use Case**: Read-heavy scenarios with infrequent writes

**Use Cases:** Observer patterns, event listeners, configuration sets with frequent reads

```java
import java.util.concurrent.*;
import java.util.*;

public class CopyOnWriteArraySetExample {
    public static void main(String[] args) throws InterruptedException {
        CopyOnWriteArraySet<String> observers = new CopyOnWriteArraySet<>();
        
        // Adding observers
        observers.add("EmailNotifier");
        observers.add("SMSNotifier");
        observers.add("PushNotifier");
        observers.add("LogNotifier");
        
        System.out.println("Initial observers: " + observers);
        
        // Simulating concurrent reads and writes
        ExecutorService executor = Executors.newFixedThreadPool(10);
        
        // Many readers (frequent operation)
        for (int i = 0; i < 20; i++) {
            executor.submit(() -> {
                // Frequent read operations
                for (String observer : observers) {
                    System.out.println("Notifying: " + observer);
                    // Simulate notification work
                    try {
                        Thread.sleep(10);
                    } catch (InterruptedException e) {
                        Thread.currentThread().interrupt();
                        return;
                    }
                }
            });
        }
        
        // Occasional writers (infrequent operation)
        for (int i = 0; i < 3; i++) {
            final int writerNum = i;
            executor.submit(() -> {
                try {
                    Thread.sleep(100); // Wait before modifying
                    observers.add("NewObserver" + writerNum);
                    System.out.println("Added NewObserver" + writerNum);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            });
        }
        
        executor.shutdown();
        executor.awaitTermination(2, TimeUnit.SECONDS);
        
        System.out.println("Final observers: " + observers);
        
        // Event system example
        EventSystem eventSystem = new EventSystem();
        
        // Register listeners
        eventSystem.addListener(event -> System.out.println("Logger: " + event));
        eventSystem.addListener(event -> System.out.println("Emailer: " + event));
        eventSystem.addListener(event -> System.out.println("Metrics: " + event));
        
        // Concurrent event publishing
        ExecutorService eventExecutor = Executors.newFixedThreadPool(5);
        
        for (int i = 0; i < 10; i++) {
            final int eventNum = i;
            eventExecutor.submit(() -> {
                eventSystem.publishEvent("Event " + eventNum);
            });
        }
        
        // Add more listeners during event processing
        eventExecutor.submit(() -> {
            try {
                Thread.sleep(50);
                eventSystem.addListener(event -> 
                    System.out.println("Late Listener: " + event));
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });
        
        eventExecutor.shutdown();
        eventExecutor.awaitTermination(1, TimeUnit.SECONDS);
        
        // Configuration example
        ConfigurationManager config = new ConfigurationManager();
        config.addFeatureFlag("DARK_MODE");
        config.addFeatureFlag("NOTIFICATIONS");
        config.addFeatureFlag("AUTO_SAVE");
        
        // Many threads checking configuration
        ExecutorService configExecutor = Executors.newFixedThreadPool(8);
        
        for (int i = 0; i < 50; i++) {
            configExecutor.submit(() -> {
                if (config.isFeatureEnabled("DARK_MODE")) {
                    System.out.println("Using dark mode UI");
                }
                if (config.isFeatureEnabled("NOTIFICATIONS")) {
                    System.out.println("Showing notifications");
                }
            });
        }
        
        // Occasionally update configuration
        configExecutor.submit(() -> {
            try {
                Thread.sleep(100);
                config.addFeatureFlag("EXPERIMENTAL_FEATURE");
                System.out.println("Added experimental feature");
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });
        
        configExecutor.shutdown();
        configExecutor.awaitTermination(1, TimeUnit.SECONDS);
        
        System.out.println("Final feature flags: " + config.getFeatureFlags());
    }
    
    // Event system using CopyOnWriteArraySet
    static class EventSystem {
        private final Set<EventListener> listeners = new CopyOnWriteArraySet<>();
        
        public void addListener(EventListener listener) {
            listeners.add(listener);
        }
        
        public void removeListener(EventListener listener) {
            listeners.remove(listener);
        }
        
        public void publishEvent(String event) {
            // Safe iteration even if listeners are added/removed concurrently
            for (EventListener listener : listeners) {
                try {
                    listener.onEvent(event);
                } catch (Exception e) {
                    System.err.println("Error in listener: " + e.getMessage());
                }
            }
        }
    }
    
    @FunctionalInterface
    interface EventListener {
        void onEvent(String event);
    }
    
    // Configuration manager using CopyOnWriteArraySet
    static class ConfigurationManager {
        private final Set<String> featureFlags = new CopyOnWriteArraySet<>();
        
        public void addFeatureFlag(String flag) {
            featureFlags.add(flag);
        }
        
        public void removeFeatureFlag(String flag) {
            featureFlags.remove(flag);
        }
        
        public boolean isFeatureEnabled(String flag) {
            return featureFlags.contains(flag);
        }
        
        public Set<String> getFeatureFlags() {
            return new HashSet<>(featureFlags); // Return copy for safety
        }
    }
}
```

---

### 7. NavigableSet Interface

**Characteristics:**
- **Extends**: SortedSet interface
- **Navigation Methods**: `lower()`, `floor()`, `ceiling()`, `higher()`
- **Implemented By**: TreeSet, ConcurrentSkipListSet
- **Reverse Navigation**: Supports reverse iteration and operations

**Use Cases:** Advanced navigation in sorted sets, range-based operations, bidirectional traversal

```java
import java.util.*;

public class NavigableSetExample {
    public static void main(String[] args) {
        NavigableSet<Integer> scores = new TreeSet<>();
        scores.addAll(Arrays.asList(65, 72, 78, 85, 91, 96, 88, 77, 82));
        
        System.out.println("All scores: " + scores);
        // Output: [65, 72, 77, 78, 82, 85, 88, 91, 96]
        
        // Navigation methods
        int targetScore = 80;
        System.out.println("Scores lower than " + targetScore + ": " + scores.lower(targetScore)); // 78
        System.out.println("Scores floor of " + targetScore + ": " + scores.floor(targetScore));   // 78
        System.out.println("Scores ceiling of " + targetScore + ": " + scores.ceiling(targetScore)); // 82
        System.out.println("Scores higher than " + targetScore + ": " + scores.higher(targetScore)); // 82
        
        // First and last
        System.out.println("Lowest score: " + scores.first());  // 65
        System.out.println("Highest score: " + scores.last());  // 96
        
        // Poll operations (remove and return)
        NavigableSet<Integer> workingScores = new TreeSet<>(scores);
        System.out.println("Remove lowest: " + workingScores.pollFirst()); // 65
        System.out.println("Remove highest: " + workingScores.pollLast()); // 96
        System.out.println("After polling: " + workingScores);
        
        // Range operations
        NavigableSet<Integer> passingScores = scores.tailSet(70, true);
        System.out.println("Passing scores (>=70): " + passingScores);
        
        NavigableSet<Integer> excellentScores = scores.tailSet(85, true);
        System.out.println("Excellent scores (>=85): " + excellentScores);
        
        NavigableSet<Integer> midRangeScores = scores.subSet(70, true, 90, false);
        System.out.println("Mid-range scores [70,90): " + midRangeScores);
        
        // Descending operations
        NavigableSet<Integer> descendingScores = scores.descendingSet();
        System.out.println("Descending order: " + descendingScores);
        
        Iterator<Integer> descendingIterator = scores.descendingIterator();
        System.out.print("Descending iteration: ");
        while (descendingIterator.hasNext()) {
            System.out.print(descendingIterator.next() + " ");
        }
        System.out.println();
        
        // Time-based example
        NavigableSet<TimeSlot> schedule = new TreeSet<>(Comparator.comparing(TimeSlot::getStartTime));
        schedule.add(new TimeSlot(9, 10, "Meeting A"));
        schedule.add(new TimeSlot(10, 11, "Break"));
        schedule.add(new TimeSlot(11, 12, "Meeting B"));
        schedule.add(new TimeSlot(14, 15, "Meeting C"));
        schedule.add(new TimeSlot(15, 16, "Code Review"));
        schedule.add(new TimeSlot(16, 17, "Planning"));
        
        System.out.println("\nSchedule:");
        for (TimeSlot slot : schedule) {
            System.out.println(slot);
        }
        
        // Find meetings before/after specific time
        TimeSlot currentTime = new TimeSlot(13, 13, "Current");
        TimeSlot previousMeeting = schedule.lower(currentTime);
        TimeSlot nextMeeting = schedule.higher(currentTime);
        
        System.out.println("Previous meeting: " + previousMeeting);
        System.out.println("Next meeting: " + nextMeeting);
        
        // Find available time slots
        NavigableSet<TimeSlot> morningMeetings = schedule.headSet(new TimeSlot(12, 12, "Lunch"), false);
        NavigableSet<TimeSlot> afternoonMeetings = schedule.tailSet(new TimeSlot(13, 13, "Afternoon"), true);
        
        System.out.println("Morning meetings: " + morningMeetings);
        System.out.println("Afternoon meetings: " + afternoonMeetings);
        
        // Priority queue simulation
        NavigableSet<Task> priorityTasks = new TreeSet<>(
            Comparator.comparing(Task::getPriority).reversed()
                     .thenComparing(Task::getCreatedTime)
        );
        
        priorityTasks.add(new Task("Fix bug", 1, System.currentTimeMillis()));
        priorityTasks.add(new Task("Write tests", 3, System.currentTimeMillis() + 1000));
        priorityTasks.add(new Task("Deploy", 1, System.currentTimeMillis() + 2000));
        priorityTasks.add(new Task("Code review", 2, System.currentTimeMillis() + 3000));
        
        System.out.println("\nPriority tasks:");
        while (!priorityTasks.isEmpty()) {
            Task nextTask = priorityTasks.pollFirst();
            System.out.println("Processing: " + nextTask);
        }
    }
    
    static class TimeSlot {
        private int startTime;
        private int endTime;
        private String activity;
        
        public TimeSlot(int startTime, int endTime, String activity) {
            this.startTime = startTime;
            this.endTime = endTime;
            this.activity = activity;
        }
        
        public int getStartTime() { return startTime; }
        public int getEndTime() { return endTime; }
        public String getActivity() { return activity; }
        
        @Override
        public String toString() {
            return String.format("%02d:00-%02d:00 %s", startTime, endTime, activity);
        }
    }
    
    static class Task {
        private String name;
        private int priority;
        private long createdTime;
        
        public Task(String name, int priority, long createdTime) {
            this.name = name;
            this.priority = priority;
            this.createdTime = createdTime;
        }
        
        public String getName() { return name; }
        public int getPriority() { return priority; }
        public long getCreatedTime() { return createdTime; }
        
        @Override
        public String toString() {
            return String.format("Task{name='%s', priority=%d}", name, priority);
        }
    }
}
```

---

### 8. SortedSet Interface

**Characteristics:**
- **Order**: Maintains natural order or custom comparator order
- **Range Views**: Provides subSet, headSet, tailSet methods
- **Implemented By**: TreeSet, ConcurrentSkipListSet
- **First/Last**: Access to first and last elements

**Use Cases:** Sorted collections, range operations, ordered data processing

```java
import java.util.*;

public class SortedSetExample {
    public static void main(String[] args) {
        SortedSet<String> words = new TreeSet<>();
        words.addAll(Arrays.asList("banana", "apple", "cherry", "date", "elderberry"));
        
        System.out.println("Sorted words: " + words);
        // Output: [apple, banana, cherry, date, elderberry]
        
        // First and last elements
        System.out.println("First word: " + words.first());
        System.out.println("Last word: " + words.last());
        
        // Range operations
        SortedSet<String> headSet = words.headSet("cherry");
        System.out.println("Words before 'cherry': " + headSet); // [apple, banana]
        
        SortedSet<String> tailSet = words.tailSet("cherry");
        System.out.println("Words from 'cherry': " + tailSet); // [cherry, date, elderberry]
        
        SortedSet<String> subSet = words.subSet("banana", "elderberry");
        System.out.println("Words between 'banana' and 'elderberry': " + subSet);
        // [banana, cherry, date]
        
        // Custom comparator
        SortedSet<String> lengthSorted = new TreeSet<>(
            Comparator.comparing(String::length).thenComparing(String::compareTo)
        );
        lengthSorted.addAll(words);
        
        System.out.println("Length sorted: " + lengthSorted);
        
        // Version number sorting
        SortedSet<Version> versions = new TreeSet<>();
        versions.add(new Version(1, 0, 0));
        versions.add(new Version(1, 2, 1));
        versions.add(new Version(1, 1, 0));
        versions.add(new Version(2, 0, 0));
        versions.add(new Version(1, 2, 0));
        
        System.out.println("Sorted versions:");
        for (Version version : versions) {
            System.out.println(version);
        }
        
        // Range queries on versions
        Version minVersion = new Version(1, 1, 0);
        Version maxVersion = new Version(2, 0, 0);
        
        SortedSet<Version> compatibleVersions = versions.subSet(minVersion, maxVersion);
        System.out.println("Compatible versions [1.1.0, 2.0.0): " + compatibleVersions);
        
        // Student grade management
        SortedSet<Student> studentsByGrade = new TreeSet<>(
            Comparator.comparing(Student::getGrade).reversed()
                     .thenComparing(Student::getName)
        );
        
        studentsByGrade.add(new Student("Alice", 85));
        studentsByGrade.add(new Student("Bob", 92));
        studentsByGrade.add(new Student("Charlie", 78));
        studentsByGrade.add(new Student("David", 92));
        studentsByGrade.add(new Student("Eve", 88));
        
        System.out.println("\nStudents by grade (descending):");
        for (Student student : studentsByGrade) {
            System.out.println(student);
        }
        
        // Top performers
        Student cutoff = new Student("", 85);
        SortedSet<Student> topPerformers = studentsByGrade.headSet(cutoff);
        System.out.println("Top performers (grade >= 85): " + topPerformers);
        
        // Date range example
        SortedSet<Date> importantDates = new TreeSet<>();
        Calendar cal = Calendar.getInstance();
        
        cal.set(2025, Calendar.JANUARY, 15);
        importantDates.add(cal.getTime());
        
        cal.set(2025, Calendar.MARCH, 10);
        importantDates.add(cal.getTime());
        
        cal.set(2025, Calendar.FEBRUARY, 1);
        importantDates.add(cal.getTime());
        
        cal.set(2025, Calendar.APRIL, 20);
        importantDates.add(cal.getTime());
        
        System.out.println("\nImportant dates (chronological):");
        for (Date date : importantDates) {
            System.out.println(String.format("%tB %te, %tY", date, date, date));
        }
        
        // First quarter dates
        cal.set(2025, Calendar.MARCH, 31);
        Date endQ1 = cal.getTime();
        SortedSet<Date> q1Dates = importantDates.headSet(endQ1);
        
        System.out.println("Q1 dates:");
        for (Date date : q1Dates) {
            System.out.println(String.format("%tB %te, %tY", date, date, date));
        }
    }
    
    static class Version implements Comparable<Version> {
        private int major, minor, patch;
        
        public Version(int major, int minor, int patch) {
            this.major = major;
            this.minor = minor;
            this.patch = patch;
        }
        
        @Override
        public int compareTo(Version other) {
            int result = Integer.compare(this.major, other.major);
            if (result != 0) return result;
            
            result = Integer.compare(this.minor, other.minor);
            if (result != 0) return result;
            
            return Integer.compare(this.patch, other.patch);
        }
        
        @Override
        public String toString() {
            return String.format("%d.%d.%d", major, minor, patch);
        }
        
        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (!(o instanceof Version)) return false;
            Version version = (Version) o;
            return major == version.major && minor == version.minor && patch == version.patch;
        }
        
        @Override
        public int hashCode() {
            return Objects.hash(major, minor, patch);
        }
    }
    
    static class Student {
        private String name;
        private int grade;
        
        public Student(String name, int grade) {
            this.name = name;
            this.grade = grade;
        }
        
        public String getName() { return name; }
        public int getGrade() { return grade; }
        
        @Override
        public String toString() {
            return String.format("%s: %d", name, grade);
        }
    }
}
```

---

## 🔚 Comprehensive Comparison Table

| Set Type              | Order Maintained      | Sorted | Thread Safe | Null Elements | Performance     | Use Case |
|-----------------------|-----------------------|--------|-------------|---------------|-----------------|----------|
| HashSet               | ❌                    | ❌     | ❌          | ✅ (one)      | O(1) avg        | General purpose, deduplication |
| LinkedHashSet         | ✅ (Insertion)        | ❌     | ❌          | ✅ (one)      | O(1) avg        | Ordered deduplication |
| TreeSet               | ✅ (Sorted)           | ✅     | ❌          | ❌            | O(log n)        | Sorted data, range queries |
| EnumSet               | ✅ (Enum Order)       | ✅     | ❌          | ❌            | O(1)            | Enum flags, permissions |
| ConcurrentSkipListSet | ✅ (Sorted)           | ✅     | ✅          | ❌            | O(log n)        | Concurrent sorted operations |
| CopyOnWriteArraySet   | ✅ (Insertion)        | ❌     | ✅          | ✅ (one)      | Read: O(1), Write: O(n) | Read-heavy concurrent scenarios |

---

## 🎯 Best Practices and Advanced Patterns

### 1. Choosing the Right Set

```java
// General purpose deduplication
Set<String> uniqueItems = new HashSet<>();

// Need to maintain insertion order
Set<String> orderedItems = new LinkedHashSet<>();

// Need sorted order or range operations
Set<Integer> sortedNumbers = new TreeSet<>();

// Working with enums (most efficient)
Set<DayOfWeek> workingDays = EnumSet.of(MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY);

// Thread-safe operations
Set<String> concurrentSet = ConcurrentHashMap.newKeySet(); // Or ConcurrentSkipListSet for sorted
```

### 2. Performance Optimization

```java
// Pre-size sets when you know the approximate size
Set<String> largeSet = new HashSet<>(10000);

// Use appropriate initial capacity and load factor
Set<String> optimizedSet = new HashSet<>(1000, 0.9f);

// For enum sets, always prefer EnumSet
Set<Status> statuses = EnumSet.noneOf(Status.class); // Much faster than HashSet
```

### 3. Set Operations

```java
public class SetOperations {
    public static <T> Set<T> union(Set<T> set1, Set<T> set2) {
        Set<T> result = new HashSet<>(set1);
        result.addAll(set2);
        return result;
    }
    
    public static <T> Set<T> intersection(Set<T> set1, Set<T> set2) {
        Set<T> result = new HashSet<>(set1);
        result.retainAll(set2);
        return result;
    }
    
    public static <T> Set<T> difference(Set<T> set1, Set<T> set2) {
        Set<T> result = new HashSet<>(set1);
        result.removeAll(set2);
        return result;
    }
    
    public static <T> Set<T> symmetricDifference(Set<T> set1, Set<T> set2) {
        Set<T> result = new HashSet<>(set1);
        Set<T> temp = new HashSet<>(set2);
        temp.removeAll(set1);
        result.removeAll(set2);
        result.addAll(temp);
        return result;
    }
    
    public static void main(String[] args) {
        Set<Integer> evens = Set.of(2, 4, 6, 8, 10);
        Set<Integer> primes = Set.of(2, 3, 5, 7, 11);
        
        System.out.println("Union: " + union(evens, primes));
        System.out.println("Intersection: " + intersection(evens, primes));
        System.out.println("Difference: " + difference(evens, primes));
        System.out.println("Symmetric Difference: " + symmetricDifference(evens, primes));
    }
}
```

### 4. Custom Objects in Sets

```java
public class Product {
    private String id;
    private String name;
    private double price;
    
    public Product(String id, String name, double price) {
        this.id = id;
        this.name = name;
        this.price = price;
    }
    
    // Essential for HashSet and LinkedHashSet
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Product)) return false;
        Product product = (Product) o;
        return Objects.equals(id, product.id);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(id);
    }
    
    // For TreeSet natural ordering
    public static class Product implements Comparable<Product> {
        // ... fields and constructor
        
        @Override
        public int compareTo(Product other) {
            return this.id.compareTo(other.id);
        }
    }
    
    // Custom comparators for TreeSet
    public static final Comparator<Product> BY_PRICE = 
        Comparator.comparing(Product::getPrice);
    
    public static final Comparator<Product> BY_NAME = 
        Comparator.comparing(Product::getName);
    
    public static final Comparator<Product> BY_PRICE_THEN_NAME = 
        Comparator.comparing(Product::getPrice).thenComparing(Product::getName);
}
```

### 5. Thread-Safe Patterns

```java
// Synchronized wrapper (not recommended for performance)
Set<String> synchronizedSet = Collections.synchronizedSet(new HashSet<>());

// Better: Use ConcurrentHashMap's keySet
Set<String> concurrentSet = ConcurrentHashMap.newKeySet();

// For sorted concurrent sets
NavigableSet<String> concurrentSortedSet = new ConcurrentSkipListSet<>();

// Copy-on-write for read-heavy scenarios
Set<String> readHeavySet = new CopyOnWriteArraySet<>();
```

### 6. Iteration Safety

```java
// Safe iteration with Iterator for modification
Set<String> set = new HashSet<>(Arrays.asList("a", "b", "c", "d"));
Iterator<String> iterator = set.iterator();
while (iterator.hasNext()) {
    String item = iterator.next();
    if (item.equals("b")) {
        iterator.remove(); // Safe removal
    }
}

// Stream-based filtering (creates new set)
Set<String> filtered = set.stream()
    .filter(s -> !s.equals("c"))
    .collect(Collectors.toSet());

// Concurrent modification safe with CopyOnWriteArraySet
CopyOnWriteArraySet<String> cowSet = new CopyOnWriteArraySet<>(Arrays.asList("a", "b", "c"));
for (String item : cowSet) {
    if (item.equals("b")) {
        cowSet.add("d"); // Safe to modify during iteration
    }
}
```

---

## 📚 Summary and Guidelines

### When to Use Each Set Type:

**HashSet:**
- General-purpose set operations
- Fast lookups and insertions
- When order doesn't matter
- Removing duplicates from collections

**LinkedHashSet:**
- Need to maintain insertion order
- LRU cache implementations
- Ordered processing of unique elements

**TreeSet:**
- Need sorted order
- Range queries (elements between values)
- Navigation operations (find next/previous)
- Custom sorting requirements

**EnumSet:**
- Working with enum values
- Permission/flag systems
- State management
- Maximum performance with enums

**ConcurrentSkipListSet:**
- Thread-safe sorted operations
- Concurrent range queries
- Lock-free sorted collections

**CopyOnWriteArraySet:**
- Read-heavy scenarios
- Observer patterns
- Event listener management
- Infrequent modifications

### Performance Considerations:

1. **EnumSet** is fastest for enum elements
2. **HashSet** is fastest for general objects
3. **TreeSet** for sorted access with O(log n) operations
4. **ConcurrentSkipListSet** for thread-safe sorted operations
5. **CopyOnWriteArraySet** for read-heavy concurrent scenarios

### Memory Considerations:

1. **EnumSet** uses bit vectors (most memory efficient for enums)
2. **HashSet** has overhead for hash table
3. **LinkedHashSet** has additional overhead for maintaining order
4. **TreeSet** has tree node overhead
5. **CopyOnWriteArraySet** can use significant memory due to copying

Choose based on your specific requirements for ordering, thread safety, performance, and functionality needs.
