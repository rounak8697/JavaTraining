# Day 6: Interfaces & Packages

## 🗺️ Learning Roadmap

```
Day 1 (Basics) → Day 2 (Control Flow) → Day 3 (Keywords & Inheritance) → Day 4 (Abstraction & Polymorphism) → Day 5 (Methods & Abstract Classes) → Day 6 (Interfaces & Packages) → Day 7 (Exception Handling & Arrays)
                                                                                                                                                   ↓
                                                                                                                                              You are here
```

## 🎯 Learning Objectives

By the end of this session, you will be able to:
- Understand the purpose and benefits of interfaces
- Define and implement interfaces in Java
- Use default and static methods in interfaces
- Implement multiple interfaces
- Work with functional interfaces and lambda expressions
- Understand the purpose of packages in Java
- Create and use custom packages
- Use import statements effectively
- Apply access modifiers with packages
- Navigate the Java API packages

## ⏱️ Time Allocation
- Interfaces (45 minutes)
- Packages (45 minutes)

---

## Session 1: Interfaces (45 minutes)

### 1. Introduction to Interfaces

#### What is an Interface?

An interface in Java is a reference type, similar to a class, that can contain only constants, method signatures, default methods, static methods, and nested types. Interfaces cannot be instantiated—they can only be implemented by classes or extended by other interfaces.

#### 🔍 Key Characteristics

- Declared using the `interface` keyword
- Methods are implicitly `public` and `abstract` (prior to Java 8)
- Variables are implicitly `public`, `static`, and `final`
- Cannot be instantiated directly
- A class can implement multiple interfaces
- An interface can extend multiple interfaces

#### 📊 Visual Representation

```
┌───────────────────────────────────────────────────┐
│                                                   │
│                   INTERFACE                       │
│                                                   │
│  ┌─────────────────────────────────────────────┐  │
│  │ interface Drawable {                        │  │
│  │   // Constants                              │  │
│  │   int MAX_SIZE = 100;                       │  │
│  │                                             │  │
│  │   // Abstract methods                       │  │
│  │   void draw();                              │  │
│  │   void resize(double factor);               │  │
│  │                                             │  │
│  │   // Default method (Java 8+)               │  │
│  │   default void clear() {                    │  │
│  │     System.out.println("Clearing...");      │  │
│  │   }                                         │  │
│  │                                             │  │
│  │   // Static method (Java 8+)                │  │
│  │   static void info() {                      │  │
│  │     System.out.println("Drawable interface");│  │
│  │   }                                         │  │
│  │ }                                           │  │
│  └─────────────────────────────────────────────┘  │
│                       ▲                           │
│                       │                           │
│  ┌─────────────────────────────────────────────┐  │
│  │ class Circle implements Drawable {          │  │
│  │   @Override                                 │  │
│  │   public void draw() {                      │  │
│  │     // Implementation                       │  │
│  │   }                                         │  │
│  │                                             │  │
│  │   @Override                                 │  │
│  │   public void resize(double factor) {       │  │
│  │     // Implementation                       │  │
│  │   }                                         │  │
│  │ }                                           │  │
│  └─────────────────────────────────────────────┘  │
│                                                   │
└───────────────────────────────────────────────────┘
```

#### Purpose of Interfaces

Interfaces serve several important purposes in Java:

1. **Defining a Contract**: Interfaces define what a class can do without specifying how it does it.
2. **Enabling Polymorphism**: Objects of different classes can be treated as objects of a common interface type.
3. **Multiple Inheritance of Type**: Java doesn't support multiple inheritance of classes, but it does support multiple inheritance of interfaces.
4. **Decoupling**: Interfaces help reduce dependencies between components, making code more maintainable.
5. **API Design**: Interfaces are useful for defining stable APIs that can evolve over time.

### 2. Defining and Implementing Interfaces

#### Basic Interface Definition

```java
public interface Animal {
    // Constant (implicitly public, static, final)
    String CATEGORY = "Living Being";
    
    // Abstract methods (implicitly public and abstract)
    void eat();
    void sleep();
    void makeSound();
}
```

#### Implementing an Interface

```java
public class Dog implements Animal {
    @Override
    public void eat() {
        System.out.println("Dog is eating");
    }
    
    @Override
    public void sleep() {
        System.out.println("Dog is sleeping");
    }
    
    @Override
    public void makeSound() {
        System.out.println("Woof!");
    }
    
    // Class-specific method
    public void wagTail() {
        System.out.println("Dog is wagging its tail");
    }
}
```

#### Using Interface References

