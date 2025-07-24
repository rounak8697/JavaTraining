# Day 1: Java Fundamentals & Operators

## Session 1: Introduction to Java (45 minutes)

### 1. Introduction to Java
Java is a high-level, object-oriented programming language developed by Sun Microsystems (now Oracle).

**Key Features:**
- Platform Independent (Write Once, Run Anywhere)
- Object-Oriented
- Secure
- Robust
- Multithreaded
- Simple

### 2. Concept of OOPS (Object-Oriented Programming)

**Four Pillars of OOP:**
1. **Encapsulation**: Wrapping data and methods together
2. **Inheritance**: Acquiring properties from parent class
3. **Polymorphism**: One interface, multiple implementations
4. **Abstraction**: Hiding implementation details

### 3. Java Virtual Machine (JVM) Basics

**JVM Architecture:**
```
Source Code (.java) → Compiler (javac) → Bytecode (.class) → JVM → Machine Code
```

**Components:**
- **Class Loader**: Loads class files
- **Memory Area**: Heap, Stack, Method Area
- **Execution Engine**: Interprets bytecode

### 4. Configuration & Installation

**Steps:**
1. Download JDK from Oracle website
2. Set JAVA_HOME environment variable
3. Add Java bin directory to PATH
4. Verify installation: `java -version`

### 5. First Java Program: Hello World

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

**Compilation and Execution:**
```bash
javac HelloWorld.java
java HelloWorld
```

### 6. Variables in Java

**Types of Variables:**
- **Local Variables**: Declared inside methods
- **Instance Variables**: Declared inside class but outside methods
- **Static Variables**: Declared with static keyword

```java
public class VariableExample {
    static int staticVar = 10;    // Static variable
    int instanceVar = 20;         // Instance variable
    
    public void method() {
        int localVar = 30;        // Local variable
    }
}
```

### 7. Java Data Types

**Primitive Data Types:**
- **byte**: 8-bit (-128 to 127)
- **short**: 16-bit (-32,768 to 32,767)
- **int**: 32-bit (-2^31 to 2^31-1)
- **long**: 64-bit (-2^63 to 2^63-1)
- **float**: 32-bit floating point
- **double**: 64-bit floating point
- **char**: 16-bit Unicode character
- **boolean**: true or false

**Non-Primitive Data Types:**
- String, Arrays, Classes, Interfaces

```java
public class DataTypesExample {
    public static void main(String[] args) {
        byte b = 100;
        short s = 1000;
        int i = 100000;
        long l = 100000L;
        float f = 10.5f;
        double d = 10.5;
        char c = 'A';
        boolean bool = true;
        String str = "Hello";
    }
}
```

### 8. Casting (Type Conversion)

**Implicit Casting (Widening):**
```java
int i = 100;
long l = i;    // Automatic conversion
double d = l;  // Automatic conversion
```

**Explicit Casting (Narrowing):**
```java
double d = 100.04;
long l = (long) d;    // Explicit casting
int i = (int) l;      // Explicit casting
```

---

## Session 2: Java Operators (45 minutes)

### 1. Types of Operators

#### Arithmetic Operators
```java
int a = 10, b = 3;
System.out.println(a + b);  // Addition: 13
System.out.println(a - b);  // Subtraction: 7
System.out.println(a * b);  // Multiplication: 30
System.out.println(a / b);  // Division: 3
System.out.println(a % b);  // Modulus: 1
```

#### Assignment Operators
```java
int a = 10;
a += 5;  // a = a + 5 = 15
a -= 3;  // a = a - 3 = 12
a *= 2;  // a = a * 2 = 24
a /= 4;  // a = a / 4 = 6
a %= 5;  // a = a % 5 = 1
```

#### Comparison Operators
```java
int a = 10, b = 20;
System.out.println(a == b);  // Equal to: false
System.out.println(a != b);  // Not equal: true
System.out.println(a > b);   // Greater than: false
System.out.println(a < b);   // Less than: true
System.out.println(a >= b);  // Greater than or equal: false
System.out.println(a <= b);  // Less than or equal: true
```

#### Logical Operators
```java
boolean x = true, y = false;
System.out.println(x && y);  // Logical AND: false
System.out.println(x || y);  // Logical OR: true
System.out.println(!x);      // Logical NOT: false
```

#### Unary Operators
```java
int a = 10;
System.out.println(++a);  // Pre-increment: 11
System.out.println(a++);  // Post-increment: 11 (then becomes 12)
System.out.println(--a);  // Pre-decrement: 11
System.out.println(a--);  // Post-decrement: 11 (then becomes 10)
```

#### Bitwise Operators
```java
int a = 5, b = 3;  // a = 101, b = 011 in binary
System.out.println(a & b);  // AND: 1
System.out.println(a | b);  // OR: 7
System.out.println(a ^ b);  // XOR: 6
System.out.println(~a);     // NOT: -6
System.out.println(a << 1); // Left shift: 10
System.out.println(a >> 1); // Right shift: 2
```

#### Ternary Operator
```java
int a = 10, b = 20;
int max = (a > b) ? a : b;  // max = 20
String result = (a % 2 == 0) ? "Even" : "Odd";  // result = "Even"
```

### 2. Operator Precedence
1. Postfix: expr++, expr--
2. Unary: ++expr, --expr, +expr, -expr, ~, !
3. Multiplicative: *, /, %
4. Additive: +, -
5. Shift: <<, >>, >>>
6. Relational: <, >, <=, >=, instanceof
7. Equality: ==, !=
8. Bitwise AND: &
9. Bitwise XOR: ^
10. Bitwise OR: |
11. Logical AND: &&
12. Logical OR: ||
13. Ternary: ? :
14. Assignment: =, +=, -=, *=, /=, %=

### 3. Expressions and Operands

**Expression**: Combination of variables, operators, and method calls
**Operand**: Values on which operators work

```java
int result = (a + b) * c / d;  // Expression with multiple operands
```

---

## Practice Problems for Day 1

### Problem 1: Basic Calculator
Create a Java program that performs basic arithmetic operations on two numbers.

### Problem 2: Data Type Conversion
Write a program that demonstrates all types of casting with examples.

### Problem 3: Operator Precedence
Create a program that shows the result of complex expressions and explains operator precedence.

---

## Key Takeaways
1. Java is platform-independent and object-oriented
2. JVM converts bytecode to machine code
3. Variables have different scopes and types
4. Java has 8 primitive data types
5. Casting can be implicit or explicit
6. Operators have specific precedence rules
7. Understanding operator precedence is crucial for complex expressions
