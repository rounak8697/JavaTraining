# Day 5: Practice Problems - Methods & Abstract Classes

## 🎯 Practice Objectives

By completing these practice problems, you will be able to:
- Implement method overloading in Java classes
- Create and use method overriding in inheritance hierarchies
- Distinguish between method overloading and overriding
- Implement aggregation and association relationships
- Design and implement abstract classes and methods
- Apply these concepts to solve real-world problems

## 🔄 Topics Covered

- Method Overloading
- Method Overriding
- Aggregation
- Association
- Abstract Classes
- Abstract Methods

---

## 🧩 Practice Problem 1: Method Overloading

### Problem Statement
Create a `Calculator` class that demonstrates method overloading. The class should have multiple versions of an `add` method that can:
1. Add two integers
2. Add three integers
3. Add two doubles
4. Add an integer array
5. Add a variable number of integers (using varargs)

### Starter Code
```java
public class Calculator {
    // Implement overloaded add methods here
    
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        
        // Test your methods here
        System.out.println("2 + 3 = " + calc.add(2, 3));
        System.out.println("2 + 3 + 4 = " + calc.add(2, 3, 4));
        System.out.println("2.5 + 3.7 = " + calc.add(2.5, 3.7));
        
        int[] numbers = {1, 2, 3, 4, 5};
        System.out.println("Sum of array: " + calc.add(numbers));
        
        System.out.println("Sum of varargs: " + calc.add(10, 20, 30, 40, 50));
    }
}
```

### Expected Output
```
2 + 3 = 5
2 + 3 + 4 = 9
2.5 + 3.7 = 6.2
Sum of array: 15
Sum of varargs: 150
```

### 💡 Hints
<details>
<summary>Hint 1</summary>
For the array version, use a loop to iterate through the array elements and add them up.
</details>

<details>
<summary>Hint 2</summary>
For the varargs version, use the syntax <code>public int add(int... numbers)</code>. Inside the method, you can treat <code>numbers</code> as an array.
</details>

### ✅ Solution
<details>
<summary>Solution</summary>

```java
public class Calculator {
    // Add two integers
    public int add(int a, int b) {
        return a + b;
    }
    
    // Add three integers
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    
    // Add two doubles
    public double add(double a, double b) {
        return a + b;
    }
    
    // Add an integer array
    public int add(int[] numbers) {
        int sum = 0;
        for (int num : numbers) {
            sum += num;
        }
        return sum;
    }
    
    // Add variable number of integers (varargs)
    public int add(int... numbers) {
        int sum = 0;
        for (int num : numbers) {
            sum += num;
        }
        return sum;
    }
    
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        
        System.out.println("2 + 3 = " + calc.add(2, 3));
        System.out.println("2 + 3 + 4 = " + calc.add(2, 3, 4));
        System.out.println("2.5 + 3.7 = " + calc.add(2.5, 3.7));
        
        int[] numbers = {1, 2, 3, 4, 5};
        System.out.println("Sum of array: " + calc.add(numbers));
        
        System.out.println("Sum of varargs: " + calc.add(10, 20, 30, 40, 50));
    }
}
```
</details>

---

## 🧩 Practice Problem 2: Method Overriding

### Problem Statement
Create a class hierarchy for different types of vehicles. Start with a base `Vehicle` class, then create subclasses for `Car`, `Motorcycle`, and `Truck`. Each class should override methods for `startEngine()`, `stopEngine()`, and `displayInfo()`.

### Starter Code
```java
class Vehicle {
    protected String make;
    protected String model;
    protected int year;
    
    public Vehicle(String make, String model, int year) {
        this.make = make;
        this.model = model;
        this.year = year;
    }
    
    public void startEngine() {
        System.out.println("Vehicle engine starting...");
    }
    
    public void stopEngine() {
        System.out.println("Vehicle engine stopping...");
    }
    
    public void displayInfo() {
        System.out.println("Vehicle: " + year + " " + make + " " + model);
    }
}

// Implement Car, Motorcycle, and Truck classes here

public class VehicleTest {
    public static void main(String[] args) {
        Vehicle[] vehicles = new Vehicle[3];
        vehicles[0] = new Car("Toyota", "Camry", 2022, 4);
        vehicles[1] = new Motorcycle("Harley-Davidson", "Sportster", 2021, false);
        vehicles[2] = new Truck("Ford", "F-150", 2023, 2000);
        
        for (Vehicle vehicle : vehicles) {
            vehicle.displayInfo();
            vehicle.startEngine();
            vehicle.stopEngine();
            System.out.println();
        }
    }
}
```

### Expected Output
```
Car: 2022 Toyota Camry with 4 doors
Starting car engine with key ignition...
Stopping car engine, applying parking brake...

Motorcycle: 2021 Harley-Davidson Sportster
Kickstarting motorcycle engine...
Stopping motorcycle engine, turning off fuel valve...

Truck: 2023 Ford F-150 with 2000kg payload capacity
Starting truck diesel engine...
Stopping truck engine, applying air brakes...
```

### 💡 Hints
<details>
<summary>Hint 1</summary>
Each subclass should extend the Vehicle class and override all three methods.
</details>

<details>
<summary>Hint 2</summary>
Add specific attributes to each subclass (e.g., number of doors for Car, payload capacity for Truck).
</details>

### ✅ Solution
<details>
<summary>Solution</summary>