```java
public class Main {
    public static void main(String[] args) {
        // Using concrete class reference
        Dog dog = new Dog();
        dog.eat();
        dog.makeSound();
        dog.wagTail();  // Dog-specific method
        
        // Using interface reference
        Animal animal = new Dog();
        animal.eat();
        animal.makeSound();
        // animal.wagTail();  // Error: Animal doesn't have wagTail() method
        
        // Accessing interface constant
        System.out.println("Category: " + Animal.CATEGORY);
    }
}
```

#### 🤔 Think About It
How do interfaces differ from abstract classes?

<details>
<summary>Answer</summary>
<p>Key differences between interfaces and abstract classes:</p>
<ul>
<li><strong>Multiple Inheritance</strong>: A class can implement multiple interfaces but can extend only one abstract class.</li>
<li><strong>State</strong>: Interfaces cannot have instance variables (state), while abstract classes can.</li>
<li><strong>Constructor</strong>: Interfaces cannot have constructors, while abstract classes can.</li>
<li><strong>Method Implementation</strong>: Prior to Java 8, interfaces couldn't have method implementations. Abstract classes can have both abstract and concrete methods.</li>
<li><strong>Access Modifiers</strong>: Interface methods are implicitly public, while abstract class methods can have any access modifier.</li>
<li><strong>Variables</strong>: Interface variables are implicitly public, static, and final, while abstract class variables can have any access modifier and can be non-static and non-final.</li>
<li><strong>Purpose</strong>: Interfaces are designed for defining a contract, while abstract classes are designed for code reuse and partial implementation.</li>
</ul>
</details>

### 3. Default and Static Methods in Interfaces (Java 8+)

#### Default Methods

Default methods were introduced in Java 8 to enable interface evolution. They provide a default implementation that implementing classes can use, override, or ignore.

```java
public interface Vehicle {
    void start();
    void stop();
    
    // Default method
    default void honk() {
        System.out.println("Beep beep!");
    }
}

public class Car implements Vehicle {
    @Override
    public void start() {
        System.out.println("Car starting");
    }
    
    @Override
    public void stop() {
        System.out.println("Car stopping");
    }
    
    // No need to implement honk() - using the default implementation
}

public class Truck implements Vehicle {
    @Override
    public void start() {
        System.out.println("Truck starting");
    }
    
    @Override
    public void stop() {
        System.out.println("Truck stopping");
    }
    
    // Overriding the default method
    @Override
    public void honk() {
        System.out.println("HOOOONK!");
    }
}
```

#### Static Methods

Static methods in interfaces are similar to static methods in classes. They belong to the interface and cannot be overridden by implementing classes.

```java
public interface MathOperations {
    // Abstract method
    double calculate(double x, double y);
    
    // Static method
    static double square(double num) {
        return num * num;
    }
    
    // Static method
    static double cube(double num) {
        return num * num * num;
    }
}

public class Addition implements MathOperations {
    @Override
    public double calculate(double x, double y) {
        return x + y;
    }
}

public class Main {
    public static void main(String[] args) {
        // Using static methods
        System.out.println("Square of 5: " + MathOperations.square(5));
        System.out.println("Cube of 3: " + MathOperations.cube(3));
        
        // Using instance method
        MathOperations add = new Addition();
        System.out.println("5 + 3 = " + add.calculate(5, 3));
    }
}
```

#### 🤔 Think About It
Why were default and static methods added to interfaces in Java 8?

<details>
<summary>Answer</summary>
<p>Default and static methods were added to interfaces in Java 8 for several reasons:</p>
<ul>
<li><strong>Interface Evolution</strong>: Default methods allow new methods to be added to interfaces without breaking existing implementations. This enables interfaces to evolve over time.</li>
<li><strong>Backward Compatibility</strong>: They provide a way to add new functionality to existing interfaces without forcing all implementing classes to provide an implementation.</li>
<li><strong>Multiple Inheritance of Behavior</strong>: Default methods allow interfaces to provide behavior, not just type, enabling a form of multiple inheritance of behavior.</li>
<li><strong>Utility Methods</strong>: Static methods provide a place for utility methods that are related to the interface but don't require an instance.</li>
<li><strong>Functional Programming</strong>: They support functional programming constructs in Java, particularly for functional interfaces used with lambda expressions.</li>
</ul>
</details>

### 4. Multiple Interface Implementation

One of the key advantages of interfaces is that a class can implement multiple interfaces, which is not possible with class inheritance.

```java
// First interface
interface Swimmer {
    void swim();
    
    default void floatOnWater() {
        System.out.println("Floating on water");
    }
}

// Second interface
interface Flyer {
    void fly();
    
    default void glide() {
        System.out.println("Gliding through the air");
    }
}

// Class implementing multiple interfaces
public class Duck implements Swimmer, Flyer {
    @Override
    public void swim() {
        System.out.println("Duck is swimming");
    }
    
    @Override
    public void fly() {
        System.out.println("Duck is flying");
    }
    
    // Duck can use both default methods
    public void showAbilities() {
        swim();
        floatOnWater();
        fly();
        glide();
    }
}
```

