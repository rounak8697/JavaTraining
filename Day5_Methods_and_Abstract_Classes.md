# Day 5: Methods & Abstract Classes

## 🗺️ Learning Roadmap

```
Day 1 (Basics) → Day 2 (Control Flow) → Day 3 (Keywords & Inheritance) → Day 4 (Abstraction & Polymorphism) → Day 5 (Methods & Abstract Classes) → Day 6 (Interfaces & Packages)
                                                                                                                  ↓
                                                                                                             You are here
```

## 🎯 Learning Objectives

By the end of this session, you will be able to:
- Understand and implement method overloading
- Understand and implement method overriding
- Differentiate between method overloading and overriding
- Implement aggregation and association relationships
- Design and implement abstract classes
- Create and use abstract methods
- Apply these concepts to solve real-world problems

## ⏱️ Time Allocation
- Method Concepts (45 minutes)
- Abstract Classes (45 minutes)

---

## Session 1: Method Concepts (45 minutes)

### 1. Method Overloading in Java

#### What is Method Overloading?

Method overloading is a feature that allows a class to have more than one method with the same name, but with different parameters. It is a compile-time polymorphism.

#### 🔍 Key Characteristics

- Same method name but different parameters
- Can differ in the number of parameters, type of parameters, or both
- Return type alone is not sufficient for method overloading
- Occurs within the same class
- Resolved at compile time (static binding)

#### 📊 Visual Representation

```
┌───────────────────────────────────────────────────┐
│                                                   │
│               METHOD OVERLOADING                  │
│                                                   │
│  ┌─────────────────────────────────────────────┐  │
│  │ class Calculator {                          │  │
│  │                                             │  │
│  │   int add(int a, int b) {                   │  │
│  │     return a + b;                           │  │
│  │   }                                         │  │
│  │                                             │  │
│  │   int add(int a, int b, int c) {            │  │
│  │     return a + b + c;                       │  │
│  │   }                                         │  │
│  │                                             │  │
│  │   double add(double a, double b) {          │  │
│  │     return a + b;                           │  │
│  │   }                                         │  │
│  │                                             │  │
│  └─────────────────────────────────────────────┘  │
│                                                   │
└───────────────────────────────────────────────────┘
```

#### Ways to Overload Methods

1. **Different Number of Parameters**

```java
public class Calculator {
    // Method with two parameters
    public int add(int a, int b) {
        return a + b;
    }
    
    // Method with three parameters
    public int add(int a, int b, int c) {
        return a + b + c;
    }
}
```

2. **Different Types of Parameters**

```java
public class Calculator {
    // Method with int parameters
    public int add(int a, int b) {
        return a + b;
    }
    
    // Method with double parameters
    public double add(double a, double b) {
        return a + b;
    }
}
```

3. **Different Order of Parameters**

```java
public class Printer {
    // Method with (String, int) parameters
    public void print(String text, int count) {
        for (int i = 0; i < count; i++) {
            System.out.println(text);
        }
    }
    
    // Method with (int, String) parameters
    public void print(int count, String text) {
        System.out.println(count + " copies of: " + text);
    }
}
```

#### ⚠️ Common Mistakes

1. **Return Type Alone is Not Enough**

```java
public class Example {
    // This won't compile - methods differ only by return type
    public int getValue() {
        return 10;
    }
    
    public double getValue() {  // Compilation error
        return 10.5;
    }
}
```

2. **Method Overloading vs. Method Overriding**

```java
public class Parent {
    public void display() {
        System.out.println("Parent's display method");
    }
}

public class Child extends Parent {
    // This is method overriding, not overloading
    @Override
    public void display() {
        System.out.println("Child's display method");
    }
    
    // This is method overloading
    public void display(String message) {
        System.out.println(message);
    }
}
```

#### 🔄 Real-World Example

Let's implement a `MessageSender` class that can send messages in different formats:

```java
public class MessageSender {
    // Send a simple text message
    public void send(String message) {
        System.out.println("Sending text message: " + message);
        // Code to send a text message
    }
    
    // Send a message to a specific recipient
    public void send(String message, String recipient) {
        System.out.println("Sending message to " + recipient + ": " + message);
        // Code to send a message to a specific recipient
    }
    
    // Send a message with an attachment
    public void send(String message, String recipient, String attachment) {
        System.out.println("Sending message to " + recipient + " with attachment " + 
                          attachment + ": " + message);
        // Code to send a message with an attachment
    }
    
    // Send a message to multiple recipients
    public void send(String message, String[] recipients) {
        System.out.println("Sending message to " + recipients.length + " recipients: " + message);
        // Code to send a message to multiple recipients
    }
}
```

#### 🤔 Think About It
Why is method overloading useful in programming?

<details>
<summary>Answer</summary>
Method overloading is useful because:
<ul>
<li>It improves code readability and reusability</li>
<li>It allows methods to handle different data types and parameter counts</li>
<li>It provides a consistent interface for similar operations</li>
<li>It reduces the need for different method names for similar operations</li>
<li>It makes the API more intuitive and easier to use</li>
</ul>
</details>

---

### 2. Method Overriding in Java

#### What is Method Overriding?

Method overriding is a feature that allows a subclass to provide a specific implementation of a method that is already defined in its superclass. It is a runtime polymorphism.

#### 🔍 Key Characteristics

- Same method name, same parameters, and same return type (or covariant return type)
- Occurs in an inheritance relationship (superclass and subclass)
- Annotated with `@Override` (recommended but not required)
- Resolved at runtime (dynamic binding)
- Access modifier can be the same or less restrictive

#### 📊 Visual Representation