```java
class Vehicle {
    protected String make;
    protected String model;
    protected int year;
    
    public Vehicle(String make, String model, int year) {
        this.make = make;
        this.model = model;
        this.year = year;
    }
    
    public void startEngine() {
        System.out.println("Vehicle engine starting...");
    }
    
    public void stopEngine() {
        System.out.println("Vehicle engine stopping...");
    }
    
    public void displayInfo() {
        System.out.println("Vehicle: " + year + " " + make + " " + model);
    }
}

class Car extends Vehicle {
    private int numDoors;
    
    public Car(String make, String model, int year, int numDoors) {
        super(make, model, year);
        this.numDoors = numDoors;
    }
    
    @Override
    public void startEngine() {
        System.out.println("Starting car engine with key ignition...");
    }
    
    @Override
    public void stopEngine() {
        System.out.println("Stopping car engine, applying parking brake...");
    }
    
    @Override
    public void displayInfo() {
        System.out.println("Car: " + year + " " + make + " " + model + " with " + numDoors + " doors");
    }
}

class Motorcycle extends Vehicle {
    private boolean hasSidecar;
    
    public Motorcycle(String make, String model, int year, boolean hasSidecar) {
        super(make, model, year);
        this.hasSidecar = hasSidecar;
    }
    
    @Override
    public void startEngine() {
        System.out.println("Kickstarting motorcycle engine...");
    }
    
    @Override
    public void stopEngine() {
        System.out.println("Stopping motorcycle engine, turning off fuel valve...");
    }
    
    @Override
    public void displayInfo() {
        System.out.println("Motorcycle: " + year + " " + make + " " + model + 
                          (hasSidecar ? " with sidecar" : ""));
    }
}

class Truck extends Vehicle {
    private int payloadCapacity; // in kg
    
    public Truck(String make, String model, int year, int payloadCapacity) {
        super(make, model, year);
        this.payloadCapacity = payloadCapacity;
    }
    
    @Override
    public void startEngine() {
        System.out.println("Starting truck diesel engine...");
    }
    
    @Override
    public void stopEngine() {
        System.out.println("Stopping truck engine, applying air brakes...");
    }
    
    @Override
    public void displayInfo() {
        System.out.println("Truck: " + year + " " + make + " " + model + 
                          " with " + payloadCapacity + "kg payload capacity");
    }
}

public class VehicleTest {
    public static void main(String[] args) {
        Vehicle[] vehicles = new Vehicle[3];
        vehicles[0] = new Car("Toyota", "Camry", 2022, 4);
        vehicles[1] = new Motorcycle("Harley-Davidson", "Sportster", 2021, false);
        vehicles[2] = new Truck("Ford", "F-150", 2023, 2000);
        
        for (Vehicle vehicle : vehicles) {
            vehicle.displayInfo();
            vehicle.startEngine();
            vehicle.stopEngine();
            System.out.println();
        }
    }
}
```
</details>

---

## 🧩 Practice Problem 3: Aggregation and Association

### Problem Statement
Implement a university management system that demonstrates both aggregation and association relationships. Create classes for `University`, `Department`, `Professor`, and `Student`. The relationships should be:
- A University has multiple Departments (aggregation)
- A Department has multiple Professors (aggregation)
- Professors teach Students (association)
- Students can enroll in multiple courses (association)

### Starter Code
```java
import java.util.ArrayList;
import java.util.List;

class Course {
    private String courseCode;
    private String name;
    
    public Course(String courseCode, String name) {
        this.courseCode = courseCode;
        this.name = name;
    }
    
    public String getCourseCode() {
        return courseCode;
    }
    
    public String getName() {
        return name;
    }
    
    @Override
    public String toString() {
        return courseCode + ": " + name;
    }
}

// Implement Student, Professor, Department, and University classes here

public class UniversitySystem {
    public static void main(String[] args) {
        // Create a university
        University university = new University("Tech University");
        
        // Create departments
        Department computerScience = new Department("Computer Science");
        Department mathematics = new Department("Mathematics");
        
        // Add departments to university
        university.addDepartment(computerScience);
        university.addDepartment(mathematics);
        
        // Create professors
        Professor profJava = new Professor("Dr. Java", "Programming");
        Professor profAlgo = new Professor("Dr. Algorithm", "Algorithms");
        Professor profCalc = new Professor("Dr. Calculus", "Mathematics");
        
        // Add professors to departments
        computerScience.addProfessor(profJava);
        computerScience.addProfessor(profAlgo);
        mathematics.addProfessor(profCalc);
        
        // Create courses
        Course javaCourse = new Course("CS101", "Java Programming");
        Course algoCourse = new Course("CS201", "Advanced Algorithms");
        Course calcCourse = new Course("MATH101", "Calculus I");
        
        // Assign courses to professors
        profJava.addCourse(javaCourse);
        profAlgo.addCourse(algoCourse);
        profCalc.addCourse(calcCourse);
        
        // Create students
        Student student1 = new Student("Alice", "S001");
        Student student2 = new Student("Bob", "S002");
        
        // Enroll students in courses
        student1.enrollInCourse(javaCourse);
        student1.enrollInCourse(calcCourse);
        student2.enrollInCourse(javaCourse);
        student2.enrollInCourse(algoCourse);
        
        // Display university structure
        System.out.println("University: " + university.getName());
        System.out.println("Departments:");
        for (Department dept : university.getDepartments()) {
            System.out.println("  - " + dept.getName());
            System.out.println("    Professors:");
            for (Professor prof : dept.getProfessors()) {
                System.out.println("      * " + prof.getName() + " (" + prof.getSpecialization() + ")");
                System.out.println("        Courses:");
                for (Course course : prof.getCourses()) {
                    System.out.println("          - " + course);
                }
            }
        }
        
        // Display student enrollments
        System.out.println("\nStudent Enrollments:");
        displayStudentInfo(student1);
        displayStudentInfo(student2);
    }
    
    private static void displayStudentInfo(Student student) {
        System.out.println(student.getName() + " (" + student.getId() + ")");
        System.out.println("  Enrolled Courses:");
        for (Course course : student.getEnrolledCourses()) {
            System.out.println("    - " + course);
        }
    }
}
```

### Expected Output
```
University: Tech University
Departments:
  - Computer Science
    Professors:
      * Dr. Java (Programming)
        Courses:
          - CS101: Java Programming
      * Dr. Algorithm (Algorithms)
        Courses:
          - CS201: Advanced Algorithms
  - Mathematics
    Professors:
      * Dr. Calculus (Mathematics)
        Courses:
          - MATH101: Calculus I

Student Enrollments:
Alice (S001)
  Enrolled Courses:
    - CS101: Java Programming
    - MATH101: Calculus I
Bob (S002)
  Enrolled Courses:
    - CS101: Java Programming
    - CS201: Advanced Algorithms
```

