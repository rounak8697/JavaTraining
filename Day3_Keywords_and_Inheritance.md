# Day 3: Java Keywords & Inheritance

## 🗺️ Learning Roadmap

```
Day 1 (Basics) → Day 2 (Control Flow) → Day 3 (Keywords & Inheritance) → Day 4 (OOP Advanced)
                                           ↓
                                      You are here
```

## 🎯 Learning Objectives

By the end of this session, you will be able to:
- Create and use constructors to initialize objects
- Apply the static keyword to create class-level members
- Use the super keyword to access parent class members
- Implement inheritance to create class hierarchies
- Distinguish between different types of inheritance

## ⏱️ Time Allocation
- Java Keywords (45 minutes)
- Inheritance (45 minutes)

---

## Session 1: Java Keywords (45 minutes)

### 1. Constructors

#### What is a Constructor?

A constructor is a special method that is automatically called when an object is created. It has the same name as the class and is used to initialize the object's state.

#### 🔍 Key Characteristics

- Same name as the class
- No return type (not even void)
- Automatically called when an object is created using the `new` keyword
- Can be overloaded (multiple constructors with different parameters)

#### Types of Constructors

##### Default Constructor
```java
public class Student {
    // Default constructor (no parameters)
    public Student() {
        // Initialization code
        System.out.println("Student object created");
    }
}

// Usage
Student student = new Student();
```

##### Parameterized Constructor
```java
public class Student {
    private String name;
    private int age;
    
    // Parameterized constructor
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

// Usage
Student student = new Student("John", 20);
```

##### Constructor Overloading
```java
public class Student {
    private String name;
    private int age;
    private String course;
    
    // Constructor 1
    public Student() {
        this.name = "Unknown";
        this.age = 0;
        this.course = "Not assigned";
    }
    
    // Constructor 2
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
        this.course = "Not assigned";
    }
    
    // Constructor 3
    public Student(String name, int age, String course) {
        this.name = name;
        this.age = age;
        this.course = course;
    }
}
```

#### 🤔 Think About It
What happens if you don't define any constructor in your class?

<details>
<summary>Answer</summary>
If you don't define any constructor, Java automatically provides a default no-argument constructor that doesn't do anything except call the superclass constructor.
</details>

#### Constructor Chaining with `this()`

You can call one constructor from another using `this()`:

```java
public class Student {
    private String name;
    private int age;
    private String course;
    
    public Student() {
        this("Unknown", 0, "Not assigned");
    }
    
    public Student(String name, int age) {
        this(name, age, "Not assigned");
    }
    
    public Student(String name, int age, String course) {
        this.name = name;
        this.age = age;
        this.course = course;
    }
}
```

#### ⚠️ Common Mistakes

- Trying to call `this()` after other statements (it must be the first statement)
- Forgetting that constructors are not inherited
- Creating recursive constructor calls (which causes a compilation error)

#### 🌟 Best Practices

- Use constructors to ensure objects are in a valid state when created
- Provide overloaded constructors for flexibility
- Use constructor chaining to avoid code duplication
- Keep constructors simple and focused on initialization

---

### 2. Static Keyword

The `static` keyword in Java is used to create class-level members (variables and methods) that belong to the class itself rather than to instances of the class.

#### Static Variables

Static variables (also called class variables) are shared among all instances of a class.

```java
public class Counter {
    // Static variable - shared by all instances
    public static int count = 0;
    
    // Instance variable - each instance has its own copy
    public int instanceCount = 0;
    
    public Counter() {
        count++;         // Increments the shared count
        instanceCount++; // Increments this instance's count
    }
}

// Usage
Counter c1 = new Counter();
Counter c2 = new Counter();
Counter c3 = new Counter();

System.out.println(Counter.count);        // Outputs: 3
System.out.println(c1.instanceCount);     // Outputs: 1
System.out.println(c2.instanceCount);     // Outputs: 1
```

#### 📊 Visual Representation

```
┌─────────────────────────────────────────┐
│ Counter Class                           │
│                                         │
│  ┌───────────────┐                      │
│  │ static count  │ ◄─── Shared by all   │
│  │      3        │      instances       │
│  └───────────────┘                      │
│                                         │
└─────────────────────────────────────────┘
       ▲                 ▲                ▲
       │                 │                │
┌──────┴──────┐   ┌──────┴──────┐  ┌──────┴──────┐
│ Instance c1 │   │ Instance c2 │  │ Instance c3 │
│             │   │             │  │             │
│instanceCount│   │instanceCount│  │instanceCount│
│      1      │   │      1      │  │      1      │
└─────────────┘   └─────────────┘  └─────────────┘
```