#### Handling Method Name Conflicts

When a class implements multiple interfaces that have methods with the same signature, a conflict arises. The class must provide its own implementation to resolve the conflict.

```java
interface A {
    default void show() {
        System.out.println("Interface A");
    }
}

interface B {
    default void show() {
        System.out.println("Interface B");
    }
}

// This will cause a compilation error without an override
// class C implements A, B { }

// Resolving the conflict by providing an implementation
class C implements A, B {
    @Override
    public void show() {
        // Option 1: Choose one of the default implementations
        A.super.show();
        
        // Option 2: Choose the other default implementation
        // B.super.show();
        
        // Option 3: Provide a completely new implementation
        // System.out.println("Class C");
    }
}
```

#### 🤔 Think About It
What are the advantages and challenges of implementing multiple interfaces?

<details>
<summary>Answer</summary>
<p>Advantages of implementing multiple interfaces:</p>
<ul>
<li><strong>Multiple Type Inheritance</strong>: A class can be of multiple types, increasing flexibility.</li>
<li><strong>Code Organization</strong>: Interfaces allow for better separation of concerns.</li>
<li><strong>API Design</strong>: They enable more modular and extensible API design.</li>
<li><strong>Behavior Composition</strong>: With default methods, a class can inherit behavior from multiple sources.</li>
</ul>
<p>Challenges of implementing multiple interfaces:</p>
<ul>
<li><strong>Method Conflicts</strong>: When interfaces have methods with the same signature, conflicts must be resolved.</li>
<li><strong>Complexity</strong>: Implementing many interfaces can make a class complex and harder to understand.</li>
<li><strong>Diamond Problem</strong>: When interfaces extend other interfaces, the diamond problem can occur with default methods.</li>
<li><strong>Maintenance</strong>: Changes to interfaces can affect many implementing classes.</li>
</ul>
</details>

### 5. Functional Interfaces and Lambda Expressions

#### Functional Interfaces

A functional interface is an interface that contains exactly one abstract method. They are the foundation for lambda expressions in Java.

```java
// Functional interface (has exactly one abstract method)
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
    
    // Default and static methods don't count toward the single abstract method rule
    default void printInfo() {
        System.out.println("Calculator interface");
    }
    
    static void about() {
        System.out.println("This is a functional interface");
    }
}
```

#### Lambda Expressions

Lambda expressions provide a concise way to represent instances of functional interfaces.

```java
public class Main {
    public static void main(String[] args) {
        // Traditional anonymous class implementation
        Calculator addition = new Calculator() {
            @Override
            public int calculate(int a, int b) {
                return a + b;
            }
        };
        
        // Lambda expression (more concise)
        Calculator subtraction = (a, b) -> a - b;
        
        // Lambda with block body
        Calculator multiplication = (a, b) -> {
            System.out.println("Multiplying " + a + " and " + b);
            return a * b;
        };
        
        // Using the implementations
        System.out.println("10 + 5 = " + addition.calculate(10, 5));
        System.out.println("10 - 5 = " + subtraction.calculate(10, 5));
        System.out.println("10 * 5 = " + multiplication.calculate(10, 5));
    }
}
```

#### Common Functional Interfaces in Java

Java provides several built-in functional interfaces in the `java.util.function` package:

```java
import java.util.function.Function;
import java.util.function.Predicate;
import java.util.function.Consumer;
import java.util.function.Supplier;

public class FunctionalInterfacesDemo {
    public static void main(String[] args) {
        // Function<T, R> - takes an input of type T and returns a result of type R
        Function<String, Integer> stringLength = s -> s.length();
        System.out.println("Length of 'Hello': " + stringLength.apply("Hello"));
        
        // Predicate<T> - takes an input of type T and returns a boolean
        Predicate<Integer> isEven = n -> n % 2 == 0;
        System.out.println("Is 4 even? " + isEven.test(4));
        
        // Consumer<T> - takes an input of type T and returns no result
        Consumer<String> printer = s -> System.out.println("Consuming: " + s);
        printer.accept("Hello World");
        
        // Supplier<T> - takes no input and returns a result of type T
        Supplier<Double> randomValue = () -> Math.random();
        System.out.println("Random value: " + randomValue.get());
    }
}
```

#### 🤔 Think About It
How do lambda expressions improve code readability and maintainability?