```
┌───────────────────────────────────────────────────┐
│                                                   │
│               METHOD OVERRIDING                   │
│                                                   │
│  ┌─────────────────────────────────────────────┐  │
│  │ class Animal {                              │  │
│  │   public void makeSound() {                 │  │
│  │     System.out.println("Animal sound");     │  │
│  │   }                                         │  │
│  │ }                                           │  │
│  └─────────────────────────────────────────────┘  │
│                       ▲                           │
│                       │                           │
│  ┌─────────────────────────────────────────────┐  │
│  │ class Dog extends Animal {                  │  │
│  │   @Override                                 │  │
│  │   public void makeSound() {                 │  │
│  │     System.out.println("Woof!");            │  │
│  │   }                                         │  │
│  │ }                                           │  │
│  └─────────────────────────────────────────────┘  │
│                                                   │
└───────────────────────────────────────────────────┘
```

#### Rules for Method Overriding

1. **Method Signature Must Be the Same**

```java
public class Animal {
    public void makeSound() {
        System.out.println("Animal sound");
    }
}

public class Dog extends Animal {
    @Override
    public void makeSound() {  // Same name and parameters
        System.out.println("Woof!");
    }
}
```

2. **Return Type Must Be the Same or Covariant**

```java
public class Animal {
    public Animal getAnimal() {
        return new Animal();
    }
}

public class Dog extends Animal {
    @Override
    public Dog getAnimal() {  // Covariant return type (Dog is a subtype of Animal)
        return new Dog();
    }
}
```

3. **Access Modifier Can Be the Same or Less Restrictive**

```java
public class Animal {
    protected void makeSound() {
        System.out.println("Animal sound");
    }
}

public class Dog extends Animal {
    @Override
    public void makeSound() {  // Less restrictive (protected -> public)
        System.out.println("Woof!");
    }
}
```

4. **Cannot Override Final or Static Methods**

```java
public class Animal {
    public final void eat() {
        System.out.println("Animal is eating");
    }
    
    public static void sleep() {
        System.out.println("Animal is sleeping");
    }
}

public class Dog extends Animal {
    // Cannot override final method
    // public void eat() { ... }
    
    // This is method hiding, not overriding
    public static void sleep() {
        System.out.println("Dog is sleeping");
    }
}
```

5. **Exceptions Thrown by the Overriding Method**

```java
public class Animal {
    public void move() throws IOException {
        // Implementation
    }
}

public class Dog extends Animal {
    @Override
    public void move() throws FileNotFoundException {  // Subclass of IOException
        // Implementation
    }
    
    // This would cause a compilation error
    // @Override
    // public void move() throws Exception {  // Broader exception than IOException
    //     // Implementation
    // }
}
```

#### ⚠️ Common Mistakes

1. **Confusing Overriding with Overloading**

```java
public class Animal {
    public void makeSound() {
        System.out.println("Animal sound");
    }
}

public class Dog extends Animal {
    // This is overriding
    @Override
    public void makeSound() {
        System.out.println("Woof!");
    }
    
    // This is overloading, not overriding
    public void makeSound(String intensity) {
        System.out.println("Woof " + intensity + "!");
    }
}
```

2. **Forgetting the `@Override` Annotation**

```java
public class Animal {
    public void makeSound() {
        System.out.println("Animal sound");
    }
}

public class Dog extends Animal {
    // Missing @Override annotation
    public void makeSound() {  // Still overrides, but annotation is recommended
        System.out.println("Woof!");
    }
    
    // Typo in method name - won't override but will compile
    public void makesound() {  // Different method (lowercase 's')
        System.out.println("Woof!");
    }
}
```

#### 🔄 Real-World Example

Let's implement a `Shape` hierarchy with method overriding:

```java
public class Shape {
    protected String color;
    
    public Shape(String color) {
        this.color = color;
    }
    
    public double calculateArea() {
        return 0.0;  // Default implementation
    }
    
    public void display() {
        System.out.println("This is a shape with color " + color);
    }
}

public class Circle extends Shape {
    private double radius;
    
    public Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }
    
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
    
    @Override
    public void display() {
        System.out.println("This is a " + color + " circle with radius " + radius);
    }
}

public class Rectangle extends Shape {
    private double width;
    private double height;
    
    public Rectangle(String color, double width, double height) {
        super(color);
        this.width = width;
        this.height = height;
    }
    
    @Override
    public double calculateArea() {
        return width * height;
    }
    
    @Override
    public void display() {
        System.out.println("This is a " + color + " rectangle with width " + 
                          width + " and height " + height);
    }
}
```

#### 🤔 Think About It
Why is method overriding essential for polymorphism?

<details>
<summary>Answer</summary>
Method overriding is essential for polymorphism because:
<ul>
<li>It allows objects of different classes to be treated as objects of a common superclass</li>
<li>It enables different implementations of the same method to be called based on the actual object type</li>
<li>It supports the "one interface, multiple implementations" principle</li>
<li>It facilitates code extensibility without modifying existing code</li>
<li>It enables dynamic method dispatch at runtime</li>
</ul>
</details>

---

### 3. Method Overloading vs. Method Overriding

#### 📊 Comparison Table

| Feature | Method Overloading | Method Overriding |
|---------|-------------------|-------------------|
| Definition | Multiple methods with the same name but different parameters | Subclass provides a specific implementation of a method defined in the superclass |
| Parameters | Must be different (number, type, or order) | Must be the same |
| Return Type | Can be different | Must be the same or covariant |
| Access Modifier | Can be different | Must be the same or less restrictive |
| Exceptions | Can throw different exceptions | Can throw the same or subclass exceptions |
| Binding | Compile-time (static) | Runtime (dynamic) |
| Inheritance | Not required (can be in same class) | Required (must be in subclass) |
| `@Override` Annotation | Not applicable | Recommended |
| Purpose | Provides multiple ways to call a method | Provides specific implementation in subclasses |