### 💡 Hints
<details>
<summary>Hint 1</summary>
Use ArrayLists to store collections of objects (departments, professors, courses, etc.).
</details>

<details>
<summary>Hint 2</summary>
For aggregation, the contained objects (e.g., departments) can exist independently of the container (e.g., university).
</details>

<details>
<summary>Hint 3</summary>
For association, create methods that establish relationships between objects (e.g., enrollInCourse).
</details>

### ✅ Solution
<details>
<summary>Solution</summary>

```java
import java.util.ArrayList;
import java.util.List;

class Course {
    private String courseCode;
    private String name;
    
    public Course(String courseCode, String name) {
        this.courseCode = courseCode;
        this.name = name;
    }
    
    public String getCourseCode() {
        return courseCode;
    }
    
    public String getName() {
        return name;
    }
    
    @Override
    public String toString() {
        return courseCode + ": " + name;
    }
}

class Student {
    private String name;
    private String id;
    private List<Course> enrolledCourses;
    
    public Student(String name, String id) {
        this.name = name;
        this.id = id;
        this.enrolledCourses = new ArrayList<>();
    }
    
    public void enrollInCourse(Course course) {
        enrolledCourses.add(course);
    }
    
    public String getName() {
        return name;
    }
    
    public String getId() {
        return id;
    }
    
    public List<Course> getEnrolledCourses() {
        return enrolledCourses;
    }
}

class Professor {
    private String name;
    private String specialization;
    private List<Course> courses;
    
    public Professor(String name, String specialization) {
        this.name = name;
        this.specialization = specialization;
        this.courses = new ArrayList<>();
    }
    
    public void addCourse(Course course) {
        courses.add(course);
    }
    
    public String getName() {
        return name;
    }
    
    public String getSpecialization() {
        return specialization;
    }
    
    public List<Course> getCourses() {
        return courses;
    }
}

class Department {
    private String name;
    private List<Professor> professors;
    
    public Department(String name) {
        this.name = name;
        this.professors = new ArrayList<>();
    }
    
    public void addProfessor(Professor professor) {
        professors.add(professor);
    }
    
    public String getName() {
        return name;
    }
    
    public List<Professor> getProfessors() {
        return professors;
    }
}

class University {
    private String name;
    private List<Department> departments;
    
    public University(String name) {
        this.name = name;
        this.departments = new ArrayList<>();
    }
    
    public void addDepartment(Department department) {
        departments.add(department);
    }
    
    public String getName() {
        return name;
    }
    
    public List<Department> getDepartments() {
        return departments;
    }
}

public class UniversitySystem {
    public static void main(String[] args) {
        // Create a university
        University university = new University("Tech University");
        
        // Create departments
        Department computerScience = new Department("Computer Science");
        Department mathematics = new Department("Mathematics");
        
        // Add departments to university
        university.addDepartment(computerScience);
        university.addDepartment(mathematics);
        
        // Create professors
        Professor profJava = new Professor("Dr. Java", "Programming");
        Professor profAlgo = new Professor("Dr. Algorithm", "Algorithms");
        Professor profCalc = new Professor("Dr. Calculus", "Mathematics");
        
        // Add professors to departments
        computerScience.addProfessor(profJava);
        computerScience.addProfessor(profAlgo);
        mathematics.addProfessor(profCalc);
        
        // Create courses
        Course javaCourse = new Course("CS101", "Java Programming");
        Course algoCourse = new Course("CS201", "Advanced Algorithms");
        Course calcCourse = new Course("MATH101", "Calculus I");
        
        // Assign courses to professors
        profJava.addCourse(javaCourse);
        profAlgo.addCourse(algoCourse);
        profCalc.addCourse(calcCourse);
        
        // Create students
        Student student1 = new Student("Alice", "S001");
        Student student2 = new Student("Bob", "S002");
        
        // Enroll students in courses
        student1.enrollInCourse(javaCourse);
        student1.enrollInCourse(calcCourse);
        student2.enrollInCourse(javaCourse);
        student2.enrollInCourse(algoCourse);
        
        // Display university structure
        System.out.println("University: " + university.getName());
        System.out.println("Departments:");
        for (Department dept : university.getDepartments()) {
            System.out.println("  - " + dept.getName());
            System.out.println("    Professors:");
            for (Professor prof : dept.getProfessors()) {
                System.out.println("      * " + prof.getName() + " (" + prof.getSpecialization() + ")");
                System.out.println("        Courses:");
                for (Course course : prof.getCourses()) {
                    System.out.println("          - " + course);
                }
            }
        }
        
        // Display student enrollments
        System.out.println("\nStudent Enrollments:");
        displayStudentInfo(student1);
        displayStudentInfo(student2);
    }
    
    private static void displayStudentInfo(Student student) {
        System.out.println(student.getName() + " (" + student.getId() + ")");
        System.out.println("  Enrolled Courses:");
        for (Course course : student.getEnrolledCourses()) {
            System.out.println("    - " + course);
        }
    }
}
```
</details>

---

## 🧩 Practice Problem 4: Abstract Classes and Methods

### Problem Statement
Create a banking system using abstract classes and methods. Design an abstract `BankAccount` class with concrete and abstract methods. Then implement concrete subclasses for different account types.

