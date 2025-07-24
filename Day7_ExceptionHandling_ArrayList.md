# Day 7: Exception Handling & Arrays

## 🗺️ Learning Roadmap

```
Day 1-6 (Core OOP) → Day 7 (Exception Handling & Arrays) → Day 8 (Collections Framework)
                                 ↓
                            You are here
```

## 🎯 Learning Objectives

By the end of this session, you will be able to:
- Understand and handle exceptions using `try-catch-finally` blocks.
- Create and throw custom exceptions.
- Differentiate between checked and unchecked exceptions.
- Understand the use of the `final` keyword.
- Declare, initialize, and manipulate arrays.
- Use `ArrayList` and `LinkedList` for dynamic data storage.
- Understand the trade-offs between `ArrayList` and `LinkedList`.

## ⏱️ Time Allocation
- Exception Handling & `final` keyword (45 minutes)
- Arrays & Lists (45 minutes)

---

## Session 1: Exception Handling & `final` Keyword (45 minutes)

### 1. Java Exception Handling

#### What is an Exception?
An exception is an event that disrupts the normal flow of a program. When an error occurs within a method, the method creates an exception object and hands it off to the runtime system.

**Exception Hierarchy:**
```
                Throwable
                /       \
             Error     Exception
                         /      \
                  Checked     Unchecked (RuntimeException)
                  (e.g., IOException)  (e.g., NullPointerException)
```

#### Why Handle Exceptions?
- To maintain the normal flow of the application.
- To prevent the program from terminating abruptly.
- To provide meaningful error messages to the user.

### 2. The `try-catch` Block

The `try-catch` block is used to enclose code that might throw an exception and handle it.

```java
public class TryCatchExample {
    public static void main(String[] args) {
        try {
            // Code that may throw an exception
            int result = 10 / 0; 
            System.out.println("This will not be printed.");
        } catch (ArithmeticException e) {
            // Code to handle the exception
            System.out.println("Error: Cannot divide by zero.");
            System.out.println("Exception details: " + e.getMessage());
        }
        System.out.println("Program continues after handling the exception.");
    }
}
```

**Multiple Catch Blocks:** You can use multiple `catch` blocks to handle different types of exceptions.

```java
try {
    int[] a = new int[5];
    a[5] = 30 / 0; // Generates two exceptions
} catch (ArithmeticException e) {
    System.out.println("Arithmetic Exception occurred.");
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Array Index Out Of Bounds Exception occurred.");
} catch (Exception e) {
    System.out.println("Parent Exception occurred.");
}
```

### 3. The `finally` Block

The `finally` block is always executed, whether an exception is handled or not. It's used for cleanup activities like closing files or database connections.

```java
public class FinallyExample {
    public static void main(String[] args) {
        try {
            int data = 25 / 5;
            System.out.println(data);
        } catch (NullPointerException e) {
            System.out.println(e);
        } finally {
            System.out.println("The 'finally' block is always executed.");
        }
        System.out.println("Rest of the code...");
    }
}
```

### 4. The `throw` and `throws` Keywords

#### `throw`
The `throw` keyword is used to explicitly throw an exception from a method or a block of code.

```java
public class ThrowExample {
    public static void validate(int age) {
        if (age < 18) {
            // Throw an instance of ArithmeticException
            throw new ArithmeticException("Person is not eligible to vote");
        } else {
            System.out.println("Person is eligible to vote");
        }
    }

    public static void main(String[] args) {
        try {
            validate(13);
        } catch (ArithmeticException e) {
            System.out.println("Caught exception: " + e.getMessage());
        }
    }
}
```

#### `throws`
The `throws` keyword is used in a method signature to declare the exceptions that might be thrown by the method.

```java
import java.io.IOException;

class ThrowsExample {
    // Declare that this method can throw an IOException
    void myMethod() throws IOException {
        throw new IOException("device error");
    }

    public static void main(String[] args) {
        ThrowsExample obj = new ThrowsExample();
        try {
            obj.myMethod();
        } catch (IOException e) {
            System.out.println("Exception handled: " + e.getMessage());
        }
    }
}
```

### 5. Custom Exceptions

You can create your own exception classes by extending the `Exception` class.

```java
// Custom exception class
class InvalidAgeException extends Exception {
    public InvalidAgeException(String message) {
        super(message);
    }
}

public class CustomExceptionTest {
    static void validate(int age) throws InvalidAgeException {
        if (age < 18) {
            throw new InvalidAgeException("Age is not valid to vote");
        }
    }

    public static void main(String[] args) {
        try {
            validate(13);
        } catch (InvalidAgeException ex) {
            System.out.println("Caught the exception: " + ex.getMessage());
        }
    }
}
```

### 6. The `final` Keyword

The `final` keyword is a non-access modifier used for classes, methods, and variables.

- **Final Variable**: Makes the variable a constant. Its value cannot be changed.
  ```java
  final int MAX_VALUE = 100;
  // MAX_VALUE = 150; // This would cause a compile error
  ```

- **Final Method**: Prevents the method from being overridden by a subclass.
  ```java
  class Base {
      final void show() {
          System.out.println("This is a final method.");
      }
  }
  class Derived extends Base {
      // void show() { } // Compile error: cannot override final method
  }
  ```