#### 📊 Visual Comparison

```
┌───────────────────────────────────────────────────────────────────────────┐
│                                                                           │
│                    OVERLOADING vs. OVERRIDING                             │
│                                                                           │
│  ┌─────────────────────────────┐     ┌─────────────────────────────────┐  │
│  │       OVERLOADING           │     │         OVERRIDING              │  │
│  │                             │     │                                 │  │
│  │ class Calculator {          │     │ class Animal {                  │  │
│  │   int add(int a, int b)     │     │   void makeSound() {           │  │
│  │   int add(int a, int b, c)  │     │     // Animal implementation   │  │
│  │   double add(double a, b)   │     │   }                            │  │
│  │ }                           │     │ }                              │  │
│  │                             │     │                                 │  │
│  │ • Same class                │     │ • Inheritance relationship      │  │
│  │ • Different parameters      │     │ • Same parameters               │  │
│  │ • Compile-time binding      │     │ • Runtime binding               │  │
│  └─────────────────────────────┘     └─────────────────────────────────┘  │
│                                                                           │
└───────────────────────────────────────────────────────────────────────────┘
```

#### 🔄 Example Demonstrating Both Concepts

```java
// Parent class
public class Vehicle {
    // Method to be overridden
    public void move() {
        System.out.println("Vehicle is moving");
    }
    
    // Overloaded methods
    public void start() {
        System.out.println("Vehicle starting");
    }
    
    public void start(String key) {
        System.out.println("Vehicle starting with key: " + key);
    }
}

// Child class
public class Car extends Vehicle {
    // Method overriding
    @Override
    public void move() {
        System.out.println("Car is driving on the road");
    }
    
    // Method overloading in subclass
    public void start() {
        System.out.println("Car starting");
    }
    
    public void start(String key) {
        System.out.println("Car starting with key: " + key);
    }
    
    public void start(String key, boolean remoteStart) {
        if (remoteStart) {
            System.out.println("Car starting remotely with key: " + key);
        } else {
            start(key);
        }
    }
}

// Usage
public class Main {
    public static void main(String[] args) {
        // Method overloading demonstration
        Car car = new Car();
        car.start();                     // Calls Car's start()
        car.start("ABC123");             // Calls Car's start(String)
        car.start("ABC123", true);       // Calls Car's start(String, boolean)
        
        // Method overriding demonstration
        Vehicle vehicle = new Car();     // Polymorphic reference
        vehicle.move();                  // Calls Car's move() method
    }
}
```

#### 🤔 Think About It
In what scenarios would you prefer method overloading over method overriding, and vice versa?

<details>
<summary>Answer</summary>
<p>Prefer method overloading when:</p>
<ul>
<li>You need to perform the same operation with different types or numbers of parameters</li>
<li>You want to provide convenience methods with default values</li>
<li>You need to handle different input formats for the same operation</li>
<li>You want to maintain a consistent API with different parameter options</li>
</ul>
<p>Prefer method overriding when:</p>
<ul>
<li>You need to provide a specialized implementation of a method for a subclass</li>
<li>You want to implement polymorphic behavior</li>
<li>You need to extend or modify the behavior of a parent class</li>
<li>You're implementing an abstract method or interface method</li>
</ul>
</details>

---

### 4. Aggregation in Java

#### What is Aggregation?

Aggregation is a special form of association that represents a "has-a" relationship between objects. It is a weak form of object composition where one object can exist independently of the other.

#### 🔍 Key Characteristics

- Represents a "has-a" relationship
- The contained object can exist independently of the container
- The contained object is created outside the container
- If the container is destroyed, the contained object continues to exist

#### 📊 Visual Representation

```
┌───────────────────────────────────────────────────┐
│                                                   │
│                   AGGREGATION                     │
│                                                   │
│  ┌─────────────────┐        ┌─────────────────┐  │
│  │                 │        │                 │  │
│  │    Department   │◇───────│    Employee     │  │
│  │                 │        │                 │  │
│  └─────────────────┘        └─────────────────┘  │
│                                                   │
│  Department "has-a" Employee                      │
│  Employee can exist without Department            │
│                                                   │
└───────────────────────────────────────────────────┘
```

#### 🔄 Example of Aggregation

```java
// Employee class
public class Employee {
    private String name;
    private String id;
    
    public Employee(String name, String id) {
        this.name = name;
        this.id = id;
    }
    
    public String getName() {
        return name;
    }
    
    public String getId() {
        return id;
    }
}

// Department class
public class Department {
    private String name;
    private List<Employee> employees;
    
    public Department(String name) {
        this.name = name;
        this.employees = new ArrayList<>();
    }
    
    // Add an existing employee to the department
    public void addEmployee(Employee employee) {
        employees.add(employee);
    }
    
    // Remove an employee from the department
    public void removeEmployee(Employee employee) {
        employees.remove(employee);
    }
    
    public List<Employee> getEmployees() {
        return employees;
    }
    
    public String getName() {
        return name;
    }
}

// Usage
public class Main {
    public static void main(String[] args) {
        // Create employees
        Employee emp1 = new Employee("John", "E001");
        Employee emp2 = new Employee("Alice", "E002");
        Employee emp3 = new Employee("Bob", "E003");
        
        // Create departments
        Department hr = new Department("Human Resources");
        Department engineering = new Department("Engineering");
        
        // Add employees to departments
        hr.addEmployee(emp1);
        engineering.addEmployee(emp2);
        engineering.addEmployee(emp3);
        
        // Employee can be part of multiple departments
        hr.addEmployee(emp3);
        
        // If a department is deleted, employees still exist
        System.out.println("HR Department has " + hr.getEmployees().size() + " employees");
        
        // Employee can be removed from a department
        hr.removeEmployee(emp1);
        
        // Employee still exists even after removal from department
        System.out.println("Employee " + emp1.getName() + " still exists");
    }
}
```