<details>
<summary>Answer</summary>
<p>Lambda expressions improve code readability and maintainability in several ways:</p>
<ul>
<li><strong>Conciseness</strong>: They reduce boilerplate code, making the code more compact and focused on the logic.</li>
<li><strong>Clarity</strong>: They express behavior directly at the point of use, making the intent clearer.</li>
<li><strong>Functional Style</strong>: They enable a more functional programming style, which can be more expressive for certain operations.</li>
<li><strong>Reduced Verbosity</strong>: They eliminate the need for anonymous inner classes, which can be verbose and harder to read.</li>
<li><strong>Better API Design</strong>: They enable more flexible and expressive APIs that accept behavior as parameters.</li>
<li><strong>Parallel Processing</strong>: They work well with the Streams API for parallel processing, improving performance for large data sets.</li>
</ul>
</details>

### 6. Interface Inheritance

Interfaces can extend one or more other interfaces, creating an interface hierarchy.

```java
// Base interface
interface Shape {
    double calculateArea();
}

// Extended interface
interface ColoredShape extends Shape {
    String getColor();
}

// Another extended interface
interface ResizableShape extends Shape {
    void resize(double factor);
}

// Interface extending multiple interfaces
interface ColoredResizableShape extends ColoredShape, ResizableShape {
    void applyFilter(String filter);
}

// Implementing class
class Circle implements ColoredResizableShape {
    private double radius;
    private String color;
    
    public Circle(double radius, String color) {
        this.radius = radius;
        this.color = color;
    }
    
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
    
    @Override
    public String getColor() {
        return color;
    }
    
    @Override
    public void resize(double factor) {
        this.radius *= factor;
    }
    
    @Override
    public void applyFilter(String filter) {
        System.out.println("Applying " + filter + " filter to the " + color + " circle");
    }
}
```

#### 🤔 Think About It
How does interface inheritance differ from class inheritance?

<details>
<summary>Answer</summary>
<p>Interface inheritance differs from class inheritance in several ways:</p>
<ul>
<li><strong>Multiple Inheritance</strong>: An interface can extend multiple interfaces, while a class can extend only one class.</li>
<li><strong>Implementation</strong>: Interface inheritance doesn't inherit implementations (except for default methods), while class inheritance inherits both method declarations and implementations.</li>
<li><strong>Purpose</strong>: Interface inheritance is about defining contracts and types, while class inheritance is about code reuse and specialization.</li>
<li><strong>Instantiation</strong>: Interfaces cannot be instantiated, while concrete classes can.</li>
<li><strong>Access Modifiers</strong>: All methods in an interface are implicitly public, while class methods can have various access modifiers.</li>
<li><strong>State</strong>: Interfaces cannot have instance variables (state), while classes can.</li>
</ul>
</details>

---

## Session 2: Packages (45 minutes)

### 1. Introduction to Packages

#### What is a Package?

A package in Java is a namespace that organizes a set of related classes and interfaces. Packages help in avoiding name conflicts, controlling access, and making the code more maintainable.

#### 🔍 Key Characteristics

- Packages provide a way to organize related classes and interfaces
- They help in avoiding naming conflicts
- They provide access control
- They make it easier to locate and use classes
- The Java API is organized into packages

#### 📊 Visual Representation

```
┌───────────────────────────────────────────────────┐
│                                                   │
│                     PACKAGES                      │
│                                                   │
│  ┌─────────────────────────────────────────────┐  │
│  │ com.example.project                         │  │
│  │  ┌─────────────────┐  ┌─────────────────┐   │  │
│  │  │     model       │  │    controller   │   │  │
│  │  │  ┌──────────┐   │  │  ┌──────────┐   │   │  │
│  │  │  │ User.java│   │  │  │ App.java │   │   │  │
│  │  │  └──────────┘   │  │  └──────────┘   │   │  │
│  │  │  ┌──────────┐   │  │  ┌──────────┐   │   │  │
│  │  │  │ Item.java│   │  │  │ Main.java│   │   │  │
│  │  │  └──────────┘   │  │  └──────────┘   │   │  │
│  │  └─────────────────┘  └─────────────────┘   │  │
│  │                                             │  │
│  │  ┌─────────────────┐  ┌─────────────────┐   │  │
│  │  │     util        │  │     service     │   │  │
│  │  │  ┌──────────┐   │  │  ┌──────────┐   │   │  │
│  │  │  │Helper.java│  │  │  │UserService│  │   │  │
│  │  │  └──────────┘   │  │  └──────────┘   │   │  │
│  │  └─────────────────┘  └─────────────────┘   │  │
│  └─────────────────────────────────────────────┘  │
│                                                   │
└───────────────────────────────────────────────────┘
```

#### Benefits of Using Packages