#### Static Methods

Static methods belong to the class rather than instances and can only access static variables and other static methods directly.

```java
public class MathUtils {
    // Static method
    public static int add(int a, int b) {
        return a + b;
    }
    
    // Static method
    public static int multiply(int a, int b) {
        return a * b;
    }
}

// Usage - no need to create an instance
int sum = MathUtils.add(5, 3);        // sum = 8
int product = MathUtils.multiply(5, 3); // product = 15
```

#### Static Blocks

Static blocks are used for static initialization and are executed when the class is loaded.

```java
public class DatabaseConfig {
    public static String url;
    public static String username;
    public static String password;
    
    // Static block - runs when the class is loaded
    static {
        System.out.println("Loading database configuration...");
        url = "jdbc:mysql://localhost:3306/mydb";
        username = "admin";
        password = "securepassword";
    }
}
```

#### 🔄 Static vs Non-Static

| Feature | Static | Non-Static (Instance) |
|---------|--------|----------------------|
| Belongs to | Class | Object instance |
| Memory allocation | Once per class | Once per object |
| Access | ClassName.member | objectReference.member |
| Can access | Only static members directly | Both static and non-static members |
| When created | When class is loaded | When object is instantiated |

#### 🤔 Think About It
Why is the `main` method in Java declared as `static`?

<details>
<summary>Answer</summary>
The main method is declared static because it must be called by the JVM before any objects are created. Since it's static, the JVM can call it without creating an instance of the class.
</details>

#### 🌟 Best Practices

- Use static variables for constants or shared resources
- Use static methods for utility functions that don't depend on instance state
- Avoid excessive use of static variables (can make testing difficult)
- Be careful with static variables in multithreaded environments

---

### 3. Super Keyword

The `super` keyword in Java is used to refer to the parent class (superclass). It can be used to:
1. Call the parent class constructor
2. Access parent class methods
3. Access parent class variables

#### Calling Parent Class Constructor

```java
public class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

public class Student extends Person {
    private String studentId;
    
    public Student(String name, int age, String studentId) {
        super(name, age);  // Call to parent constructor
        this.studentId = studentId;
    }
}
```

#### 📊 Visual Representation

```
┌─────────────────────┐
│ Person Constructor  │
│  - Sets name        │
│  - Sets age         │
└─────────────────────┘
          ▲
          │ super(name, age)
          │
┌─────────────────────┐
│ Student Constructor │
│  - Sets studentId   │
└─────────────────────┘
```

#### Accessing Parent Class Methods

```java
public class Animal {
    public void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

public class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Dog barks");
    }
    
    public void makeAllSounds() {
        super.makeSound();  // Calls parent's method
        makeSound();        // Calls current class's method
    }
}

// Usage
Dog dog = new Dog();
dog.makeAllSounds();
// Output:
// Animal makes a sound
// Dog barks
```

#### Accessing Parent Class Variables

```java
public class Vehicle {
    protected int maxSpeed = 120;
}

public class Car extends Vehicle {
    int maxSpeed = 180;
    
    public void display() {
        System.out.println("Maximum speed: " + maxSpeed);
        System.out.println("Parent's maximum speed: " + super.maxSpeed);
    }
}

// Usage
Car car = new Car();
car.display();
// Output:
// Maximum speed: 180
// Parent's maximum speed: 120
```

#### ⚠️ Common Mistakes

- Forgetting that `super()` must be the first statement in a constructor
- Trying to use `super` in a class that doesn't have a parent class
- Confusing `super` with `this`

#### 🌟 Best Practices

- Always call the appropriate parent constructor
- Use `super` to access overridden methods when needed
- Use `super` to resolve naming conflicts between parent and child class members

---

### 4. Final Keyword

The `final` keyword in Java can be applied to classes, methods, and variables to impose restrictions on them.

#### Final Variables

A final variable can only be assigned once. It becomes a constant.

```java
public class Constants {
    // Final primitive variable
    final int MAX_USERS = 100;
    
    // Final reference variable
    final StringBuilder builder = new StringBuilder();
    
    public void modify() {
        // MAX_USERS = 200;  // Error: cannot assign a value to final variable
        
        // The reference is final, but the object can be modified
        builder.append("Hello");  // This is allowed
        
        // builder = new StringBuilder();  // Error: cannot assign a value to final variable
    }
}
```

#### Final Methods

A final method cannot be overridden by subclasses.

```java
public class Parent {
    // Final method - cannot be overridden
    public final void showInfo() {
        System.out.println("This is the parent class");
    }
}

public class Child extends Parent {
    // Error: Cannot override the final method from Parent
    // public void showInfo() {
    //     System.out.println("This is the child class");
    // }
}
```