### Starter Code
```java
import java.util.ArrayList;
import java.util.List;

// Implement the abstract BankAccount class and concrete subclasses here

public class BankingSystem {
    public static void main(String[] args) {
        List<BankAccount> accounts = new ArrayList<>();
        
        // Create different types of accounts
        accounts.add(new SavingsAccount("SA001", "John Doe", 1000.0, 0.05));
        accounts.add(new CheckingAccount("CA001", "Jane Smith", 2000.0, 500.0));
        accounts.add(new FixedDepositAccount("FDA001", "Bob Johnson", 5000.0, 0.08, 12));
        
        // Perform operations on accounts
        for (BankAccount account : accounts) {
            System.out.println("Initial state:");
            account.displayInfo();
            
            // Deposit
            account.deposit(500.0);
            System.out.println("After deposit:");
            account.displayInfo();
            
            // Withdraw
            account.withdraw(200.0);
            System.out.println("After withdrawal:");
            account.displayInfo();
            
            // Calculate and add interest
            double interest = account.calculateInterest();
            System.out.println("Interest earned: $" + interest);
            account.deposit(interest);
            System.out.println("After adding interest:");
            account.displayInfo();
            
            System.out.println("-------------------------------");
        }
    }
}
```

### Expected Output
```
Initial state:
Savings Account [SA001]
Account Holder: John Doe
Balance: $1000.00
Interest Rate: 5.00%

After deposit:
Savings Account [SA001]
Account Holder: John Doe
Balance: $1500.00
Interest Rate: 5.00%

After withdrawal:
Savings Account [SA001]
Account Holder: John Doe
Balance: $1300.00
Interest Rate: 5.00%

Interest earned: $65.0
After adding interest:
Savings Account [SA001]
Account Holder: John Doe
Balance: $1365.00
Interest Rate: 5.00%
-------------------------------
Initial state:
Checking Account [CA001]
Account Holder: Jane Smith
Balance: $2000.00
Overdraft Limit: $500.00

After deposit:
Checking Account [CA001]
Account Holder: Jane Smith
Balance: $2500.00
Overdraft Limit: $500.00

After withdrawal:
Checking Account [CA001]
Account Holder: Jane Smith
Balance: $2300.00
Overdraft Limit: $500.00

Interest earned: $0.0
After adding interest:
Checking Account [CA001]
Account Holder: Jane Smith
Balance: $2300.00
Overdraft Limit: $500.00
-------------------------------
Initial state:
Fixed Deposit Account [FDA001]
Account Holder: Bob Johnson
Balance: $5000.00
Interest Rate: 8.00%
Term: 12 months

After deposit:
Fixed Deposit Account [FDA001]
Account Holder: Bob Johnson
Balance: $5500.00
Interest Rate: 8.00%
Term: 12 months

Withdrawal not allowed before maturity.
After withdrawal:
Fixed Deposit Account [FDA001]
Account Holder: Bob Johnson
Balance: $5500.00
Interest Rate: 8.00%
Term: 12 months

Interest earned: $440.0
After adding interest:
Fixed Deposit Account [FDA001]
Account Holder: Bob Johnson
Balance: $5940.00
Interest Rate: 8.00%
Term: 12 months
-------------------------------
```

### 💡 Hints
<details>
<summary>Hint 1</summary>
The abstract BankAccount class should have abstract methods for withdraw() and calculateInterest().
</details>

<details>
<summary>Hint 2</summary>
Each account type should have different rules for withdrawals and interest calculation.
</details>

<details>
<summary>Hint 3</summary>
Use the String.format() method to format currency values with two decimal places.
</details>

### ✅ Solution
<details>
<summary>Solution</summary>

```java
import java.util.ArrayList;
import java.util.List;

// Abstract BankAccount class
abstract class BankAccount {
    protected String accountNumber;
    protected String accountHolder;
    protected double balance;
    
    public BankAccount(String accountNumber, String accountHolder, double initialBalance) {
        this.accountNumber = accountNumber;
        this.accountHolder = accountHolder;
        this.balance = initialBalance;
    }
    
    // Concrete method
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("Deposited: $" + amount);
        }
    }
    
    // Abstract methods
    public abstract void withdraw(double amount);
    public abstract double calculateInterest();
    public abstract void displayInfo();
}

// Savings Account
class SavingsAccount extends BankAccount {
    private double interestRate; // Annual interest rate (e.g., 0.05 for 5%)
    
    public SavingsAccount(String accountNumber, String accountHolder, double initialBalance, double interestRate) {
        super(accountNumber, accountHolder, initialBalance);
        this.interestRate = interestRate;
    }
    
    @Override
    public void withdraw(double amount) {
        if (amount > 0 && balance >= amount) {
            balance -= amount;
            System.out.println("Withdrawn: $" + amount);
        } else {
            System.out.println("Insufficient funds for withdrawal.");
        }
    }
    
    @Override
    public double calculateInterest() {
        return balance * interestRate;
    }
    
    @Override
    public void displayInfo() {
        System.out.println("Savings Account [" + accountNumber + "]");
        System.out.println("Account Holder: " + accountHolder);
        System.out.println("Balance: $" + String.format("%.2f", balance));
        System.out.println("Interest Rate: " + String.format("%.2f", interestRate * 100) + "%");
        System.out.println();
    }
}

// Checking Account
class CheckingAccount extends BankAccount {
    private double overdraftLimit;
    
    public CheckingAccount(String accountNumber, String accountHolder, double initialBalance, double overdraftLimit) {
        super(accountNumber, accountHolder, initialBalance);
        this.overdraftLimit = overdraftLimit;
    }
    
    @Override
    public void withdraw(double amount) {
        if (amount > 0 && (balance + overdraftLimit) >= amount) {
            balance -= amount;
            System.out.println("Withdrawn: $" + amount);
        } else {
            System.out.println("Exceeds overdraft limit.");
        }
    }
    
    @Override
    public double calculateInterest() {
        // Checking accounts typically don't earn interest
        return 0.0;
    }
    
    @Override
    public void displayInfo() {
        System.out.println("Checking Account [" + accountNumber + "]");
        System.out.println("Account Holder: " + accountHolder);
        System.out.println("Balance: $" + String.format("%.2f", balance));
        System.out.println("Overdraft Limit: $" + String.format("%.2f", overdraftLimit));
        System.out.println();
    }
}

// Fixed Deposit Account
class FixedDepositAccount extends BankAccount {
    private double interestRate;
    private int termMonths;
    
    public FixedDepositAccount(String accountNumber, String accountHolder, double initialBalance, 
                              double interestRate, int termMonths) {
        super(accountNumber, accountHolder, initialBalance);
        this.interestRate = interestRate;
        this.termMonths = termMonths;
    }
    
    @Override
    public void withdraw(double amount) {
        // Cannot withdraw before maturity
        System.out.println("Withdrawal not allowed before maturity.");
    }
    
    @Override
    public double calculateInterest() {
        return balance * interestRate;
    }
    
    @Override
    public void displayInfo() {
        System.out.println("Fixed Deposit Account [" + accountNumber + "]");
        System.out.println("Account Holder: " + accountHolder);
        System.out.println("Balance: $" + String.format("%.2f", balance));
        System.out.println("Interest Rate: " + String.format("%.2f", interestRate * 100) + "%");
        System.out.println("Term: " + termMonths + " months");
        System.out.println();
    }
}

public class BankingSystem {
    public static void main(String[] args) {
        List<BankAccount> accounts = new ArrayList<>();
        
        // Create different types of accounts
        accounts.add(new SavingsAccount("SA001", "John Doe", 1000.0, 0.05));
        accounts.add(new CheckingAccount("CA001", "Jane Smith", 2000.0, 500.0));
        accounts.add(new FixedDepositAccount("FDA001", "Bob Johnson", 5000.0, 0.08, 12));
        
        // Perform operations on accounts
        for (BankAccount account : accounts) {
            System.out.println("Initial state:");
            account.displayInfo();
            
            // Deposit
            account.deposit(500.0);
            System.out.println("After deposit:");
            account.displayInfo();
            
            // Withdraw
            account.withdraw(200.0);
            System.out.println("After withdrawal:");
            account.displayInfo();
            
            // Calculate and add interest
            double interest = account.calculateInterest();
            System.out.println("Interest earned: $" + interest);
            account.deposit(interest);
            System.out.println("After adding interest:");
            account.displayInfo();
            
            System.out.println("-------------------------------");
        }
    }
}
```
</details>