1. **Organization**: Packages help organize related classes and interfaces.
2. **Namespace Management**: They prevent naming conflicts by providing a namespace.
3. **Access Control**: They enable access control at the package level.
4. **Encapsulation**: They help encapsulate related functionality.
5. **Reusability**: They make it easier to reuse code across projects.
6. **Distribution**: They simplify the distribution of libraries and APIs.

### 2. Creating and Using Packages

#### Package Declaration

To create a package, you use the `package` statement at the beginning of the source file:

```java
// File: com/example/math/Calculator.java
package com.example.math;

public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
    
    public int subtract(int a, int b) {
        return a - b;
    }
}
```

#### Package Naming Conventions

Package names are typically written in lowercase and follow a hierarchical naming pattern:

- Start with a domain name in reverse (e.g., `com.example`)
- Add project or organization name (e.g., `com.example.project`)
- Add module or component name (e.g., `com.example.project.util`)

#### Directory Structure

The directory structure should match the package structure:

```
src/
└── com/
    └── example/
        └── math/
            └── Calculator.java
```

#### Using Classes from Packages

To use a class from a package, you can:

1. Use the fully qualified name:

```java
// File: Main.java
public class Main {
    public static void main(String[] args) {
        com.example.math.Calculator calc = new com.example.math.Calculator();
        System.out.println("5 + 3 = " + calc.add(5, 3));
    }
}
```

2. Import the class:

```java
// File: Main.java
import com.example.math.Calculator;

public class Main {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        System.out.println("5 + 3 = " + calc.add(5, 3));
    }
}
```

3. Import all classes from a package:

```java
// File: Main.java
import com.example.math.*;

public class Main {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        System.out.println("5 + 3 = " + calc.add(5, 3));
    }
}
```

#### 🤔 Think About It
Why is it important to follow package naming conventions?

<details>
<summary>Answer</summary>
<p>Following package naming conventions is important for several reasons:</p>
<ul>
<li><strong>Uniqueness</strong>: Using reverse domain names helps ensure global uniqueness and prevents naming conflicts.</li>
<li><strong>Organization</strong>: Hierarchical naming reflects the logical organization of code.</li>
<li><strong>Readability</strong>: Consistent naming makes code more readable and understandable.</li>
<li><strong>Maintainability</strong>: Standard conventions make it easier for developers to navigate and maintain the codebase.</li>
<li><strong>Integration</strong>: Following conventions makes it easier to integrate with other libraries and frameworks.</li>
<li><strong>Distribution</strong>: It simplifies the distribution and use of libraries in different projects.</li>
</ul>
</details>

### 3. Import Statements

#### Types of Import Statements

Java provides two types of import statements:

1. **Single-Type Import**: Imports a specific class or interface.

```java
import java.util.ArrayList;
```

2. **Type-Import-on-Demand** (wildcard import): Imports all types from a package.

```java
import java.util.*;
```

#### Static Imports

Static imports allow you to import static members (methods and fields) from a class:

```java
// Without static import
System.out.println(Math.PI);
System.out.println(Math.sqrt(25));

// With static import
import static java.lang.Math.PI;
import static java.lang.Math.sqrt;

System.out.println(PI);
System.out.println(sqrt(25));

// With wildcard static import
import static java.lang.Math.*;

System.out.println(PI);
System.out.println(sqrt(25));
System.out.println(cos(0));
```

#### Import Order and Best Practices

It's a good practice to organize imports in a specific order:

1. Java core packages (`java.*`)
2. Java extension packages (`javax.*`)
3. Third-party libraries
4. Your own packages

Within each group, imports should be alphabetically ordered.

```java
// Java core packages
import java.io.File;
import java.util.ArrayList;
import java.util.List;

// Java extension packages
import javax.swing.JFrame;
import javax.xml.parsers.DocumentBuilder;

// Third-party libraries
import org.apache.commons.lang3.StringUtils;
import org.junit.Test;

// Your own packages
import com.example.project.model.User;
import com.example.project.util.Helper;
```

#### 🤔 Think About It
When should you use wildcard imports versus single-type imports?

<details>
<summary>Answer</summary>
<p>Considerations for wildcard imports versus single-type imports:</p>
<ul>
<li><strong>Single-Type Imports</strong> are generally preferred when:</li>
<ul>
<li>You're using only a few classes from a package</li>
<li>You want to make it clear exactly which classes are being used</li>
<li>You want to avoid potential naming conflicts</li>
<li>You want to improve code readability by explicitly showing dependencies</li>
</ul>
<li><strong>Wildcard Imports</strong> may be appropriate when:</li>
<ul>
<li>You're using many classes from the same package</li>
<li>The package is well-known and standard (like java.util)</li>
<li>You want to reduce the number of import statements</li>
<li>There's little risk of naming conflicts</li>
</ul>
<li>Many coding standards and teams prefer single-type imports for clarity and to avoid potential issues with name conflicts or unintended dependencies.</li>
</ul>
</details>