#### 🤔 Think About It
What's the difference between aggregation and composition?

<details>
<summary>Answer</summary>
<p>The key differences between aggregation and composition are:</p>
<ul>
<li><strong>Lifetime dependency</strong>: In aggregation, the contained object can exist independently of the container. In composition, the contained object cannot exist without the container.</li>
<li><strong>Ownership</strong>: Aggregation implies a weaker "has-a" relationship where the container doesn't own the contained object. Composition implies a stronger "part-of" relationship where the container owns the contained object.</li>
<li><strong>Creation/destruction</strong>: In aggregation, the contained object is typically created outside the container and passed in. In composition, the contained object is created inside the container and destroyed when the container is destroyed.</li>
<li><strong>Relationship strength</strong>: Aggregation is a weak association, while composition is a strong association.</li>
</ul>
</details>

---

### 5. Association in Java

#### What is Association?

Association is a relationship between two separate classes that establishes through their objects. It can be one-to-one, one-to-many, many-to-one, or many-to-many.

#### 🔍 Key Characteristics

- Represents a "uses-a" relationship
- Objects are independent of each other
- No ownership implied
- Can be unidirectional or bidirectional

#### 📊 Visual Representation

```
┌───────────────────────────────────────────────────┐
│                                                   │
│                   ASSOCIATION                     │
│                                                   │
│  ┌─────────────────┐        ┌─────────────────┐  │
│  │                 │        │                 │  │
│  │     Teacher     │────────│     Student     │  │
│  │                 │        │                 │  │
│  └─────────────────┘        └─────────────────┘  │
│                                                   │
│  Teacher "teaches" Student                        │
│  Student "learns from" Teacher                    │
│                                                   │
└───────────────────────────────────────────────────┘
```

#### Types of Association

1. **One-to-One Association**

```java
// Person class
public class Person {
    private String name;
    private Passport passport;  // One-to-one association
    
    public Person(String name) {
        this.name = name;
    }
    
    public void setPassport(Passport passport) {
        this.passport = passport;
    }
    
    public Passport getPassport() {
        return passport;
    }
}

// Passport class
public class Passport {
    private String passportNumber;
    private Person owner;  // One-to-one association (bidirectional)
    
    public Passport(String passportNumber, Person owner) {
        this.passportNumber = passportNumber;
        this.owner = owner;
        owner.setPassport(this);  // Establish bidirectional relationship
    }
    
    public String getPassportNumber() {
        return passportNumber;
    }
    
    public Person getOwner() {
        return owner;
    }
}
```

2. **One-to-Many Association**

```java
// University class
public class University {
    private String name;
    private List<Student> students;  // One-to-many association
    
    public University(String name) {
        this.name = name;
        this.students = new ArrayList<>();
    }
    
    public void addStudent(Student student) {
        students.add(student);
    }
    
    public List<Student> getStudents() {
        return students;
    }
}

// Student class
public class Student {
    private String name;
    private String id;
    private University university;  // Many-to-one association
    
    public Student(String name, String id) {
        this.name = name;
        this.id = id;
    }
    
    public void enrollInUniversity(University university) {
        this.university = university;
        university.addStudent(this);  // Establish bidirectional relationship
    }
    
    public University getUniversity() {
        return university;
    }
}
```

3. **Many-to-Many Association**

```java
// Course class
public class Course {
    private String code;
    private String name;
    private List<Student> enrolledStudents;  // Many-to-many association
    
    public Course(String code, String name) {
        this.code = code;
        this.name = name;
        this.enrolledStudents = new ArrayList<>();
    }
    
    public void enrollStudent(Student student) {
        enrolledStudents.add(student);
        student.enrollInCourse(this);  // Establish bidirectional relationship
    }
    
    public List<Student> getEnrolledStudents() {
        return enrolledStudents;
    }
}

// Student class
public class Student {
    private String name;
    private String id;
    private List<Course> courses;  // Many-to-many association
    
    public Student(String name, String id) {
        this.name = name;
        this.id = id;
        this.courses = new ArrayList<>();
    }
    
    public void enrollInCourse(Course course) {
        courses.add(course);
    }
    
    public List<Course> getCourses() {
        return courses;
    }
}
```

#### 🔄 Real-World Example

Let's implement a library management system to demonstrate association:

```java
// Book class
public class Book {
    private String title;
    private String isbn;
    private Author author;  // Association with Author
    
    public Book(String title, String isbn, Author author) {
        this.title = title;
        this.isbn = isbn;
        this.author = author;
        author.addBook(this);  // Establish bidirectional relationship
    }
    
    public String getTitle() {
        return title;
    }
    
    public String getIsbn() {
        return isbn;
    }
    
    public Author getAuthor() {
        return author;
    }
}

// Author class
public class Author {
    private String name;
    private List<Book> books;  // Association with Book
    
    public Author(String name) {
        this.name = name;
        this.books = new ArrayList<>();
    }
    
    public void addBook(Book book) {
        books.add(book);
    }
    
    public String getName() {
        return name;
    }
    
    public List<Book> getBooks() {
        return books;
    }
}

// Library class
public class Library {
    private String name;
    private List<Book> books;  // Association with Book
    private List<Member> members;  // Association with Member
    
    public Library(String name) {
        this.name = name;
        this.books = new ArrayList<>();
        this.members = new ArrayList<>();
    }
    
    public void addBook(Book book) {
        books.add(book);
    }
    
    public void registerMember(Member member) {
        members.add(member);
        member.setLibrary(this);  // Establish bidirectional relationship
    }
    
    public void lendBook(Book book, Member member) {
        if (books.contains(book) && members.contains(member)) {
            member.borrowBook(book);
            books.remove(book);
        }
    }
    
    public void returnBook(Book book, Member member) {
        if (member.getBooks().contains(book)) {
            member.returnBook(book);
            books.add(book);
        }
    }
}

// Member class
public class Member {
    private String name;
    private String memberId;
    private Library library;  // Association with Library
    private List<Book> borrowedBooks;  // Association with Book
    
    public Member(String name, String memberId) {
        this.name = name;
        this.memberId = memberId;
        this.borrowedBooks = new ArrayList<>();
    }
    
    public void setLibrary(Library library) {
        this.library = library;
    }
    
    public void borrowBook(Book book) {
        borrowedBooks.add(book);
    }
    
    public void returnBook(Book book) {
        borrowedBooks.remove(book);
    }
    
    public List<Book> getBooks() {
        return borrowedBooks;
    }
}
```

#### 🤔 Think About It
How does association differ from inheritance?

<details>
<summary>Answer</summary>
<p>Association and inheritance represent fundamentally different relationships:</p>
<ul>
<li><strong>Inheritance</strong> represents an "is-a" relationship where a subclass is a specialized version of a superclass. It involves extending a class to create a new class that inherits properties and behaviors.</li>
<li><strong>Association</strong> represents a "uses-a" or "has-a" relationship where classes are connected through their objects. It involves objects of separate classes interacting with each other.</li>
<li><strong>Code reuse</strong>: Inheritance is primarily for code reuse through class extension. Association is for object collaboration through object references.</li>
<li><strong>Structure</strong>: Inheritance creates a hierarchical relationship between classes. Association creates a network of collaborating objects.</li>
<li><strong>Coupling</strong>: Inheritance creates tight coupling between classes. Association allows for looser coupling.</li>
<li><strong>Flexibility</strong>: Association is generally more flexible than inheritance as it allows for runtime changes in relationships.</li>
</ul>
</details>

---

## Session 2: Abstract Classes (45 minutes)

### 1. Abstract Classes in Java

#### What is an Abstract Class?

An abstract class is a class that cannot be instantiated and is designed to be subclassed. It may contain both abstract methods (methods without a body) and concrete methods (methods with a body).

#### 🔍 Key Characteristics

- Declared with the `abstract` keyword
- Cannot be instantiated directly
- May contain abstract methods
- May contain concrete methods
- May contain instance variables
- Subclasses must implement all abstract methods
- Can have constructors (used by subclasses)

#### 📊 Visual Representation

```
┌───────────────────────────────────────────────────┐
│                                                   │
│               ABSTRACT CLASS                      │
│                                                   │
│  ┌─────────────────────────────────────────────┐  │
│  │ abstract class Shape {                      │  │
│  │   // Instance variables                     │  │
│  │   protected String color;                   │  │
│  │                                             │  │
│  │   // Constructor                            │  │
│  │   public Shape(String color) {              │  │
│  │     this.color = color;                     │  │
│  │   }                                         │  │
│  │                                             │  │
│  │   // Concrete method                        │  │
│  │   public String getColor() {                │  │
│  │     return color;                           │  │
│  │   }                                         │  │
│  │                                             │  │
│  │   // Abstract method                        │  │
│  │   public abstract double calculateArea();   │  │
│  │ }                                           │  │
│  └─────────────────────────────────────────────┘  │
│                       ▲                           │
│                       │                           │
│  ┌─────────────────────────────────────────────┐  │
│  │ class Circle extends Shape {                │  │
│  │   private double radius;                    │  │
│  │                                             │  │
│  │   public Circle(String color, double r) {   │  │
│  │     super(color);                           │  │
│  │     this.radius = r;                        │  │
│  │   }                                         │  │
│  │                                             │  │
│  │   // Implementation of abstract method      │  │
│  │   @Override                                 │  │
│  │   public double calculateArea() {           │  │
│  │     return Math.PI * radius * radius;       │  │
│  │   }                                         │  │
│  │ }                                           │  │
│  └─────────────────────────────────────────────┘  │
│                                                   │
└───────────────────────────────────────────────────┘
```

#### When to Use Abstract Classes

1. **Common Base Class**: When you want to provide a common base class for a group of related classes
2. **Partial Implementation**: When you want to provide some common implementation while forcing subclasses to implement specific methods
3. **Template Method Pattern**: When you want to define a template for an algorithm but allow subclasses to implement specific steps
4. **Common State**: When related classes need to share common state (instance variables)

#### 🔄 Example of an Abstract Class