#### Final Classes

A final class cannot be extended (subclassed).

```java
// Final class - cannot be extended
public final class Utility {
    public void doSomething() {
        // Method implementation
    }
}

// Error: Cannot extend final class
// public class AdvancedUtility extends Utility {
//     // Class implementation
// }
```

#### 🤔 Think About It
Why is the String class in Java declared as final?

<details>
<summary>Answer</summary>
The String class is declared final for security and efficiency. Since strings are widely used and often contain sensitive information, making String immutable and final prevents malicious code from extending and potentially compromising the String class. It also allows for optimizations like string pooling.
</details>

#### 🌟 Best Practices

- Use final for constants (usually with static)
- Use final for parameters that shouldn't be modified within a method
- Use final for methods that provide core functionality that shouldn't be altered
- Use final for classes that are designed to be used "as is" and not extended

---

## Session 2: Inheritance (45 minutes)

### 1. Java Inheritance Concepts

Inheritance is one of the core principles of object-oriented programming. It allows a class to inherit properties and behaviors from another class.

#### Basic Inheritance

```java
// Parent class (superclass)
public class Animal {
    protected String name;
    protected int age;
    
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public void eat() {
        System.out.println(name + " is eating");
    }
    
    public void sleep() {
        System.out.println(name + " is sleeping");
    }
}

// Child class (subclass)
public class Dog extends Animal {
    private String breed;
    
    public Dog(String name, int age, String breed) {
        super(name, age);
        this.breed = breed;
    }
    
    public void bark() {
        System.out.println(name + " is barking");
    }
}

// Usage
Dog dog = new Dog("Buddy", 3, "Golden Retriever");
dog.eat();    // Inherited method
dog.sleep();  // Inherited method
dog.bark();   // Dog's own method
```

#### 📊 Visual Representation

```
┌───────────────────────┐
│ Animal                │
│───────────────────────│
│ # name: String        │
│ # age: int            │
│───────────────────────│
│ + Animal(name, age)   │
│ + eat(): void         │
│ + sleep(): void       │
└───────────────────────┘
           ▲
           │ extends
           │
┌───────────────────────┐
│ Dog                   │
│───────────────────────│
│ - breed: String       │
│───────────────────────│
│ + Dog(name,age,breed) │
│ + bark(): void        │
└───────────────────────┘
```

#### Inheritance Terminology

- **Superclass/Parent class**: The class being inherited from
- **Subclass/Child class**: The class that inherits from another class
- **IS-A relationship**: A fundamental principle (Dog IS-A Animal)
- **Inheritance hierarchy**: The tree-like structure formed by inheritance relationships

#### Method Overriding

Method overriding allows a subclass to provide a specific implementation of a method that is already defined in its superclass.

```java
public class Animal {
    public void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

public class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Dog barks: Woof! Woof!");
    }
}

public class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Cat meows: Meow!");
    }
}

// Usage
Animal animal = new Animal();
Animal dog = new Dog();
Animal cat = new Cat();

animal.makeSound();  // Output: Animal makes a sound
dog.makeSound();     // Output: Dog barks: Woof! Woof!
cat.makeSound();     // Output: Cat meows: Meow!
```

#### 🔄 Method Overriding vs. Method Overloading

| Method Overriding | Method Overloading |
|-------------------|-------------------|
| Same method name and parameters | Same method name but different parameters |
| Occurs in subclass | Can occur in same class or subclass |
| Runtime polymorphism | Compile-time polymorphism |
| Uses `@Override` annotation | No special annotation |
| Requires inheritance | Doesn't require inheritance |

#### 🤔 Think About It
What happens if a subclass defines a method with the same name as a superclass method, but with different parameters?

<details>
<summary>Answer</summary>
This is not method overriding but method overloading. The subclass will have both methods available - the inherited method from the superclass and the new method with different parameters.
</details>

---

### 2. Types of Inheritance

Java supports several types of inheritance patterns:

#### Single Inheritance

A class inherits from only one superclass.

```java
public class Animal { /* ... */ }
public class Dog extends Animal { /* ... */ }
```

#### 📊 Visual Representation

```
┌─────────┐
│ Animal  │
└────┬────┘
     │
     ▼
┌─────────┐
│   Dog   │
└─────────┘
```

#### Multilevel Inheritance

A class inherits from a class, which in turn inherits from another class.

```java
public class Animal { /* ... */ }
public class Mammal extends Animal { /* ... */ }
public class Dog extends Mammal { /* ... */ }
```

#### 📊 Visual Representation