### 4. Access Modifiers and Packages

#### Access Levels in Java

Java provides four levels of access control:

| Modifier | Class | Package | Subclass | World |
|----------|-------|---------|----------|-------|
| public | Yes | Yes | Yes | Yes |
| protected | Yes | Yes | Yes | No |
| default (no modifier) | Yes | Yes | No | No |
| private | Yes | No | No | No |

#### Package-Private Access (Default)

When no access modifier is specified, the member has "package-private" access, meaning it's accessible only within its own package:

```java
// File: com/example/util/Helper.java
package com.example.util;

class Helper {  // package-private class
    void helperMethod() {  // package-private method
        System.out.println("Helper method");
    }
}

// File: com/example/util/UtilityClass.java
package com.example.util;

public class UtilityClass {
    public void doSomething() {
        Helper helper = new Helper();  // Accessible within the same package
        helper.helperMethod();  // Accessible within the same package
    }
}

// File: com/example/app/App.java
package com.example.app;

import com.example.util.UtilityClass;
// import com.example.util.Helper;  // Error: Helper is not public

public class App {
    public static void main(String[] args) {
        UtilityClass util = new UtilityClass();
        util.doSomething();  // OK
        
        // Helper helper = new Helper();  // Error: Helper is not accessible
        // helper.helperMethod();  // Error: helperMethod is not accessible
    }
}
```

#### Protected Access

Protected members are accessible within the same package and by subclasses in any package:

```java
// File: com/example/model/Shape.java
package com.example.model;

public class Shape {
    protected double area;  // protected field
    
    protected void calculateArea() {  // protected method
        System.out.println("Calculating area");
    }
}

// File: com/example/model/Circle.java
package com.example.model;

public class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
        calculateArea();  // Accessible as a subclass in the same package
        this.area = Math.PI * radius * radius;  // Accessible as a subclass in the same package
    }
}

// File: com/example/graphics/Rectangle.java
package com.example.graphics;

import com.example.model.Shape;

public class Rectangle extends Shape {
    private double width;
    private double height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
        calculateArea();  // Accessible as a subclass in a different package
        this.area = width * height;  // Accessible as a subclass in a different package
    }
    
    // Cannot access package-private members of Shape
}
```

#### 🤔 Think About It
How do access modifiers
#### 🤔 Think About It
How do access modifiers help in achieving encapsulation?

<details>
<summary>Answer</summary>
<p>Access modifiers help achieve encapsulation in several ways:</p>
<ul>
<li><strong>Information Hiding</strong>: They allow you to hide implementation details by making fields private.</li>
<li><strong>Controlled Access</strong>: They provide controlled access to class members through public methods (getters and setters).</li>
<li><strong>Preventing Misuse</strong>: They prevent direct manipulation of an object's state, reducing the risk of invalid states.</li>
<li><strong>Modularity</strong>: They enable you to change the internal implementation without affecting client code.</li>
<li><strong>Package-Level Design</strong>: They allow you to design package-level APIs that are separate from public APIs.</li>
<li><strong>Inheritance Control</strong>: They control what subclasses can access and override.</li>
</ul>
</details>

### 5. Java API Packages

The Java API (Application Programming Interface) is organized into packages that provide various functionalities. Here are some of the most commonly used packages:

#### Core Packages

1. **java.lang**: Contains fundamental classes and is automatically imported.
   - `String`, `Object`, `System`, `Math`, `Thread`, etc.

2. **java.util**: Contains utility classes, collections framework, date and time facilities, etc.
   - `ArrayList`, `HashMap`, `Date`, `Scanner`, etc.

3. **java.io**: Provides classes for input and output operations.
   - `File`, `InputStream`, `OutputStream`, `Reader`, `Writer`, etc.

4. **java.nio**: Provides enhanced I/O operations (New I/O).
   - `Path`, `Files`, `Channels`, etc.

5. **java.net**: Contains classes for networking operations.
   - `URL`, `Socket`, `ServerSocket`, etc.

#### GUI and Graphics Packages

1. **java.awt**: Abstract Window Toolkit for creating GUI components.
   - `Frame`, `Button`, `Graphics`, etc.

2. **javax.swing**: Provides lightweight GUI components.
   - `JFrame`, `JButton`, `JTable`, etc.

3. **javafx**: Modern GUI framework (separate module in Java 11+).
   - `Stage`, `Scene`, `Button`, etc.