```java
// Abstract class
public abstract class Vehicle {
    // Instance variables
    protected String make;
    protected String model;
    protected int year;
    
    // Constructor
    public Vehicle(String make, String model, int year) {
        this.make = make;
        this.model = model;
        this.year = year;
    }
    
    // Concrete methods
    public String getMake() {
        return make;
    }
    
    public String getModel() {
        return model;
    }
    
    public int getYear() {
        return year;
    }
    
    public void displayInfo() {
        System.out.println("Vehicle: " + year + " " + make + " " + model);
    }
    
    // Abstract methods
    public abstract void start();
    public abstract void stop();
    public abstract double calculateFuelEfficiency();
}

// Concrete subclass
public class Car extends Vehicle {
    private int numDoors;
    private String engineType;
    
    public Car(String make, String model, int year, int numDoors, String engineType) {
        super(make, model, year);
        this.numDoors = numDoors;
        this.engineType = engineType;
    }
    
    @Override
    public void start() {
        System.out.println("Car starting: Turn key in ignition");
    }
    
    @Override
    public void stop() {
        System.out.println("Car stopping: Apply brakes and turn off ignition");
    }
    
    @Override
    public double calculateFuelEfficiency() {
        // Example calculation
        return engineType.equals("hybrid") ? 50.0 : 25.0;
    }
    
    public int getNumDoors() {
        return numDoors;
    }
}

// Another concrete subclass
public class Motorcycle extends Vehicle {
    private boolean hasSidecar;
    
    public Motorcycle(String make, String model, int year, boolean hasSidecar) {
        super(make, model, year);
        this.hasSidecar = hasSidecar;
    }
    
    @Override
    public void start() {
        System.out.println("Motorcycle starting: Press starter button");
    }
    
    @Override
    public void stop() {
        System.out.println("Motorcycle stopping: Apply brakes and turn off engine");
    }
    
    @Override
    public double calculateFuelEfficiency() {
        // Example calculation
        return hasSidecar ? 40.0 : 50.0;
    }
    
    public boolean hasSidecar() {
        return hasSidecar;
    }
}
```

#### ⚠️ Common Mistakes

1. **Trying to Instantiate an Abstract Class**

```java
// This will cause a compilation error
Vehicle vehicle = new Vehicle("Toyota", "Camry", 2022);

// Correct way
Vehicle vehicle = new Car("Toyota", "Camry", 2022, 4, "gasoline");
```

2. **Not Implementing All Abstract Methods**

```java
// This will cause a compilation error
public class Truck extends Vehicle {
    public Truck(String make, String model, int year) {
        super(make, model, year);
    }
    
    // Missing implementations of start(), stop(), and calculateFuelEfficiency()
}
```

3. **Confusing Abstract Classes with Interfaces**

```java
// Abstract class - can have instance variables and method implementations
public abstract class Shape {
    protected String color;
    
    public Shape(String color) {
        this.color = color;
    }
    
    public String getColor() {
        return color;
    }
    
    public abstract double calculateArea();
}

// Interface - can only have constants and abstract methods (prior to Java 8)
public interface Drawable {
    void draw();
    void resize(double factor);
}
```

### 2. Abstract Methods in Java

#### What is an Abstract Method?

An abstract method is a method that is declared without an implementation (without a body). It must be implemented by any concrete (non-abstract) subclass.

#### 🔍 Key Characteristics

- Declared with the `abstract` keyword
- Has no method body (no implementation)
- Must be in an abstract class or interface
- Must be implemented by any concrete subclass
- Cannot be private or final

#### Rules for Abstract Methods

1. **Must Be in an Abstract Class**

```java
// This will cause a compilation error
public class Shape {
    public abstract double calculateArea();  // Error: abstract method in non-abstract class
}

// Correct way
public abstract class Shape {
    public abstract double calculateArea();
}
```

2. **Cannot Have a Body**

```java
// This will cause a compilation error
public abstract class Shape {
    public abstract double calculateArea() {  // Error: abstract method cannot have a body
        return 0.0;
    }
}

// Correct way
public abstract class Shape {
    public abstract double calculateArea();
}
```

3. **Cannot Be Private or Final**

```java
// This will cause a compilation error
public abstract class Shape {
    private abstract double calculateArea();  // Error: abstract method cannot be private
    
    final abstract double calculatePerimeter();  // Error: abstract method cannot be final
}

// Correct way
public abstract class Shape {
    public abstract double calculateArea();
    
    protected abstract double calculatePerimeter();
}
```

4. **Must Be Implemented by Concrete Subclasses**

```java
public abstract class Shape {
    public abstract double calculateArea();
}

// This will cause a compilation error
public class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    // Missing implementation of calculateArea()
}

// Correct way
public class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}
```

#### 🔄 Real-World Example

Let's implement a banking system with abstract methods:

```java
// Abstract class
public abstract class BankAccount {
    protected String accountNumber;
    protected String accountHolder;
    protected double balance;
    
    public BankAccount(String accountNumber, String accountHolder, double initialBalance) {
        this.accountNumber = accountNumber;
        this.accountHolder = accountHolder;
        this.balance = initialBalance;
    }
    
    // Concrete methods
    public String getAccountNumber() {
        return accountNumber;
    }
    
    public String getAccountHolder() {
        return accountHolder;
    }
    
    public double getBalance() {
        return balance;
    }
    
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("Deposited: $" + amount);
        }
    }
    
    // Abstract methods
    public abstract void withdraw(double amount);
    public abstract double calculateInterest();
    public abstract void displayAccountInfo();
}

// Concrete subclass - Savings Account
public class SavingsAccount extends BankAccount {
    private double interestRate;
    private double minimumBalance;
    
    public SavingsAccount(String accountNumber, String accountHolder,
                         double initialBalance, double interestRate,
                         double minimumBalance) {
        super(accountNumber, accountHolder, initialBalance);
        this.interestRate = interestRate;
        this.minimumBalance = minimumBalance;
    }
    
    @Override
    public void withdraw(double amount) {
        if (amount > 0 && (balance - amount) >= minimumBalance) {
            balance -= amount;
            System.out.println("Withdrawn: $" + amount);
        } else {
            System.out.println("Withdrawal failed. Minimum balance requirement not met.");
        }
    }
    
    @Override
    public double calculateInterest() {
        return balance * interestRate;
    }
    
    @Override
    public void displayAccountInfo() {
        System.out.println("Savings Account Information:");
        System.out.println("Account Number: " + accountNumber);
        System.out.println("Account Holder: " + accountHolder);
        System.out.println("Balance: $" + balance);
        System.out.println("Interest Rate: " + (interestRate * 100) + "%");
        System.out.println("Minimum Balance: $" + minimumBalance);
    }
}

// Concrete subclass - Checking Account
public class CheckingAccount extends BankAccount {
    private double overdraftLimit;
    private double transactionFee;
    
    public CheckingAccount(String accountNumber, String accountHolder,
                          double initialBalance, double overdraftLimit,
                          double transactionFee) {
        super(accountNumber, accountHolder, initialBalance);
        this.overdraftLimit = overdraftLimit;
        this.transactionFee = transactionFee;
    }
    
    @Override
    public void withdraw(double amount) {
        if (amount > 0 && (balance - amount - transactionFee) >= -overdraftLimit) {
            balance -= (amount + transactionFee);
            System.out.println("Withdrawn: $" + amount);
            System.out.println("Transaction Fee: $" + transactionFee);
        } else {
            System.out.println("Withdrawal failed. Overdraft limit exceeded.");
        }
    }
    
    @Override
    public double calculateInterest() {
        // Checking accounts typically don't earn interest
        return 0.0;
    }
    
    @Override
    public void displayAccountInfo() {
        System.out.println("Checking Account Information:");
        System.out.println("Account Number: " + accountNumber);
        System.out.println("Account Holder: " + accountHolder);
        System.out.println("Balance: $" + balance);
        System.out.println("Overdraft Limit: $" + overdraftLimit);
        System.out.println("Transaction Fee: $" + transactionFee);
    }
}
```