---

## 🧩 Practice Problem 5: Combining Concepts

### Problem Statement
Create a media player application that combines method overloading, method overriding, aggregation, association, and abstract classes. The application should be able to play different types of media (audio, video) and manage playlists.

### Starter Code
```java
import java.util.ArrayList;
import java.util.List;

// Implement the necessary classes here

public class MediaPlayerApp {
    public static void main(String[] args) {
        // Create media files
        AudioFile song1 = new AudioFile("Song1.mp3", "Artist1", 180, "Pop");
        AudioFile song2 = new AudioFile("Song2.mp3", "Artist2", 210, "Rock");
        VideoFile video1 = new VideoFile("Video1.mp4", "Director1", 600, "HD");
        VideoFile video2 = new VideoFile("Video2.mp4", "Director2", 1200, "4K");
        
        // Create a playlist
        Playlist myPlaylist = new Playlist("My Favorites");
        myPlaylist.addMedia(song1);
        myPlaylist.addMedia(song2);
        myPlaylist.addMedia(video1);
        
        // Create a media player
        MediaPlayer player = new MediaPlayer("SuperPlayer 3000");
        player.addPlaylist(myPlaylist);
        
        // Play media
        System.out.println("Playing individual files:");
        player.play(song1);
        player.play(video1);
        
        // Play with specific settings
        System.out.println("\nPlaying with specific settings:");
        player.play(song2, 0.8); // Play audio at 80% volume
        player.play(video2, true); // Play video with subtitles
        
        // Play playlist
        System.out.println("\nPlaying playlist:");
        player.playPlaylist(myPlaylist);
        
        // Display player information
        System.out.println("\nMedia Player Information:");
        player.displayInfo();
    }
}
```

### Expected Output
```
Playing individual files:
Playing audio file: Song1.mp3
Artist: Artist1
Genre: Pop
Duration: 3:00

Playing video file: Video1.mp4
Director: Director1
Quality: HD
Duration: 10:00

Playing with specific settings:
Playing audio file: Song2.mp3
Artist: Artist2
Genre: Rock
Duration: 3:30
Volume: 80%

Playing video file: Video2.mp4
Director: Director2
Quality: 4K
Duration: 20:00
Subtitles: On

Playing playlist:
Now playing playlist: My Favorites (3 items)

Playing audio file: Song1.mp3
Artist: Artist1
Genre: Pop
Duration: 3:00

Playing audio file: Song2.mp3
Artist: Artist2
Genre: Rock
Duration: 3:30

Playing video file: Video1.mp4
Director: Director1
Quality: HD
Duration: 10:00

Media Player Information:
Player: SuperPlayer 3000
Number of playlists: 1
Total media files: 3
```

### 💡 Hints
<details>
<summary>Hint 1</summary>
Create an abstract MediaFile class that both AudioFile and VideoFile extend.
</details>

<details>
<summary>Hint 2</summary>
Use method overloading in the MediaPlayer class to provide different ways to play media files.
</details>

<details>
<summary>Hint 3</summary>
Implement aggregation between MediaPlayer and Playlist, and association between Playlist and MediaFile.
</details>

### ✅ Solution
<details>
<summary>Solution</summary>