- **Final Class**: Prevents the class from being extended (inherited).
  ```java
  final class FinalClass {
      // ...
  }
  // class SubClass extends FinalClass { } // Compile error
  ```

---

## Session 2: Arrays & Lists (45 minutes)

### 1. Arrays

An array is a container object that holds a fixed number of values of a single type. The length of an array is established when the array is created.

#### Declaration and Initialization
```java
// Declaration
int[] anArray;

// Initialization with size
anArray = new int[10];

// Declaration and initialization in one line
String[] names = {"Alice", "Bob", "Charlie"};
```

#### Accessing Elements
Array elements are accessed by their index, starting from 0.
```java
int[] numbers = {10, 20, 30, 40, 50};
System.out.println("First element: " + numbers[0]); // 10
System.out.println("Third element: " + numbers[2]); // 30

numbers[1] = 25; // Modify an element
```

#### Iterating Over an Array
```java
String[] fruits = {"Apple", "Banana", "Orange"};

// Using a for loop
for (int i = 0; i < fruits.length; i++) {
    System.out.println(fruits[i]);
}

// Using an enhanced for-each loop
for (String fruit : fruits) {
    System.out.println(fruit);
}
```

### 2. `ArrayList`

`ArrayList` is a resizable array implementation from the Collections Framework. It provides dynamic arrays in Java.

#### Creating an `ArrayList`
```java
import java.util.ArrayList;
import java.util.List;

// Create an ArrayList of Strings
List<String> animals = new ArrayList<>();
```

#### Common `ArrayList` Operations
```java
List<String> animals = new ArrayList<>();

// Add elements
animals.add("Lion");
animals.add("Tiger");
animals.add("Bear");
System.out.println("ArrayList: " + animals); // [Lion, Tiger, Bear]

// Get an element
String firstAnimal = animals.get(0);
System.out.println("First animal: " + firstAnimal); // Lion

// Set an element
animals.set(1, "Zebra");
System.out.println("Modified ArrayList: " + animals); // [Lion, Zebra, Bear]

// Remove an element
animals.remove(2);
System.out.println("After removal: " + animals); // [Lion, Zebra]

// Get size
System.out.println("Size: " + animals.size()); // 2
```

### 3. `LinkedList`

`LinkedList` is another implementation of the `List` interface. It stores elements in a doubly-linked list structure. It's efficient for insertions and deletions.

#### Creating a `LinkedList`
```java
import java.util.LinkedList;
import java.util.List;

List<String> cars = new LinkedList<>();
```

#### Common `LinkedList` Operations
`LinkedList` supports all the standard `List` operations like `add`, `get`, `remove`, etc. It also provides additional methods for adding/removing from the beginning or end.

```java
LinkedList<String> cars = new LinkedList<>();
cars.add("Volvo");
cars.add("BMW");

// Add to the beginning
cars.addFirst("Ford");

// Add to the end
cars.addLast("Mazda");

System.out.println("LinkedList: " + cars); // [Ford, Volvo, BMW, Mazda]

// Remove from the beginning
cars.removeFirst();

// Remove from the end
cars.removeLast();

System.out.println("After removal: " + cars); // [Volvo, BMW]
```

### 4. `ArrayList` vs. `LinkedList`

The choice between `ArrayList` and `LinkedList` depends on the specific use case.

| Feature | `ArrayList` | `LinkedList` |
|---|---|---|
| **Internal Structure** | Dynamic Array | Doubly-Linked List |
| **Element Access (`get`)** | Fast (O(1)) | Slow (O(n)) |
| **Insertion/Deletion (Middle)** | Slow (O(n)) | Fast (O(1)) once node is found |
| **Insertion/Deletion (End)** | Fast (Amortized O(1)) | Fast (O(1)) |
| **Memory Overhead** | Lower | Higher (stores pointers) |

#### 🤔 When to use which?

<details>
<summary>Answer</summary>

*   **Use `ArrayList` when:**
    *   You have frequent random access operations (using `get(index)`).
    *   You have more read operations than write (insertion/deletion) operations.
    *   You are adding/removing elements mostly at the end of the list.

*   **Use `LinkedList` when:**
    *   You have frequent insertion and deletion operations, especially in the middle of the list.
    *   You don't need random access to elements.
    *   You need to use it as a Queue or Deque, as it implements these interfaces.

</details>

---

## 🔄 Recap

In this session, we covered:

1.  **Exception Handling**
    *   Using `try-catch-finally` to manage errors gracefully.
    *   Creating and using custom exceptions for application-specific errors.
    *   Understanding the difference between `throw` and `throws`.

2.  **`final` Keyword**
    *   Creating constants, final methods, and final classes.

3.  **Arrays and Lists**
    *   Using fixed-size arrays for simple data collections.
    *   Using `ArrayList` for fast random access and dynamic resizing.
    *   Using `LinkedList` for fast insertions and deletions.

## 🧩 Next Steps

In the next session, we'll dive deeper into the **Java Collections Framework**, exploring:
- Maps (`HashMap`, `TreeMap`, `LinkedHashMap`)
- Sets (`HashSet`, `TreeSet`, `LinkedHashSet`)
- Legacy collections and other important concepts.