#### 🤔 Think About It
What are the advantages of using abstract classes and methods in Java?

<details>
<summary>Answer</summary>
<p>The advantages of using abstract classes and methods include:</p>
<ul>
<li><strong>Code Reuse</strong>: Abstract classes allow you to define common behavior in one place that can be inherited by multiple subclasses.</li>
<li><strong>Enforced Implementation</strong>: Abstract methods ensure that subclasses provide implementations for essential functionality.</li>
<li><strong>Partial Implementation</strong>: Unlike interfaces (prior to Java 8), abstract classes can provide both abstract methods and concrete implementations.</li>
<li><strong>Common State</strong>: Abstract classes can have instance variables, allowing subclasses to share common state.</li>
<li><strong>Design Flexibility</strong>: They provide a middle ground between concrete classes and interfaces, offering more flexibility in design.</li>
<li><strong>Template Method Pattern</strong>: They enable the implementation of the template method pattern, where an algorithm's structure is defined in the abstract class, but specific steps are implemented by subclasses.</li>
</ul>
</details>

### 3. Abstract Classes vs. Interfaces

#### 📊 Comparison Table

| Feature | Abstract Class | Interface |
|---------|---------------|-----------|
| Definition | A class that cannot be instantiated and may contain abstract methods | A contract that specifies behavior a class must implement |
| Keyword | `abstract` | `interface` |
| Instantiation | Cannot be instantiated | Cannot be instantiated |
| Methods | Can have both abstract and concrete methods | Prior to Java 8: Only abstract methods<br>Java 8+: Can have default and static methods |
| Variables | Can have instance variables | Can only have constants (public static final) |
| Constructors | Can have constructors | Cannot have constructors |
| Access Modifiers | Can use all access modifiers | All methods are implicitly public |
| Inheritance | A class can extend only one abstract class | A class can implement multiple interfaces |
| Purpose | For code reuse and partial implementation | For defining a contract of behavior |

#### 📊 Visual Comparison

```
┌───────────────────────────────────────────────────────────────────────────┐
│                                                                           │
│                ABSTRACT CLASS vs. INTERFACE                               │
│                                                                           │
│  ┌─────────────────────────────┐     ┌─────────────────────────────────┐  │
│  │     ABSTRACT CLASS          │     │          INTERFACE              │  │
│  │                             │     │                                 │  │
│  │ abstract class Shape {      │     │ interface Drawable {            │  │
│  │   // Instance variables     │     │   // Constants only             │  │
│  │   protected String color;   │     │   int MAX_SIZE = 100;           │  │
│  │                             │     │                                 │  │
│  │   // Constructor            │     │   // No constructors            │  │
│  │   public Shape(String c) {  │     │                                 │  │
│  │     this.color = c;         │     │   // Abstract methods           │  │
│  │   }                         │     │   void draw();                  │  │
│  │                             │     │   void resize(double factor);   │  │
│  │   // Concrete method        │     │                                 │  │
│  │   public String getColor() {│     │   // Default method (Java 8+)   │  │
│  │     return color;           │     │   default void clear() {        │  │
│  │   }                         │     │     System.out.println("Clear");│  │
│  │                             │     │   }                             │  │
│  │   // Abstract method        │     │ }                               │  │
│  │   public abstract double    │     │                                 │  │
│  │     calculateArea();        │     │                                 │  │
│  │ }                           │     │                                 │  │
│  └─────────────────────────────┘     └─────────────────────────────────┘  │
│                                                                           │
└───────────────────────────────────────────────────────────────────────────┘
```

#### When to Use Abstract Classes vs. Interfaces

**Use Abstract Classes When:**

- You want to share code among closely related classes
- You need to have instance variables (state)
- You want to provide a partial implementation
- Your classes need constructors
- You want to use access modifiers other than public
- You're designing a framework where future extensions will share common behavior

**Use Interfaces When:**

- You want to define a contract that unrelated classes can implement
- You need multiple inheritance (a class can implement multiple interfaces)
- You want to specify behavior without dictating implementation
- You're designing for API stability where implementation details may change
- You want to define a common behavior across your entire application