```java
import java.util.ArrayList;
import java.util.List;

// Abstract class for media files
abstract class MediaFile {
    protected String fileName;
    protected int durationInSeconds;
    
    public MediaFile(String fileName, int durationInSeconds) {
        this.fileName = fileName;
        this.durationInSeconds = durationInSeconds;
    }
    
    public String getFileName() {
        return fileName;
    }
    
    public String getDuration() {
        int minutes = durationInSeconds / 60;
        int seconds = durationInSeconds % 60;
        return minutes + ":" + (seconds < 10 ? "0" + seconds : seconds);
    }
    
    // Abstract method to be implemented by subclasses
    public abstract void play();
}

// Audio file class
class AudioFile extends MediaFile {
    private String artist;
    private String genre;
    
    public AudioFile(String fileName, String artist, int durationInSeconds, String genre) {
        super(fileName, durationInSeconds);
        this.artist = artist;
        this.genre = genre;
    }
    
    @Override
    public void play() {
        System.out.println("Playing audio file: " + fileName);
        System.out.println("Artist: " + artist);
        System.out.println("Genre: " + genre);
        System.out.println("Duration: " + getDuration());
        System.out.println();
    }
    
    // Method overloading - play with volume
    public void play(double volume) {
        System.out.println("Playing audio file: " + fileName);
        System.out.println("Artist: " + artist);
        System.out.println("Genre: " + genre);
        System.out.println("Duration: " + getDuration());
        System.out.println("Volume: " + (int)(volume * 100) + "%");
        System.out.println();
    }
}

// Video file class
class VideoFile extends MediaFile {
    private String director;
    private String quality;
    
    public VideoFile(String fileName, String director, int durationInSeconds, String quality) {
        super(fileName, durationInSeconds);
        this.director = director;
        this.quality = quality;
    }
    
    @Override
    public void play() {
        System.out.println("Playing video file: " + fileName);
        System.out.println("Director: " + director);
        System.out.println("Quality: " + quality);
        System.out.println("Duration: " + getDuration());
        System.out.println();
    }
    
    // Method overloading - play with subtitles
    public void play(boolean subtitles) {
        System.out.println("Playing video file: " + fileName);
        System.out.println("Director: " + director);
        System.out.println("Quality: " + quality);
        System.out.println("Duration: " + getDuration());
        System.out.println("Subtitles: " + (subtitles ? "On" : "Off"));
        System.out.println();
    }
}

// Playlist class
class Playlist {
    private String name;
    private List<MediaFile> mediaFiles;
    
    public Playlist(String name) {
        this.name = name;
        this.mediaFiles = new ArrayList<>();
    }
    
    public void addMedia(MediaFile media) {
        mediaFiles.add(media);
    }
    
    public void removeMedia(MediaFile media) {
        mediaFiles.remove(media);
    }
    
    public String getName() {
        return name;
    }
    
    public List<MediaFile> getMediaFiles() {
        return mediaFiles;
    }
    
    public int getSize() {
        return mediaFiles.size();
    }
}

// Media player class
class MediaPlayer {
    private String model;
    private List<Playlist> playlists;
    
    public MediaPlayer(String model) {
        this.model = model;
        this.playlists = new ArrayList<>();
    }
    
    public void addPlaylist(Playlist playlist) {
        playlists.add(playlist);
    }
    
    // Method overloading for different play options
    public void play(MediaFile media) {
        media.play();
    }
    
    public void play(AudioFile audio, double volume) {
        audio.play(volume);
    }
    
    public void play(VideoFile video, boolean subtitles) {
        video.play(subtitles);
    }
    
    public void playPlaylist(Playlist playlist) {
        System.out.println("Now playing playlist: " + playlist.getName() +
                          " (" + playlist.getSize() + " items)");
        System.out.println();
        
        for (MediaFile media : playlist.getMediaFiles()) {
            media.play();
        }
    }
    
    public void displayInfo() {
        System.out.println("Player: " + model);
        System.out.println("Number of playlists: " + playlists.size());
        
        int totalMediaFiles = 0;
        for (Playlist playlist : playlists) {
            totalMediaFiles += playlist.getSize();
        }
        
        System.out.println("Total media files: " + totalMediaFiles);
    }
}

public class MediaPlayerApp {
    public static void main(String[] args) {
        // Create media files
        AudioFile song1 = new AudioFile("Song1.mp3", "Artist1", 180, "Pop");
        AudioFile song2 = new AudioFile("Song2.mp3", "Artist2", 210, "Rock");
        VideoFile video1 = new VideoFile("Video1.mp4", "Director1", 600, "HD");
        VideoFile video2 = new VideoFile("Video2.mp4", "Director2", 1200, "4K");
        
        // Create a playlist
        Playlist myPlaylist = new Playlist("My Favorites");
        myPlaylist.addMedia(song1);
        myPlaylist.addMedia(song2);
        myPlaylist.addMedia(video1);
        
        // Create a media player
        MediaPlayer player = new MediaPlayer("SuperPlayer 3000");
        player.addPlaylist(myPlaylist);
        
        // Play media
        System.out.println("Playing individual files:");
        player.play(song1);
        player.play(video1);
        
        // Play with specific settings
        System.out.println("\nPlaying with specific settings:");
        player.play(song2, 0.8); // Play audio at 80% volume
        player.play(video2, true); // Play video with subtitles
        
        // Play playlist
        System.out.println("\nPlaying playlist:");
        player.playPlaylist(myPlaylist);
        
        // Display player information
        System.out.println("\nMedia Player Information:");
        player.displayInfo();
    }
}
```
</details>

---

## 🏆 Challenge Problem: Shape Hierarchy with Drawing Capabilities

### Problem Statement
Create a comprehensive shape hierarchy that demonstrates all the concepts learned in this session. The hierarchy should include:

1. An abstract `Shape` class with:
   - Abstract methods for calculating area and perimeter
   - Concrete methods for displaying information and scaling the shape
   - Method overloading for different ways to draw the shape

2. Concrete shape classes (`Circle`, `Rectangle`, `Triangle`) that:
   - Override the abstract methods
   - Implement shape-specific functionality
   - Use method overloading for different constructors

3. A `DrawingCanvas` class that:
   - Has an aggregation relationship with shapes
   - Can add, remove, and draw multiple shapes
   - Provides different drawing options (color, line thickness)

4. A `ShapeGroup` class that:
   - Has an association relationship with shapes
   - Can group multiple shapes together
   - Allows operations on all shapes in the group