#### Database Packages

1. **java.sql**: Provides API for accessing and processing data in a database.
   - `Connection`, `Statement`, `ResultSet`, etc.

2. **javax.sql**: Provides extended database capabilities.
   - `DataSource`, `RowSet`, etc.

#### XML and JSON Packages

1. **javax.xml**: Provides XML processing capabilities.
   - `parsers`, `transform`, `xpath`, etc.

2. **java.json** (Java 15+): Provides JSON processing capabilities.
   - `JsonObject`, `JsonArray`, etc.

#### Concurrency Packages

1. **java.util.concurrent**: Provides utilities for concurrent programming.
   - `ExecutorService`, `Future`, `ConcurrentHashMap`, etc.

#### Example of Using Java API Packages

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;
import java.io.File;
import java.io.FileNotFoundException;

public class JavaAPIDemo {
    public static void main(String[] args) {
        // Using java.util.ArrayList
        List<String> names = new ArrayList<>();
        names.add("Alice");
        names.add("Bob");
        names.add("Charlie");
        
        System.out.println("Names: " + names);
        
        // Using java.util.Scanner and java.io.File
        try {
            Scanner scanner = new Scanner(new File("data.txt"));
            while (scanner.hasNextLine()) {
                String line = scanner.nextLine();
                System.out.println("Read: " + line);
            }
            scanner.close();
        } catch (FileNotFoundException e) {
            System.out.println("File not found: " + e.getMessage());
        }
        
        // Using java.lang.Math (automatically imported)
        double randomValue = Math.random();
        System.out.println("Random value: " + randomValue);
        
        // Using java.time (Java 8+)
        java.time.LocalDate today = java.time.LocalDate.now();
        System.out.println("Today's date: " + today);
    }
}
```

#### 🤔 Think About It
Why is the Java API organized into packages?

<details>
<summary>Answer</summary>
<p>The Java API is organized into packages for several reasons:</p>
<ul>
<li><strong>Organization</strong>: Packages group related classes and interfaces, making the API more organized and easier to navigate.</li>
<li><strong>Namespace Management</strong>: Packages prevent naming conflicts among classes with the same name.</li>
<li><strong>Access Control</strong>: Packages enable access control at the package level, allowing certain classes to be accessible only within their package.</li>
<li><strong>Encapsulation</strong>: Packages help encapsulate related functionality, hiding implementation details.</li>
<li><strong>Modularity</strong>: Packages make the API more modular, allowing developers to use only the parts they need.</li>
<li><strong>Versioning</strong>: Packages facilitate versioning and evolution of the API over time.</li>
<li><strong>Distribution</strong>: Packages simplify the distribution of the API as JAR files or modules.</li>
</ul>
</details>

### 6. Creating Custom Packages

#### Step 1: Define the Package Structure

First, decide on a package naming convention and structure. For example:

```
com.example.project
├── model
├── service
├── util
└── controller
```

#### Step 2: Create the Directory Structure

Create directories that match your package structure:

```
src/
└── com/
    └── example/
        └── project/
            ├── model/
            ├── service/
            ├── util/
            └── controller/
```

#### Step 3: Create Classes in the Packages

Create Java files in the appropriate directories and declare the package at the top:

```java
// File: src/com/example/project/model/User.java
package com.example.project.model;

public class User {
    private String username;
    private String email;
    
    public User(String username, String email) {
        this.username = username;
        this.email = email;
    }
    
    public String getUsername() {
        return username;
    }
    
    public String getEmail() {
        return email;
    }
    
    @Override
    public String toString() {
        return "User{username='" + username + "', email='" + email + "'}";
    }
}
```

```java
// File: src/com/example/project/service/UserService.java
package com.example.project.service;

import com.example.project.model.User;
import java.util.ArrayList;
import java.util.List;

public class UserService {
    private List<User> users = new ArrayList<>();
    
    public void addUser(User user) {
        users.add(user);
    }
    
    public List<User> getAllUsers() {
        return new ArrayList<>(users);
    }
    
    public User findByUsername(String username) {
        return users.stream()
                   .filter(user -> user.getUsername().equals(username))
                   .findFirst()
                   .orElse(null);
    }
}
```

```java
// File: src/com/example/project/util/StringUtils.java
package com.example.project.util;

public class StringUtils {
    private StringUtils() {
        // Private constructor to prevent instantiation
    }
    
    public static boolean isEmpty(String str) {
        return str == null || str.trim().isEmpty();
    }
    
    public static String capitalize(String str) {
        if (isEmpty(str)) {
            return str;
        }
        return Character.toUpperCase(str.charAt(0)) + str.substring(1);
    }
}
```

```java
// File: src/com/example/project/controller/UserController.java
package com.example.project.controller;

