# Day 1: Practice Problems

## Problem 1: Basic Calculator
**Difficulty**: Beginner
**Time**: 15 minutes
**Concepts**: Variables, Data Types, Operators

### Problem Statement
Create a Java program that acts as a basic calculator. The program should:
1. Take two numbers as input from the user
2. Perform all arithmetic operations (+, -, *, /, %)
3. Display the results with proper formatting

### Expected Output
```
Enter first number: 15
Enter second number: 4

Results:
15 + 4 = 19
15 - 4 = 11
15 * 4 = 60
15 / 4 = 3.75
15 % 4 = 3
```

### Solution
```java
import java.util.Scanner;

public class BasicCalculator {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.print("Enter first number: ");
        double num1 = scanner.nextDouble();
        
        System.out.print("Enter second number: ");
        double num2 = scanner.nextDouble();
        
        System.out.println("\nResults:");
        System.out.println(num1 + " + " + num2 + " = " + (num1 + num2));
        System.out.println(num1 + " - " + num2 + " = " + (num1 - num2));
        System.out.println(num1 + " * " + num2 + " = " + (num1 * num2));
        
        if (num2 != 0) {
            System.out.println(num1 + " / " + num2 + " = " + (num1 / num2));
            System.out.println(num1 + " % " + num2 + " = " + (num1 % num2));
        } else {
            System.out.println("Division by zero is not allowed!");
        }
        
        scanner.close();
    }
}
```

---

## Problem 2: Data Type Showcase
**Difficulty**: Beginner
**Time**: 20 minutes
**Concepts**: Data Types, Casting, Variables

### Problem Statement
Create a Java program that demonstrates:
1. All primitive data types with their ranges
2. Implicit and explicit casting examples
3. Variable scope (local, instance, static)

### Expected Output
```
=== Primitive Data Types ===
byte: -128 to 127, Current value: 100
short: -32768 to 32767, Current value: 1000
int: -2147483648 to 2147483647, Current value: 100000
long: -9223372036854775808 to 9223372036854775807, Current value: 1000000
float: 1.4E-45 to 3.4028235E38, Current value: 10.5
double: 4.9E-324 to 1.7976931348623157E308, Current value: 20.99
char: 0 to 65535, Current value: A
boolean: true or false, Current value: true

=== Casting Examples ===
Implicit Casting: int 100 to long 100
Explicit Casting: double 99.99 to int 99
```

### Solution
```java
public class DataTypeShowcase {
    static int staticVar = 500;  // Static variable
    int instanceVar = 300;       // Instance variable
    
    public static void main(String[] args) {
        int localVar = 100;      // Local variable
        
        // Primitive data types
        byte b = 100;
        short s = 1000;
        int i = 100000;
        long l = 1000000L;
        float f = 10.5f;
        double d = 20.99;
        char c = 'A';
        boolean bool = true;
        
        System.out.println("=== Primitive Data Types ===");
        System.out.println("byte: " + Byte.MIN_VALUE + " to " + Byte.MAX_VALUE + ", Current value: " + b);
        System.out.println("short: " + Short.MIN_VALUE + " to " + Short.MAX_VALUE + ", Current value: " + s);
        System.out.println("int: " + Integer.MIN_VALUE + " to " + Integer.MAX_VALUE + ", Current value: " + i);
        System.out.println("long: " + Long.MIN_VALUE + " to " + Long.MAX_VALUE + ", Current value: " + l);
        System.out.println("float: " + Float.MIN_VALUE + " to " + Float.MAX_VALUE + ", Current value: " + f);
        System.out.println("double: " + Double.MIN_VALUE + " to " + Double.MAX_VALUE + ", Current value: " + d);
        System.out.println("char: " + (int)Character.MIN_VALUE + " to " + (int)Character.MAX_VALUE + ", Current value: " + c);
        System.out.println("boolean: true or false, Current value: " + bool);
        
        System.out.println("\n=== Casting Examples ===");
        // Implicit casting
        long implicitLong = i;
        System.out.println("Implicit Casting: int " + i + " to long " + implicitLong);
        
        // Explicit casting
        double originalDouble = 99.99;
        int explicitInt = (int) originalDouble;
        System.out.println("Explicit Casting: double " + originalDouble + " to int " + explicitInt);
        
        System.out.println("\n=== Variable Scope ===");
        System.out.println("Local Variable: " + localVar);
        System.out.println("Static Variable: " + staticVar);
        
        DataTypeShowcase obj = new DataTypeShowcase();
        System.out.println("Instance Variable: " + obj.instanceVar);
    }
}
```