### Starter Code
```java
import java.util.ArrayList;
import java.util.List;

// Implement the necessary classes here

public class ShapeDrawingApp {
    public static void main(String[] args) {
        // Create shapes
        Circle circle = new Circle("Red", 5.0);
        Rectangle rectangle = new Rectangle("Blue", 4.0, 6.0);
        Triangle triangle = new Triangle("Green", 3.0, 4.0, 5.0);
        
        // Display shape information
        System.out.println("Shape Information:");
        circle.displayInfo();
        rectangle.displayInfo();
        triangle.displayInfo();
        
        // Scale shapes
        System.out.println("\nAfter scaling:");
        circle.scale(1.5);
        rectangle.scale(2.0);
        triangle.scale(0.5);
        
        circle.displayInfo();
        rectangle.displayInfo();
        triangle.displayInfo();
        
        // Create a drawing canvas
        DrawingCanvas canvas = new DrawingCanvas("Canvas1", 800, 600);
        canvas.addShape(circle);
        canvas.addShape(rectangle);
        canvas.addShape(triangle);
        
        // Draw all shapes on the canvas
        System.out.println("\nDrawing on canvas:");
        canvas.drawAllShapes();
        
        // Draw with specific options
        System.out.println("\nDrawing with specific options:");
        canvas.drawAllShapes("Dotted", 2.0);
        
        // Create a shape group
        ShapeGroup group = new ShapeGroup("MyGroup");
        group.addShape(circle);
        group.addShape(rectangle);
        
        // Perform group operations
        System.out.println("\nGroup operations:");
        group.displayInfo();
        group.moveAllShapes(10, 20);
        group.scaleAllShapes(1.2);
        group.displayInfo();
    }
}
```

### 💡 Hints
<details>
<summary>Hint 1</summary>
The abstract Shape class should include instance variables for color and position (x, y coordinates).
</details>

<details>
<summary>Hint 2</summary>
Implement method overloading in the draw methods to allow drawing with different options.
</details>

<details>
<summary>Hint 3</summary>
For the Triangle class, use Heron's formula to calculate the area: sqrt(s * (s-a) * (s-b) * (s-c)) where s = (a+b+c)/2
</details>

### ✅ Solution
<details>
<summary>Solution</summary>