#### 🔄 Example Demonstrating Both Concepts

```java
// Abstract class
public abstract class Animal {
    // Instance variables
    protected String name;
    protected int age;
    
    // Constructor
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // Concrete methods
    public String getName() {
        return name;
    }
    
    public int getAge() {
        return age;
    }
    
    public void sleep() {
        System.out.println(name + " is sleeping");
    }
    
    // Abstract methods
    public abstract void makeSound();
    public abstract void eat();
}

// Interfaces
public interface Swimmable {
    void swim();
    
    default void dive() {
        System.out.println("Diving underwater");
    }
}

public interface Flyable {
    void fly();
    
    default void land() {
        System.out.println("Landing safely");
    }
}

// Concrete class implementing both abstract class and interfaces
public class Duck extends Animal implements Swimmable, Flyable {
    private String species;
    
    public Duck(String name, int age, String species) {
        super(name, age);
        this.species = species;
    }
    
    @Override
    public void makeSound() {
        System.out.println("Quack! Quack!");
    }
    
    @Override
    public void eat() {
        System.out.println(name + " is eating bread");
    }
    
    @Override
    public void swim() {
        System.out.println(name + " is swimming in the pond");
    }
    
    @Override
    public void fly() {
        System.out.println(name + " is flying at low altitude");
    }
    
    public String getSpecies() {
        return species;
    }
}

// Another concrete class
public class Dog extends Animal {
    private String breed;
    
    public Dog(String name, int age, String breed) {
        super(name, age);
        this.breed = breed;
    }
    
    @Override
    public void makeSound() {
        System.out.println("Woof! Woof!");
    }
    
    @Override
    public void eat() {
        System.out.println(name + " is eating dog food");
    }
    
    public String getBreed() {
        return breed;
    }
}

// Usage
public class Main {
    public static void main(String[] args) {
        // Using abstract class
        Animal dog = new Dog("Buddy", 3, "Golden Retriever");
        dog.makeSound();  // Calls Dog's implementation
        dog.sleep();      // Uses Animal's implementation
        
        // Using both abstract class and interfaces
        Duck duck = new Duck("Donald", 2, "Mallard");
        duck.makeSound();  // From Animal
        duck.swim();       // From Swimmable
        duck.fly();        // From Flyable
        duck.dive();       // Default method from Swimmable
        
        // Polymorphism with interfaces
        Swimmable swimmer = duck;
        swimmer.swim();    // Calls Duck's implementation
        
        Flyable flyer = duck;
        flyer.fly();       // Calls Duck's implementation
    }
}
```

#### 🤔 Think About It
In what scenarios would you choose an abstract class over an interface, and vice versa?

<details>
<summary>Answer</summary>
<p>Choose an abstract class when:</p>
<ul>
<li>You need to share code and state among closely related classes</li>
<li>You want to provide a common implementation for some methods while requiring specific implementations for others</li>
<li>You need to use non-public access modifiers for methods</li>
<li>You need to maintain state (instance variables) that subclasses can access and modify</li>
<li>You want to provide a constructor that subclasses can use</li>
<li>You're designing a framework where future extensions will share common behavior</li>
</ul>
<p>Choose an interface when:</p>
<ul>
<li>You want to define a contract that unrelated classes can implement</li>
<li>You need a class to implement multiple behaviors (Java doesn't support multiple inheritance of classes)</li>
<li>You want to specify what a class should do without dictating how it should do it</li>
<li>You're designing for API stability where implementation details may change</li>
<li>You want to define a common behavior that can be implemented by any class regardless of its place in the class hierarchy</li>
<li>You're working with a team and want to clearly define the expected behavior without implementation details</li>
</ul>
</details>

---

## 🔄 Recap

In this session, we covered:

1. **Method Overloading**
   - Same method name, different parameters
   - Compile-time polymorphism
   - Occurs within the same class

2. **Method Overriding**
   - Same method name, same parameters
   - Runtime polymorphism
   - Occurs in inheritance relationship

3. **Aggregation**
   - "Has-a" relationship
   - Weak association
   - Container doesn't own contained object

4. **Association**
   - "Uses-a" relationship
   - Objects interact with each other
   - Can be one-to-one, one-to-many, or many-to-many

5. **Abstract Classes**
   - Cannot be instantiated
   - May contain abstract and concrete methods
   - Used for code reuse and partial implementation

6. **Abstract Methods**
   - No implementation (no body)
   - Must be implemented by concrete subclasses
   - Cannot be private or final

## 📚 Additional Resources

- [Oracle Java Documentation - Abstract Methods and Classes](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)
- [Oracle Java Documentation - Overriding and Hiding Methods](https://docs.oracle.com/javase/tutorial/java/IandI/override.html)
- [Baeldung - Java Method Overloading](https://www.baeldung.com/java-method-overload-override)
- [GeeksforGeeks - Association, Composition and Aggregation in Java](https://www.geeksforgeeks.org/association-composition-aggregation-java/)
- [JavaTpoint - Abstract Class in Java](https://www.javatpoint.com/abstract-class-in-java)

## 🧩 Next Steps

In the next session, we'll explore:
- Interfaces in Java
- Packages and access modifiers
- Creating and using custom packages

## 🔍 Key Takeaways

- Method overloading provides multiple ways to call a method with different parameters
- Method overriding allows subclasses to provide specific implementations of methods
- Aggregation and association represent different types of relationships between objects
- Abstract classes provide a way to share code while enforcing implementation of specific methods
- Abstract methods define a contract that subclasses must fulfill
- Understanding when to use abstract classes vs. interfaces is crucial for good object-oriented design