```
┌─────────┐
│ Animal  │
└────┬────┘
     │
     ▼
┌─────────┐
│ Mammal  │
└────┬────┘
     │
     ▼
┌─────────┐
│   Dog   │
└─────────┘
```

#### Hierarchical Inheritance

Multiple classes inherit from a single superclass.

```java
public class Animal { /* ... */ }
public class Dog extends Animal { /* ... */ }
public class Cat extends Animal { /* ... */ }
public class Bird extends Animal { /* ... */ }
```

#### 📊 Visual Representation

```
          ┌─────────┐
          │ Animal  │
          └────┬────┘
               │
     ┌─────────┼─────────┐
     │         │         │
     ▼         ▼         ▼
┌─────────┐┌─────────┐┌─────────┐
│   Dog   ││   Cat   ││  Bird   │
└─────────┘└─────────┘└─────────┘
```

#### Multiple Inheritance (Through Interfaces)

Java doesn't support multiple inheritance of classes, but it can be achieved through interfaces.

```java
public interface Swimmer { 
    void swim();
}

public interface Flyer { 
    void fly();
}

public class Duck implements Swimmer, Flyer {
    @Override
    public void swim() {
        System.out.println("Duck is swimming");
    }
    
    @Override
    public void fly() {
        System.out.println("Duck is flying");
    }
}
```

#### 📊 Visual Representation

```
┌─────────┐  ┌─────────┐
│ Swimmer │  │  Flyer  │
└────┬────┘  └────┬────┘
     │            │
     └──────┬─────┘
            │
            ▼
       ┌─────────┐
       │  Duck   │
       └─────────┘
```

#### Hybrid Inheritance

A combination of two or more types of inheritance.

```java
public class Animal { /* ... */ }
public class Mammal extends Animal { /* ... */ }
public class Aquatic extends Animal { /* ... */ }
public class Dolphin extends Mammal implements Swimmer { /* ... */ }
```

#### ⚠️ Common Inheritance Pitfalls

- **Diamond Problem**: A potential ambiguity that arises when a class inherits from two classes that have a common superclass
- **Excessive Inheritance**: Creating deep inheritance hierarchies that are hard to understand and maintain
- **Inheritance vs. Composition**: Using inheritance when composition would be more appropriate

#### 🌟 Best Practices

- Keep inheritance hierarchies shallow (not too many levels)
- Follow the Liskov Substitution Principle (a subclass should be substitutable for its superclass)
- Prefer composition over inheritance when appropriate
- Use interfaces for multiple inheritance
- Document the purpose of inheritance clearly

---

### 3. Sample Project Implementation

Let's implement a simple banking system to demonstrate inheritance concepts:

```java
// Base class
public class Account {
    protected String accountNumber;
    protected String accountHolder;
    protected double balance;
    
    public Account(String accountNumber, String accountHolder, double initialBalance) {
        this.accountNumber = accountNumber;
        this.accountHolder = accountHolder;
        this.balance = initialBalance;
    }
    
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("Deposited: $" + amount);
        } else {
            System.out.println("Invalid deposit amount");
        }
    }
    
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.println("Withdrawn: $" + amount);
        } else {
            System.out.println("Invalid withdrawal amount or insufficient funds");
        }
    }
    
    public void displayInfo() {
        System.out.println("Account Number: " + accountNumber);
        System.out.println("Account Holder: " + accountHolder);
        System.out.println("Balance: $" + balance);
    }
    
    public double getBalance() {
        return balance;
    }
}

// Savings Account (Child class)
public class SavingsAccount extends Account {
    private double interestRate;
    
    public SavingsAccount(String accountNumber, String accountHolder, 
                         double initialBalance, double interestRate) {
        super(accountNumber, accountHolder, initialBalance);
        this.interestRate = interestRate;
    }
    
    public void addInterest() {
        double interest = balance * interestRate / 100;
        deposit(interest);
        System.out.println("Interest added: $" + interest);
    }
    
    @Override
    public void displayInfo() {
        super.displayInfo();
        System.out.println("Account Type: Savings");
        System.out.println("Interest Rate: " + interestRate + "%");
    }
}

// Current Account (Child class)
public class CurrentAccount extends Account {
    private double overdraftLimit;
    
    public CurrentAccount(String accountNumber, String accountHolder, 
                         double initialBalance, double overdraftLimit) {
        super(accountNumber, accountHolder, initialBalance);
        this.overdraftLimit = overdraftLimit;
    }
    
    @Override
    public void withdraw(double amount) {
        if (amount > 0 && amount <= (balance + overdraftLimit)) {
            balance -= amount;
            System.out.println("Withdrawn: $" + amount);
        } else {
            System.out.println("Invalid withdrawal amount or exceeds overdraft limit");
        }
    }
    
    @Override
    public void displayInfo() {
        super.displayInfo();
        System.out.println("Account Type: Current");
        System.out.println("Overdraft Limit: $" + overdraftLimit);
    }
}

// Fixed Deposit Account (Child class)
public class FixedDepositAccount extends Account {
    private int lockInPeriod; // in months
    private double maturityAmount;
    
    public FixedDepositAccount(String accountNumber, String accountHolder, 
                              double initialBalance, int lockInPeriod, double maturityAmount) {
        super(accountNumber, accountHolder, initialBalance);
        this.lockInPeriod = lockInPeriod;
        this.maturityAmount = maturityAmount;
    }
    
    @Override
    public void withdraw(double amount) {
        System.out.println("Cannot withdraw from Fixed Deposit before maturity");
    }
    
    public void withdrawAfterMaturity() {
        System.out.println("Maturity amount of $" + maturityAmount + " has been released");
        balance = 0;
    }
    
    @Override
    public void displayInfo() {
        super.displayInfo();
        System.out.println("Account Type: Fixed Deposit");
        System.out.println("Lock-in Period: " + lockInPeriod + " months");
        System.out.println("Maturity Amount: $" + maturityAmount);
    }
}

// Main class to demonstrate the banking system
public class BankingDemo {
    public static void main(String[] args) {
        // Create different types of accounts
        SavingsAccount savings = new SavingsAccount("SA001", "John Doe", 1000, 3.5);
        CurrentAccount current = new CurrentAccount("CA001", "Jane Smith", 2000, 500);
        FixedDepositAccount fd = new FixedDepositAccount("FD001", "Bob Johnson", 5000, 12, 5300);
        
        // Demonstrate polymorphism
        Account[] accounts = {savings, current, fd};
        
        System.out.println("===== Account Information =====");
        for (Account account : accounts) {
            account.displayInfo();
            System.out.println("-----------------------------");
        }
        
        System.out.println("\n===== Account Operations =====");
        
        // Savings account operations
        System.out.println("\nSavings Account:");
        savings.deposit(500);
        savings.addInterest();
        savings.withdraw(200);
        System.out.println("Current Balance: $" + savings.getBalance());
        
        // Current account operations
        System.out.println("\nCurrent Account:");
        current.deposit(300);
        current.withdraw(2500); // This will use overdraft
        System.out.println("Current Balance: $" + current.getBalance());
        
        // Fixed deposit account operations
        System.out.println("\nFixed Deposit Account:");
        fd.deposit(1000);
        fd.withdraw(500); // This will be rejected
        System.out.println("Current Balance: $" + fd.getBalance());
    }
}
```

#### 🔄 Project Walkthrough

1. We created a base `Account` class with common attributes and behaviors
2. We derived three specialized account types:
   - `SavingsAccount` with interest functionality
   - `CurrentAccount` with overdraft capability
   - `FixedDepositAccount` with maturity constraints
3. Each subclass:
   - Calls the parent constructor using `super()`
   - Overrides methods as needed for specialized behavior
   - Adds new methods specific to its type
4. The main class demonstrates:
   - Object creation for different account types
   - Polymorphism through an array of the base type
   - Specific operations for each account type

#### 🚀 Extension Ideas

1. Add a `TransactionHistory` class to track all transactions
2. Implement a `Bank` class to manage multiple accounts
3. Add exception handling for invalid operations
4. Create a simple user interface for the banking system

---

## 🧠 Key Takeaways

1. **Constructors** initialize objects and can be overloaded for flexibility
2. **Static members** belong to the class rather than instances and are shared
3. **Super keyword** allows access to parent class members and constructors
4. **Final keyword** restricts modification of variables, methods, and classes
5. **Inheritance** enables code reuse and specialization through parent-child relationships
6. **Method overriding** allows subclasses to provide specific implementations
7. **Java supports** single, multilevel, and hierarchical inheritance directly
8. **Multiple inheritance** can be achieved through interfaces

## 📚 Further Reading

- Java Documentation: [Inheritance](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html)
- Java Documentation: [Using the Keyword super](https://docs.oracle.com/javase/tutorial/java/IandI/super.html)
- Book: "Effective Java" by Joshua Bloch (Chapter on Inheritance)
- Article: "Composition vs. Inheritance: How to Choose?"

## 🔄 Connection to Next Session

In the next session, we'll explore Abstraction and Polymorphism, which build upon the inheritance concepts we've learned today. We'll see how to create abstract classes and interfaces, and how polymorphism allows for flexible and extensible code.