```java
import java.util.ArrayList;
import java.util.List;

// Abstract Shape class
abstract class Shape {
    protected String color;
    protected double x;
    protected double y;
    
    public Shape(String color) {
        this.color = color;
        this.x = 0.0;
        this.y = 0.0;
    }
    
    public Shape(String color, double x, double y) {
        this.color = color;
        this.x = x;
        this.y = y;
    }
    
    // Abstract methods
    public abstract double calculateArea();
    public abstract double calculatePerimeter();
    public abstract void scale(double factor);
    
    // Concrete methods
    public void move(double deltaX, double deltaY) {
        this.x += deltaX;
        this.y += deltaY;
    }
    
    public void displayInfo() {
        System.out.println(getClass().getSimpleName() + " [" + color + "]");
        System.out.println("Position: (" + x + ", " + y + ")");
        System.out.println("Area: " + String.format("%.2f", calculateArea()));
        System.out.println("Perimeter: " + String.format("%.2f", calculatePerimeter()));
        System.out.println();
    }
    
    // Method overloading for drawing
    public void draw() {
        System.out.println("Drawing a " + color + " " + getClass().getSimpleName());
    }
    
    public void draw(String lineStyle) {
        System.out.println("Drawing a " + color + " " + getClass().getSimpleName() +
                          " with " + lineStyle + " lines");
    }
    
    public void draw(String lineStyle, double thickness) {
        System.out.println("Drawing a " + color + " " + getClass().getSimpleName() +
                          " with " + lineStyle + " lines of thickness " + thickness);
    }
}

// Circle class
class Circle extends Shape {
    private double radius;
    
    public Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }
    
    public Circle(String color, double x, double y, double radius) {
        super(color, x, y);
        this.radius = radius;
    }
    
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
    
    @Override
    public double calculatePerimeter() {
        return 2 * Math.PI * radius;
    }
    
    @Override
    public void scale(double factor) {
        this.radius *= factor;
    }
    
    @Override
    public void displayInfo() {
        System.out.println(getClass().getSimpleName() + " [" + color + "]");
        System.out.println("Position: (" + x + ", " + y + ")");
        System.out.println("Radius: " + radius);
        System.out.println("Area: " + String.format("%.2f", calculateArea()));
        System.out.println("Perimeter: " + String.format("%.2f", calculatePerimeter()));
        System.out.println();
    }
}

// Rectangle class
class Rectangle extends Shape {
    private double width;
    private double height;
    
    public Rectangle(String color, double width, double height) {
        super(color);
        this.width = width;
        this.height = height;
    }
    
    public Rectangle(String color, double x, double y, double width, double height) {
        super(color, x, y);
        this.width = width;
        this.height = height;
    }
    
    @Override
    public double calculateArea() {
        return width * height;
    }
    
    @Override
    public double calculatePerimeter() {
        return 2 * (width + height);
    }
    
    @Override
    public void scale(double factor) {
        this.width *= factor;
        this.height *= factor;
    }
    
    @Override
    public void displayInfo() {
        System.out.println(getClass().getSimpleName() + " [" + color + "]");
        System.out.println("Position: (" + x + ", " + y + ")");
        System.out.println("Width: " + width + ", Height: " + height);
        System.out.println("Area: " + String.format("%.2f", calculateArea()));
        System.out.println("Perimeter: " + String.format("%.2f", calculatePerimeter()));
        System.out.println();
    }
}

// Triangle class
class Triangle extends Shape {
    private double sideA;
    private double sideB;
    private double sideC;
    
    public Triangle(String color, double sideA, double sideB, double sideC) {
        super(color);
        this.sideA = sideA;
        this.sideB = sideB;
        this.sideC = sideC;
    }
    
    public Triangle(String color, double x, double y, double sideA, double sideB, double sideC) {
        super(color, x, y);
        this.sideA = sideA;
        this.sideB = sideB;
        this.sideC = sideC;
    }
    
    @Override
    public double calculateArea() {
        // Using Heron's formula
        double s = (sideA + sideB + sideC) / 2;
        return Math.sqrt(s * (s - sideA) * (s - sideB) * (s - sideC));
    }
    
    @Override
    public double calculatePerimeter() {
        return sideA + sideB + sideC;
    }
    
    @Override
    public void scale(double factor) {
        this.sideA *= factor;
        this.sideB *= factor;
        this.sideC *= factor;
    }
    
    @Override
    public void displayInfo() {
        System.out.println(getClass().getSimpleName() + " [" + color + "]");
        System.out.println("Position: (" + x + ", " + y + ")");
        System.out.println("Sides: a=" + sideA + ", b=" + sideB + ", c=" + sideC);
        System.out.println("Area: " + String.format("%.2f", calculateArea()));
        System.out.println("Perimeter: " + String.format("%.2f", calculatePerimeter()));
        System.out.println();
    }
}

// DrawingCanvas class (aggregation with shapes)
class DrawingCanvas {
    private String name;
    private int width;
    private int height;
    private List<Shape> shapes;
    
    public DrawingCanvas(String name, int width, int height) {
        this.name = name;
        this.width = width;
        this.height = height;
        this.shapes = new ArrayList<>();
    }
    
    public void addShape(Shape shape) {
        shapes.add(shape);
    }
    
    public void removeShape(Shape shape) {
        shapes.remove(shape);
    }
    
    public void drawAllShapes() {
        System.out.println("Drawing all shapes on canvas " + name + " (" + width + "x" + height + "):");
        for (Shape shape : shapes) {
            shape.draw();
        }
    }
    
    public void drawAllShapes(String lineStyle) {
        System.out.println("Drawing all shapes on canvas " + name + " with " + lineStyle + " lines:");
        for (Shape shape : shapes) {
            shape.draw(lineStyle);
        }
    }
    
    public void drawAllShapes(String lineStyle, double thickness) {
        System.out.println("Drawing all shapes on canvas " + name + " with " + lineStyle +
                          " lines of thickness " + thickness + ":");
        for (Shape shape : shapes) {
            shape.draw(lineStyle, thickness);
        }
    }
}

// ShapeGroup class (association with shapes)
class ShapeGroup {
    private String name;
    private List<Shape> shapes;
    
    public ShapeGroup(String name) {
        this.name = name;
        this.shapes = new ArrayList<>();
    }
    
    public void addShape(Shape shape) {
        shapes.add(shape);
    }
    
    public void removeShape(Shape shape) {
        shapes.remove(shape);
    }
    
    public void moveAllShapes(double deltaX, double deltaY) {
        System.out.println("Moving all shapes in group " + name + " by (" + deltaX + ", " + deltaY + ")");
        for (Shape shape : shapes) {
            shape.move(deltaX, deltaY);
        }
    }
    
    public void scaleAllShapes(double factor) {
        System.out.println("Scaling all shapes in group " + name + " by factor " + factor);
        for (Shape shape : shapes) {
            shape.scale(factor);
        }
    }
    
    public void displayInfo() {
        System.out.println("Shape Group: " + name);
        System.out.println("Number of shapes: " + shapes.size());
        for (Shape shape : shapes) {
            shape.displayInfo();
        }
    }
}

public class ShapeDrawingApp {
    public static void main(String[] args) {
        // Create shapes
        Circle circle = new Circle("Red", 5.0);
        Rectangle rectangle = new Rectangle("Blue", 4.0, 6.0);
        Triangle triangle = new Triangle("Green", 3.0, 4.0, 5.0);
        
        // Display shape information
        System.out.println("Shape Information:");
        circle.displayInfo();
        rectangle.displayInfo();
        triangle.displayInfo();
        
        // Scale shapes
        System.out.println("\nAfter scaling:");
        circle.scale(1.5);
        rectangle.scale(2.0);
        triangle.scale(0.5);
        
        circle.displayInfo();
        rectangle.displayInfo();
        triangle.displayInfo();
        
        // Create a drawing canvas
        DrawingCanvas canvas = new DrawingCanvas("Canvas1", 800, 600);
        canvas.addShape(circle);
        canvas.addShape(rectangle);
        canvas.addShape(triangle);
        
        // Draw all shapes on the canvas
        System.out.println("\nDrawing on canvas:");
        canvas.drawAllShapes();
        
        // Draw with specific options
        System.out.println("\nDrawing with specific options:");
        canvas.drawAllShapes("Dotted", 2.0);
        
        // Create a shape group
        ShapeGroup group = new ShapeGroup("MyGroup");
        group.addShape(circle);
        group.addShape(rectangle);
        
        // Perform group operations
        System.out.println("\nGroup operations:");
        group.displayInfo();
        group.moveAllShapes(10, 20);
        group.scaleAllShapes(1.2);
        group.displayInfo();
    }
}
```
</details>

---

## 📚 Additional Resources

- [Oracle Java Documentation - Method Overloading](https://docs.oracle.com/javase/tutorial/java/javaOO/methods.html)
- [Oracle Java Documentation - Method Overriding](https://docs.oracle.com/javase/tutorial/java/IandI/override.html)
- [Oracle Java Documentation - Abstract Classes](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)
- [Baeldung - Java Aggregation](https://www.baeldung.com/java-composition-aggregation-association)
- [GeeksforGeeks - Association, Composition and Aggregation in Java](https://www.geeksforgeeks.org/association-composition-aggregation-java/)
- [JavaTpoint - Abstract Class in Java](https://www.javatpoint.com/abstract-class-in-java)

## 🔍 Key Takeaways

- Method overloading allows a class to have multiple methods with the same name but different parameters
- Method overriding allows a subclass to provide a specific implementation of a method defined in its superclass
- Aggregation represents a "has-a" relationship where the contained object can exist independently
- Association represents a "uses-a" relationship between objects that are independent of each other
- Abstract classes cannot be instantiated and may contain both abstract and concrete methods
- Abstract methods have no implementation and must be implemented by concrete subclasses
- These concepts are fundamental to object-oriented programming and are used extensively in real-world applications
            