import com.example.project.model.User;
import com.example.project.service.UserService;
import com.example.project.util.StringUtils;

public class UserController {
    private UserService userService = new UserService();
    
    public void createUser(String username, String email) {
        if (StringUtils.isEmpty(username) || StringUtils.isEmpty(email)) {
            throw new IllegalArgumentException("Username and email cannot be empty");
        }
        
        User user = new User(username, email);
        userService.addUser(user);
        System.out.println("User created: " + user);
    }
    
    public void displayAllUsers() {
        System.out.println("All Users:");
        for (User user : userService.getAllUsers()) {
            System.out.println("- " + user);
        }
    }
    
    public User findUser(String username) {
        return userService.findByUsername(username);
    }
}
```

#### Step 4: Create a Main Class to Use the Packages

```java
// File: src/com/example/project/Main.java
package com.example.project;

import com.example.project.controller.UserController;
import com.example.project.model.User;

public class Main {
    public static void main(String[] args) {
        UserController controller = new UserController();
        
        // Create users
        controller.createUser("alice", "alice@example.com");
        controller.createUser("bob", "bob@example.com");
        controller.createUser("charlie", "charlie@example.com");
        
        // Display all users
        controller.displayAllUsers();
        
        // Find a user
        User user = controller.findUser("bob");
        if (user != null) {
            System.out.println("Found user: " + user);
        } else {
            System.out.println("User not found");
        }
    }
}
```

#### Step 5: Compile and Run

Compile the code from the `src` directory:

```bash
javac com/example/project/Main.java
```

Run the program:

```bash
java com.example.project.Main
```

#### 🤔 Think About It
What are the benefits of organizing your code into custom packages?

<details>
<summary>Answer</summary>
<p>Organizing code into custom packages offers several benefits:</p>
<ul>
<li><strong>Code Organization</strong>: Packages provide a logical structure for organizing related classes and interfaces.</li>
<li><strong>Namespace Management</strong>: They prevent naming conflicts by providing a unique namespace for your classes.</li>
<li><strong>Access Control</strong>: They enable fine-grained access control through package-private access.</li>
<li><strong>Encapsulation</strong>: They help encapsulate implementation details, exposing only what's necessary.</li>
<li><strong>Modularity</strong>: They make the codebase more modular, allowing for better separation of concerns.</li>
<li><strong>Maintainability</strong>: They improve code maintainability by making it easier to locate and understand related code.</li>
<li><strong>Reusability</strong>: They facilitate code reuse across different parts of the application or different projects.</li>
<li><strong>Scalability</strong>: They make the codebase more scalable as it grows by providing a clear structure.</li>
<li><strong>Collaboration</strong>: They make it easier for teams to work on different parts of the codebase simultaneously.</li>
</ul>
</details>

---

## 🔄 Recap

In this session, we covered:

1. **Interfaces**
   - Definition and purpose of interfaces
   - Defining and implementing interfaces
   - Default and static methods in interfaces (Java 8+)
   - Multiple interface implementation
   - Functional interfaces and lambda expressions
   - Interface inheritance

2. **Packages**
   - Definition and purpose of packages
   - Creating and using packages
   - Import statements and best practices
   - Access modifiers and their relationship with packages
   - Java API packages
   - Creating custom packages

## 📚 Additional Resources

- [Oracle Java Documentation - Interfaces](https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html)
- [Oracle Java Documentation - Packages](https://docs.oracle.com/javase/tutorial/java/package/index.html)
- [Oracle Java Documentation - Lambda Expressions](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)
- [Baeldung - Java Interfaces](https://www.baeldung.com/java-interfaces)
- [Baeldung - Java Packages](https://www.baeldung.com/java-packages)
- [GeeksforGeeks - Interfaces in Java](https://www.geeksforgeeks.org/interfaces-in-java/)
- [JavaTpoint - Java Package](https://www.javatpoint.com/package)

## 🧩 Next Steps

In the next session, we'll explore:
- Exception handling in Java
- Arrays and array manipulation
- Multi-dimensional arrays
- Array algorithms and operations

## 🔍 Key Takeaways

- Interfaces define a contract that classes must fulfill, enabling polymorphism and multiple type inheritance
- Default and static methods in interfaces (Java 8+) provide implementation within interfaces
- Functional interfaces with a single abstract method are the foundation for lambda expressions
- Packages organize related classes and interfaces, providing namespace management and access control
- Import statements make classes from other packages accessible in your code
- Access modifiers control the visibility of classes, methods, and fields at different levels
- The Java API is organized into packages that provide various functionalities
- Custom packages help organize your code in a logical and maintainable structure