---

## Problem 3: Operator Precedence Challenge
**Difficulty**: Intermediate
**Time**: 25 minutes
**Concepts**: Operators, Precedence, Expressions

### Problem Statement
Create a Java program that:
1. Evaluates complex expressions and shows step-by-step calculation
2. Demonstrates operator precedence rules
3. Uses all types of operators (arithmetic, logical, bitwise, ternary)

### Expected Output
```
=== Operator Precedence Challenge ===

Expression 1: 2 + 3 * 4
Step-by-step: 2 + (3 * 4) = 2 + 12 = 14
Result: 14

Expression 2: 10 > 5 && 3 < 8 || false
Step-by-step: (10 > 5) && (3 < 8) || false = true && true || false = true || false = true
Result: true

Expression 3: 5 & 3 | 2
Step-by-step: (5 & 3) | 2 = 1 | 2 = 3
Result: 3

Ternary Operation: Is 15 even? false
```

### Solution
```java
public class OperatorPrecedenceChallenge {
    public static void main(String[] args) {
        System.out.println("=== Operator Precedence Challenge ===\n");
        
        // Expression 1: Arithmetic operators
        int expr1 = 2 + 3 * 4;
        System.out.println("Expression 1: 2 + 3 * 4");
        System.out.println("Step-by-step: 2 + (3 * 4) = 2 + 12 = 14");
        System.out.println("Result: " + expr1 + "\n");
        
        // Expression 2: Logical operators
        boolean expr2 = 10 > 5 && 3 < 8 || false;
        System.out.println("Expression 2: 10 > 5 && 3 < 8 || false");
        System.out.println("Step-by-step: (10 > 5) && (3 < 8) || false = true && true || false = true || false = true");
        System.out.println("Result: " + expr2 + "\n");
        
        // Expression 3: Bitwise operators
        int expr3 = 5 & 3 | 2;
        System.out.println("Expression 3: 5 & 3 | 2");
        System.out.println("Step-by-step: (5 & 3) | 2 = 1 | 2 = 3");
        System.out.println("Binary: 101 & 011 | 010 = 001 | 010 = 011");
        System.out.println("Result: " + expr3 + "\n");
        
        // Ternary operator
        int number = 15;
        String result = (number % 2 == 0) ? "even" : "odd";
        System.out.println("Ternary Operation: Is " + number + " even? " + result + "\n");
        
        // Complex expression
        int a = 10, b = 5, c = 3;
        int complex = a + b * c / 2 - 1;
        System.out.println("Complex Expression: " + a + " + " + b + " * " + c + " / 2 - 1");
        System.out.println("Step-by-step: 10 + (5 * 3) / 2 - 1 = 10 + 15 / 2 - 1 = 10 + 7 - 1 = 16");
        System.out.println("Result: " + complex + "\n");
        
        // Increment/Decrement operators
        int x = 5;
        System.out.println("Increment/Decrement Demo:");
        System.out.println("Initial x: " + x);
        System.out.println("x++: " + (x++));  // Post-increment
        System.out.println("x after post-increment: " + x);
        System.out.println("++x: " + (++x));  // Pre-increment
        System.out.println("x after pre-increment: " + x);
    }
}
```

---

## Learning Objectives Achieved
After completing these problems, students will understand:

1. **Basic Calculator**: 
   - How to use Scanner for input
   - Arithmetic operations implementation
   - Handling division by zero

2. **Data Type Showcase**:
   - All primitive data types and their ranges
   - Difference between implicit and explicit casting
   - Variable scope concepts

3. **Operator Precedence Challenge**:
   - How operator precedence affects expression evaluation
   - Step-by-step expression breakdown
   - Complex expression handling

## Additional Practice Suggestions
1. Create a program that converts temperature between Celsius and Fahrenheit
2. Build a simple interest calculator using arithmetic operators
3. Write a program that checks if a number is within a specific range using logical operators
