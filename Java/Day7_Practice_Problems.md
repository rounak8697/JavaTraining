# Day 7: Practice Problems - Exception Handling & Arrays

## 🎯 Practice Objectives

By completing these practice problems, you will be able to:
- Implement `try-catch-finally` blocks to handle exceptions.
- Create and use custom exceptions.
- Manipulate data using arrays.
- Perform common operations on `ArrayList` and `LinkedList`.

---

## 🧩 Practice Problem 1: Custom Exception for User Validation

### Problem Statement
Create a user registration system that validates a user's age.
1.  Create a custom exception class named `InvalidAgeException` that extends `Exception`.
2.  Create a `UserRegistration` class with a method `registerUser(String name, int age)`.
3.  This method should throw an `InvalidAgeException` if the age is less than 18.
4.  In the `main` method, call `registerUser` with both valid and invalid ages, and handle the exception using a `try-catch` block.

### Starter Code
```java
// Create your InvalidAgeException class here

class UserRegistration {
    public void registerUser(String name, int age) /* Add throws declaration */ {
        // Add validation logic here
    }
}

public class RegistrationTest {
    public static void main(String[] args) {
        UserRegistration registration = new UserRegistration();

        // Test with a valid age
        try {
            registration.registerUser("Alice", 25);
            System.out.println("User Alice registered successfully.");
        } catch (InvalidAgeException e) {
            System.out.println("Error registering Alice: " + e.getMessage());
        }

        // Test with an invalid age
        try {
            registration.registerUser("Bob", 16);
            System.out.println("User Bob registered successfully.");
        } catch (InvalidAgeException e) {
            System.out.println("Error registering Bob: " + e.getMessage());
        }
    }
}
```

### Expected Output
```
User Alice registered successfully.
Error registering Bob: Age must be 18 or older.
```

### ✅ Solution
<details>
<summary>Solution</summary>

```java
// Custom exception class
class InvalidAgeException extends Exception {
    public InvalidAgeException(String message) {
        super(message);
    }
}

class UserRegistration {
    public void registerUser(String name, int age) throws InvalidAgeException {
        if (age < 18) {
            throw new InvalidAgeException("Age must be 18 or older.");
        }
        System.out.println("Registering user: " + name);
    }
}

public class RegistrationTest {
    public static void main(String[] args) {
        UserRegistration registration = new UserRegistration();

        // Test with a valid age
        try {
            registration.registerUser("Alice", 25);
            System.out.println("User Alice registered successfully.");
        } catch (InvalidAgeException e) {
            System.out.println("Error registering Alice: " + e.getMessage());
        }

        // Test with an invalid age
        try {
            registration.registerUser("Bob", 16);
            System.out.println("User Bob registered successfully.");
        } catch (InvalidAgeException e) {
            System.out.println("Error registering Bob: " + e.getMessage());
        }
    }
}
```
</details>

---

## 🧩 Practice Problem 2: Array Data Analyzer

### Problem Statement
Write a program that takes an array of integers and calculates the following statistics:
1.  The minimum value.
2.  The maximum value.
3.  The average value.
4.  The sum of all values.

The program should handle an empty array gracefully by catching the `ArrayIndexOutOfBoundsException` and printing an informative message.

### Starter Code
```java
public class ArrayAnalyzer {
    public static void analyze(int[] data) {
        try {
            // Your analysis code here
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Error: The array is empty. Cannot perform analysis.");
        }
    }

    public static void main(String[] args) {
        int[] numbers = {15, 25, 3, 42, 18, 5};
        System.out.println("Analyzing a non-empty array:");
        analyze(numbers);

        System.out.println("\nAnalyzing an empty array:");
        int[] emptyArray = {};
        analyze(emptyArray);
    }
}
```

### Expected Output
```
Analyzing a non-empty array:
Minimum: 3
Maximum: 42
Sum: 108
Average: 18.0

Analyzing an empty array:
Error: The array is empty. Cannot perform analysis.
```

### ✅ Solution
<details>
<summary>Solution</summary>

```java
import java.util.Arrays;

public class ArrayAnalyzer {
    public static void analyze(int[] data) {
        if (data == null || data.length == 0) {
            System.out.println("Error: The array is empty or null. Cannot perform analysis.");
            return;
        }

        int min = data[0];
        int max = data[0];
        int sum = 0;

        for (int num : data) {
            if (num < min) {
                min = num;
            }
            if (num > max) {
                max = num;
            }
            sum += num;
        }

        double average = (double) sum / data.length;

        System.out.println("Minimum: " + min);
        System.out.println("Maximum: " + max);
        System.out.println("Sum: " + sum);
        System.out.println("Average: " + average);
    }

    public static void main(String[] args) {
        int[] numbers = {15, 25, 3, 42, 18, 5};
        System.out.println("Analyzing a non-empty array:");
        analyze(numbers);

        System.out.println("\nAnalyzing an empty array:");
        int[] emptyArray = {};
        analyze(emptyArray);
    }
}
```
</details>