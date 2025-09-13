# Day 3: Java Keywords & Inheritance - Practice Problems

## 🎯 Skills You'll Practice
- Creating and using constructors
- Implementing static variables and methods
- Using the super keyword to access parent class members
- Designing and implementing inheritance hierarchies
- Applying method overriding

## 🧠 Learning Journey
Today's problems will help you apply the Java keywords and inheritance concepts you learned in the lecture. You'll start with basic constructor and static member problems, then progress to more complex inheritance scenarios.

---

## Problem 1: Student Record System

**Difficulty**: ⭐ Beginner  
**Estimated Time**: 15 minutes  
**Focus**: Constructors, static variables

### 📝 Problem Statement
Create a `Student` class that manages student records. The class should:
1. Have instance variables for `id`, `name`, and `grade`
2. Have a static variable `totalStudents` to keep track of the number of students created
3. Implement multiple constructors:
   - A default constructor that sets default values
   - A parameterized constructor that takes id, name, and grade
   - A constructor that takes only id and name (sets grade to "N/A")
4. Include a static method `getTotalStudents()` that returns the total number of students
5. Include a method `displayInfo()` to print student information

### 🔍 Example
```
Student 1 Info:
ID: 101
Name: John Doe
Grade: A
Total students: 1

Student 2 Info:
ID: 102
Name: Jane Smith
Grade: B
Total students: 2

Student 3 Info:
ID: 103
Name: Bob Johnson
Grade: N/A
Total students: 3
```

### 💡 Hints
<details>
<summary>Click for hints</summary>

1. Increment the static `totalStudents` variable in each constructor
2. Use constructor chaining with `this()` to avoid code duplication
3. Remember that static variables are shared across all instances of the class
</details>

### ✅ Solution
```java
public class Student {
    private int id;
    private String name;
    private String grade;
    
    // Static variable to track total students
    private static int totalStudents = 0;
    
    // Default constructor
    public Student() {
        this(0, "Unknown", "N/A");
    }
    
    // Constructor with id and name
    public Student(int id, String name) {
        this(id, name, "N/A");
    }
    
    // Constructor with all fields
    public Student(int id, String name, String grade) {
        this.id = id;
        this.name = name;
        this.grade = grade;
        totalStudents++; // Increment the counter
    }
    
    // Static method to get total students
    public static int getTotalStudents() {
        return totalStudents;
    }
    
    // Method to display student information
    public void displayInfo() {
        System.out.println("Student Info:");
        System.out.println("ID: " + id);
        System.out.println("Name: " + name);
        System.out.println("Grade: " + grade);
        System.out.println("Total students: " + totalStudents);
        System.out.println();
    }
    
    public static void main(String[] args) {
        // Create students using different constructors
        Student student1 = new Student(101, "John Doe", "A");
        student1.displayInfo();
        
        Student student2 = new Student(102, "Jane Smith", "B");
        student2.displayInfo();
        
        Student student3 = new Student(103, "Bob Johnson");
        student3.displayInfo();
    }
}
```

### 🔄 Code Walkthrough
1. We define a `Student` class with instance variables for id, name, and grade
2. We create a static variable `totalStudents` to track the number of students
3. We implement three constructors with constructor chaining
4. Each constructor increments the static counter
5. We provide a static method to access the total count
6. The `displayInfo()` method shows both instance data and the static count
7. In the main method, we demonstrate creating students with different constructors

### 🚀 Extension Challenge
Add a static method `resetCount()` that resets the student count to zero, and a static method `getAverageGrade()` that calculates the average grade of all students (you'll need to store all students in a static collection).

---

## Problem 2: Temperature Converter

**Difficulty**: ⭐ Beginner  
**Estimated Time**: 15 minutes  
**Focus**: Static methods, final variables

### 📝 Problem Statement
Create a utility class `TemperatureConverter` that provides methods to convert between different temperature units. The class should:
1. Be declared as `final` (cannot be extended)
2. Have a private constructor (cannot be instantiated)
3. Include static final variables for freezing and boiling points of water in Celsius
4. Provide static methods for:
   - Converting Celsius to Fahrenheit
   - Converting Fahrenheit to Celsius
   - Converting Celsius to Kelvin
   - Converting Kelvin to Celsius
5. Include a static method that tells if a given temperature (in any unit) is below freezing, above boiling, or in between

### 🔍 Example
```
32.0°F = 0.0°C
98.6°F = 37.0°C
100.0°C = 212.0°F
0.0°C = 273.15K
Water state at 20.0°C: Liquid
Water state at 110.0°C: Gas
Water state at -10.0°C: Solid
```

### 💡 Hints
<details>
<summary>Click for hints</summary>

1. Use the following conversion formulas:
   - F = C × 9/5 + 32
   - C = (F - 32) × 5/9
   - K = C + 273.15
2. Make the class final and constructor private to enforce utility class pattern
3. Use static final constants for freezing (0°C) and boiling (100°C) points
</details>

### ✅ Solution
```java
public final class TemperatureConverter {
    // Constants for water freezing and boiling points in Celsius
    public static final double FREEZING_POINT_C = 0.0;
    public static final double BOILING_POINT_C = 100.0;
    
    // Private constructor to prevent instantiation
    private TemperatureConverter() {
        // This prevents instantiation of the utility class
    }
    
    // Celsius to Fahrenheit
    public static double celsiusToFahrenheit(double celsius) {
        return celsius * 9.0/5.0 + 32;
    }
    
    // Fahrenheit to Celsius
    public static double fahrenheitToCelsius(double fahrenheit) {
        return (fahrenheit - 32) * 5.0/9.0;
    }
    
    // Celsius to Kelvin
    public static double celsiusToKelvin(double celsius) {
        return celsius + 273.15;
    }
    
    // Kelvin to Celsius
    public static double kelvinToCelsius(double kelvin) {
        return kelvin - 273.15;
    }
    
    // Determine water state based on temperature
    public static String getWaterState(double temperature, char unit) {
        // Convert to Celsius for comparison
        double celsius;
        switch (unit) {
            case 'C':
                celsius = temperature;
                break;
            case 'F':
                celsius = fahrenheitToCelsius(temperature);
                break;
            case 'K':
                celsius = kelvinToCelsius(temperature);
                break;
            default:
                return "Invalid unit";
        }
        
        // Determine state
        if (celsius < FREEZING_POINT_C) {
            return "Solid";
        } else if (celsius > BOILING_POINT_C) {
            return "Gas";
        } else {
            return "Liquid";
        }
    }
    
    public static void main(String[] args) {
        // Test conversions
        System.out.println("32.0°F = " + fahrenheitToCelsius(32.0) + "°C");
        System.out.println("98.6°F = " + fahrenheitToCelsius(98.6) + "°C");
        System.out.println("100.0°C = " + celsiusToFahrenheit(100.0) + "°F");
        System.out.println("0.0°C = " + celsiusToKelvin(0.0) + "K");
        
        // Test water states
        System.out.println("Water state at 20.0°C: " + getWaterState(20.0, 'C'));
        System.out.println("Water state at 110.0°C: " + getWaterState(110.0, 'C'));
        System.out.println("Water state at -10.0°C: " + getWaterState(-10.0, 'C'));
    }
}
```

### 🔄 Code Walkthrough
1. We create a final class that cannot be extended
2. We define static final constants for freezing and boiling points
3. We make the constructor private to prevent instantiation
4. We implement static methods for temperature conversions
5. We create a method to determine water state based on temperature
6. In the main method, we demonstrate the conversions and state determination

### 🚀 Extension Challenge
Add methods to convert to and from the Rankine temperature scale, and add a method that takes a temperature and returns the closest common reference point (e.g., "Room temperature", "Body temperature", "Freezing point of water").

---

## Problem 3: Shape Hierarchy

**Difficulty**: ⭐⭐ Intermediate  
**Estimated Time**: 25 minutes  
**Focus**: Inheritance, method overriding, super keyword

### 📝 Problem Statement
Create a shape hierarchy with the following classes:
1. `Shape` (parent class):
   - Has instance variables for `color` and `filled` (boolean)
   - Has a constructor that initializes these variables
   - Has getters and setters for all variables
   - Has a `toString()` method that returns shape information
   - Has an abstract `getArea()` method
   - Has an abstract `getPerimeter()` method

2. `Circle` (extends `Shape`):
   - Has an instance variable for `radius`
   - Has a constructor that calls the parent constructor
   - Implements `getArea()` and `getPerimeter()`
   - Overrides `toString()` to include circle-specific information

3. `Rectangle` (extends `Shape`):
   - Has instance variables for `width` and `length`
   - Has a constructor that calls the parent constructor
   - Implements `getArea()` and `getPerimeter()`
   - Overrides `toString()` to include rectangle-specific information

4. `Square` (extends `Rectangle`):
   - Has a constructor that calls the parent constructor with equal width and length
   - Overrides setters to ensure width and length are always equal
   - Overrides `toString()` to include square-specific information

### 🔍 Example
```
Circle: radius=5.0, color=red, filled=true
Area: 78.54, Perimeter: 31.42

Rectangle: width=4.0, length=6.0, color=blue, filled=false
Area: 24.00, Perimeter: 20.00

Square: side=3.0, color=green, filled=true
Area: 9.00, Perimeter: 12.00
```

### 💡 Hints
<details>
<summary>Click for hints</summary>

1. Use `super` to call the parent class constructor and methods
2. For the `Circle` class, use Math.PI for calculations
3. For the `Square` class, ensure that setting either width or length updates both
4. Format decimal outputs to two decimal places
</details>

### ✅ Solution
```java
// Base class
public abstract class Shape {
    private String color;
    private boolean filled;
    
    public Shape() {
        this("red", true);
    }
    
    public Shape(String color, boolean filled) {
        this.color = color;
        this.filled = filled;
    }
    
    public String getColor() {
        return color;
    }
    
    public void setColor(String color) {
        this.color = color;
    }
    
    public boolean isFilled() {
        return filled;
    }
    
    public void setFilled(boolean filled) {
        this.filled = filled;
    }
    
    public abstract double getArea();
    
    public abstract double getPerimeter();
    
    @Override
    public String toString() {
        return "color=" + color + ", filled=" + filled;
    }
}

// Circle class
public class Circle extends Shape {
    private double radius;
    
    public Circle() {
        this(1.0);
    }
    
    public Circle(double radius) {
        this(radius, "red", true);
    }
    
    public Circle(double radius, String color, boolean filled) {
        super(color, filled);
        this.radius = radius;
    }
    
    public double getRadius() {
        return radius;
    }
    
    public void setRadius(double radius) {
        this.radius = radius;
    }
    
    @Override
    public double getArea() {
        return Math.PI * radius * radius;
    }
    
    @Override
    public double getPerimeter() {
        return 2 * Math.PI * radius;
    }
    
    @Override
    public String toString() {
        return "Circle: radius=" + radius + ", " + super.toString();
    }
}

// Rectangle class
public class Rectangle extends Shape {
    private double width;
    private double length;
    
    public Rectangle() {
        this(1.0, 1.0);
    }
    
    public Rectangle(double width, double length) {
        this(width, length, "red", true);
    }
    
    public Rectangle(double width, double length, String color, boolean filled) {
        super(color, filled);
        this.width = width;
        this.length = length;
    }
    
    public double getWidth() {
        return width;
    }
    
    public void setWidth(double width) {
        this.width = width;
    }
    
    public double getLength() {
        return length;
    }
    
    public void setLength(double length) {
        this.length = length;
    }
    
    @Override
    public double getArea() {
        return width * length;
    }
    
    @Override
    public double getPerimeter() {
        return 2 * (width + length);
    }
    
    @Override
    public String toString() {
        return "Rectangle: width=" + width + ", length=" + length + ", " + super.toString();
    }
}

// Square class
public class Square extends Rectangle {
    public Square() {
        this(1.0);
    }
    
    public Square(double side) {
        this(side, "red", true);
    }
    
    public Square(double side, String color, boolean filled) {
        super(side, side, color, filled);
    }
    
    public double getSide() {
        return getWidth(); // Width and length are the same
    }
    
    public void setSide(double side) {
        setWidth(side);
        setLength(side);
    }
    
    @Override
    public void setWidth(double side) {
        super.setWidth(side);
        super.setLength(side);
    }
    
    @Override
    public void setLength(double side) {
        super.setWidth(side);
        super.setLength(side);
    }
    
    @Override
    public String toString() {
        return "Square: side=" + getSide() + ", " + super.toString().substring(super.toString().indexOf("color"));
    }
}

// Test class
public class ShapeTest {
    public static void main(String[] args) {
        // Create a circle
        Circle circle = new Circle(5.0, "red", true);
        System.out.println(circle);
        System.out.printf("Area: %.2f, Perimeter: %.2f\n\n", circle.getArea(), circle.getPerimeter());
        
        // Create a rectangle
        Rectangle rectangle = new Rectangle(4.0, 6.0, "blue", false);
        System.out.println(rectangle);
        System.out.printf("Area: %.2f, Perimeter: %.2f\n\n", rectangle.getArea(), rectangle.getPerimeter());
        
        // Create a square
        Square square = new Square(3.0, "green", true);
        System.out.println(square);
        System.out.printf("Area: %.2f, Perimeter: %.2f\n\n", square.getArea(), square.getPerimeter());
        
        // Demonstrate polymorphism
        Shape shape1 = new Circle(2.0);
        Shape shape2 = new Rectangle(2.0, 3.0);
        Shape shape3 = new Square(4.0);
        
        System.out.println("Using polymorphism:");
        System.out.printf("Shape 1 Area: %.2f\n", shape1.getArea());
        System.out.printf("Shape 2 Area: %.2f\n", shape2.getArea());
        System.out.printf("Shape 3 Area: %.2f\n", shape3.getArea());
    }
}
```

### 🔄 Code Walkthrough
1. We create an abstract `Shape` class with common properties and abstract methods
2. We implement `Circle` extending `Shape` with radius-specific properties
3. We implement `Rectangle` extending `Shape` with width and length properties
4. We implement `Square` extending `Rectangle`, ensuring width and length are always equal
5. Each class calls its parent constructor using `super`
6. Each class overrides `toString()` to include its specific properties
7. We demonstrate creating and using each shape type
8. We show polymorphism by storing different shapes in Shape variables

### 🚀 Extension Challenge
Add a `Triangle` class to the hierarchy that takes three sides as parameters. Implement the area calculation using Heron's formula and add validation to ensure the triangle is valid (sum of any two sides must be greater than the third side).

---

## Problem 4: Banking System

**Difficulty**: ⭐⭐⭐ Advanced  
**Estimated Time**: 30 minutes  
**Focus**: Inheritance hierarchy, method overriding, constructors

### 📝 Problem Statement
Implement a banking system with the following classes:

1. `BankAccount` (parent class):
   - Has instance variables for `accountNumber`, `accountHolder`, and `balance`
   - Has constructors, getters, and setters
   - Has methods `deposit()`, `withdraw()`, and `getBalance()`
   - Has a method `displayInfo()` to show account details

2. `SavingsAccount` (extends `BankAccount`):
   - Has an additional instance variable for `interestRate`
   - Overrides the parent constructor
   - Has a method `calculateInterest()` that returns the interest amount
   - Has a method `addInterest()` that adds interest to the balance
   - Overrides `displayInfo()` to include savings-specific information

3. `CheckingAccount` (extends `BankAccount`):
   - Has an additional instance variable for `overdraftLimit`
   - Overrides the parent constructor
   - Overrides `withdraw()` to allow withdrawals up to the overdraft limit
   - Overrides `displayInfo()` to include checking-specific information

4. `Bank` (manages accounts):
   - Has an array or collection to store multiple accounts
   - Has methods to add accounts, find accounts by number, and display all accounts
   - Has a method to perform transfers between accounts

### 🔍 Example
```
Created Savings Account: S001
Created Checking Account: C001

Initial Account Information:
Savings Account: S001
Account Holder: John Doe
Balance: $1000.00
Interest Rate: 3.50%

Checking Account: C001
Account Holder: Jane Smith
Balance: $2000.00
Overdraft Limit: $500.00

Depositing $500 to S001
New Balance: $1500.00

Adding interest to S001
Interest Added: $52.50
New Balance: $1552.50

Withdrawing $2300 from C001
Withdrawal successful (with overdraft)
New Balance: $-300.00

Transferring $500 from S001 to C001
Transfer successful
S001 New Balance: $1052.50
C001 New Balance: $200.00
```

### 💡 Hints
<details>
<summary>Click for hints</summary>

1. Use `super` to call the parent class constructor and methods
2. For the `SavingsAccount`, calculate interest as balance * interestRate / 100
3. For the `CheckingAccount`, allow withdrawals if amount <= balance + overdraftLimit
4. For the `Bank` class, use an ArrayList to store accounts for flexibility
5. Implement proper validation for all operations
</details>

### ✅ Solution
```java
import java.util.ArrayList;
import java.util.List;

// Base class
public class BankAccount {
    protected String accountNumber;
    protected String accountHolder;
    protected double balance;
    
    public BankAccount(String accountNumber, String accountHolder, double initialBalance) {
        this.accountNumber = accountNumber;
        this.accountHolder = accountHolder;
        this.balance = initialBalance;
    }
    
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
            System.out.printf("Deposited: $%.2f\n", amount);
        } else {
            System.out.println("Invalid deposit amount");
        }
    }
    
    public boolean withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.printf("Withdrawn: $%.2f\n", amount);
            return true;
        } else {
            System.out.println("Invalid withdrawal amount or insufficient funds");
            return false;
        }
    }
    
    public void displayInfo() {
        System.out.println("Account Number: " + accountNumber);
        System.out.println("Account Holder: " + accountHolder);
        System.out.printf("Balance: $%.2f\n", balance);
    }
}

// Savings Account
public class SavingsAccount extends BankAccount {
    private double interestRate; // Annual interest rate in percentage
    
    public SavingsAccount(String accountNumber, String accountHolder, 
                         double initialBalance, double interestRate) {
        super(accountNumber, accountHolder, initialBalance);
        this.interestRate = interestRate;
    }
    
    public double getInterestRate() {
        return interestRate;
    }
    
    public void setInterestRate(double interestRate) {
        this.interestRate = interestRate;
    }
    
    public double calculateInterest() {
        return balance * interestRate / 100;
    }
    
    public void addInterest() {
        double interest = calculateInterest();
        deposit(interest);
        System.out.printf("Interest Added: $%.2f\n", interest);
    }
    
    @Override
    public void displayInfo() {
        System.out.println("Savings Account: " + accountNumber);
        System.out.println("Account Holder: " + accountHolder);
        System.out.printf("Balance: $%.2f\n", balance);
        System.out.printf("Interest Rate: %.2f%%\n", interestRate);
    }
}

// Checking Account
public class CheckingAccount extends BankAccount {
    private double overdraftLimit;
    
    public CheckingAccount(String accountNumber, String accountHolder, 
                          double initialBalance, double overdraftLimit) {
        super(accountNumber, accountHolder, initialBalance);
        this.overdraftLimit = overdraftLimit;
    }
    
    public double getOverdraftLimit() {
        return overdraftLimit;
    }
    
    public void setOverdraftLimit(double overdraftLimit) {
        this.overdraftLimit = overdraftLimit;
    }
    
    @Override
    public boolean withdraw(double amount) {
        if (amount > 0 && amount <= (balance + overdraftLimit)) {
            balance -= amount;
            if (balance < 0) {
                System.out.printf("Withdrawn: $%.2f (with overdraft)\n", amount);
            } else {
                System.out.printf("Withdrawn: $%.2f\n", amount);
            }
            return true;
        } else {
            System.out.println("Invalid withdrawal amount or exceeds overdraft limit");
            return false;
        }
    }
    
    @Override
    public void displayInfo() {
        System.out.println("Checking Account: " + accountNumber);
        System.out.println("Account Holder: " + accountHolder);
        System.out.printf("Balance: $%.2f\n", balance);
        System.out.printf("Overdraft Limit: $%.2f\n", overdraftLimit);
    }
}

// Bank class to manage accounts
public class Bank {
    private List<BankAccount> accounts;
    
    public Bank() {
        accounts = new ArrayList<>();
    }
    
    public void addAccount(BankAccount account) {
        accounts.add(account);
        System.out.println("Created " + (account instanceof SavingsAccount ? "Savings" : "Checking") + 
                          " Account: " + account.getAccountNumber());
    }
    
    public BankAccount findAccount(String accountNumber) {
        for (BankAccount account : accounts) {
            if (account.getAccountNumber().equals(accountNumber)) {
                return account;
            }
        }
        return null;
    }
    
    public void displayAllAccounts() {
        System.out.println("\nAll Accounts:");
        for (BankAccount account : accounts) {
            account.displayInfo();
            System.out.println();
        }
    }
    
    public boolean transfer(String fromAccountNumber, String toAccountNumber, double amount) {
        BankAccount fromAccount = findAccount(fromAccountNumber);
        BankAccount toAccount = findAccount(toAccountNumber);
        
        if (fromAccount == null || toAccount == null) {
            System.out.println("One or both accounts not found");
            return false;
        }
        
        if (fromAccount.withdraw(amount)) {
            toAccount.deposit(amount);
            System.out.println("Transfer successful");
            System.out.printf("%s New Balance: $%.2f\n", fromAccountNumber, fromAccount.getBalance());
            System.out.printf("%s New Balance: $%.2f\n", toAccountNumber, toAccount.getBalance());
            return true;
        } else {
            System.out.println("Transfer failed");
            return false;
        }
    }
}

// Test class
public class BankingSystemTest {
    public static void main(String[] args) {
        // Create a bank
        Bank bank = new Bank();
        
        // Create accounts
        SavingsAccount savings = new SavingsAccount("S001", "John Doe", 1000, 3.5);
        CheckingAccount checking = new CheckingAccount("C001", "Jane Smith", 2000, 500);
        
        // Add accounts to bank
        bank.addAccount(savings);
        bank.addAccount(checking);
        
        System.out.println("\nInitial Account Information:");
        savings.displayInfo();
        System.out.println();
        checking.displayInfo();
        
        // Test operations
        System.out.println("\nDepositing $500 to S001");
        savings.deposit(500);
        System.out.printf("New Balance: $%.2f\n", savings.getBalance());
        
        System.out.println("\nAdding interest to S001");
        savings.addInterest();
        System.out.printf("New Balance: $%.2f\n", savings.getBalance());
        
        System.out.println("\nWithdrawing $2300 from C001");
        checking.withdraw(2300);
        System.out.printf("New Balance: $%.2f\n", checking.getBalance());
        
        System.out.println("\nTransferring $500 from S001 to C001");
        bank.transfer("S001", "C001", 500);
    }
}
```

### 🔄 Code Walkthrough
1. We create a base `BankAccount` class with common functionality
2. We implement `SavingsAccount` with interest calculation capabilities
3. We implement `CheckingAccount` with overdraft functionality
4. We create a `Bank` class to manage multiple accounts
5. Each subclass calls its parent constructor using `super`
6. Each subclass overrides methods as needed for specialized behavior
7. We demonstrate creating accounts, performing transactions, and managing accounts through the bank

### 🚀 Extension Challenge
Add a `FixedDepositAccount` class that:
- Has a lock-in period (in months)
- Doesn't allow withdrawals before the lock-in period ends
- Has a higher interest rate than regular savings accounts
- Applies a penalty for early withdrawals

---

## Problem 5: Vehicle Rental System

**Difficulty**: ⭐⭐⭐ Advanced  
**Estimated Time**: 35 minutes  
**Focus**: Inheritance hierarchy, polymorphism, static members

### 📝 Problem Statement
Design and implement a vehicle rental system with the following classes:

1. `Vehicle` (abstract parent class):
   - Has instance variables for `registrationNumber`, `brand`, `model`, and `dailyRate`
   - Has constructors, getters, and setters
   - Has an abstract method `calculateRentalCost(int days)`
   - Has a method `displayInfo()` to show vehicle details

2. `Car` (extends `Vehicle`):
   - Has additional instance variables for `numberOfSeats` and `fuelType`
   - Implements `calculateRentalCost()` (daily rate * days)
   - Overrides `displayInfo()` to include car-specific information

3. `Motorcycle` (extends `Vehicle`):
   - Has additional instance variables for `engineCapacity` (in cc)
   - Implements `calculateRentalCost()` (daily rate * days, with 10% discount for rentals > 7 days)
   - Overrides `displayInfo()` to include motorcycle-specific information

4. `Truck` (extends `Vehicle`):
   - Has additional instance variables for `cargoCapacity` (in tons)
   - Implements `calculateRentalCost()` (daily rate * days + additional fee based on cargo capacity)
   - Overrides `displayInfo()` to include truck-specific information

5. `RentalAgency` (manages vehicles):
   - Has a static variable for the agency name
   - Has an array or collection to store multiple vehicles
   - Has methods to add vehicles, find vehicles by registration number
   - Has a method to rent a vehicle for a specified number of days
   - Has a method to display all available vehicles

### 🔍 Example
```
Welcome to ABC Rental Agency

Available Vehicles:
Car: Toyota Camry (ABC123)
Seats: 5, Fuel: Gasoline
Daily Rate: $50.00

Motorcycle: Honda CBR (XYZ789)
Engine: 600cc
Daily Rate: $30.00

Truck: Ford F150 (TRK456)
Cargo Capacity: 2.5 tons
Daily Rate: $80.00

Renting Toyota Camry for 5 days
Rental Cost: $250.00

Renting Honda CBR for 10 days
Rental Cost: $270.00 (includes 10% discount)

Renting Ford F150 for 3 days
Rental Cost: $265.00 (includes cargo fee)
```

### 💡 Hints
<details>
<summary>Click for hints</summary>

1. Use an abstract class for `Vehicle` with common properties and methods
2. Implement the rental cost calculation differently for each vehicle type
3. Use polymorphism to store different vehicle types in the same collection
4. For the truck's rental cost, add a fee of $5 per ton of cargo capacity per day
5. Use static members for agency-wide information
</details>

### ✅ Solution
```java
import java.util.ArrayList;
import java.util.List;

// Abstract base class
public abstract class Vehicle {
    protected String registrationNumber;
    protected String brand;
    protected String model;
    protected double dailyRate;
    protected boolean isAvailable;
    
    public Vehicle(String registrationNumber, String brand, String model, double dailyRate) {
        this.registrationNumber = registrationNumber;
        this.brand = brand;
        this.model = model;
        this.dailyRate = dailyRate;
        this.isAvailable = true;
    }
    
    public String getRegistrationNumber() {
        return registrationNumber;
    }
    
    public String getBrand() {
        return brand;
    }
    
    public String getModel() {
        return model;
    }
    
    public double getDailyRate() {
        return dailyRate;
    }
    
    public boolean isAvailable() {
        return isAvailable;
    }
    
    public void setAvailable(boolean available) {
        isAvailable = available;
    }
    
    public abstract double calculateRentalCost(int days);
    
    public void displayInfo() {
        System.out.println(brand + " " + model + " (" + registrationNumber + ")");
        System.out.printf("Daily Rate: $%.2f\n", dailyRate);
    }
}

// Car class
public class Car extends Vehicle {
    private int numberOfSeats;
    private String fuelType;
    
    public Car(String registrationNumber, String brand, String model,
              double dailyRate, int numberOfSeats, String fuelType) {
        super(registrationNumber, brand, model, dailyRate);
        this.numberOfSeats = numberOfSeats;
        this.fuelType = fuelType;
    }
    
    public int getNumberOfSeats() {
        return numberOfSeats;
    }
    
    public String getFuelType() {
        return fuelType;
    }
    
    @Override
    public double calculateRentalCost(int days) {
        return dailyRate * days;
    }
    
    @Override
    public void displayInfo() {
        System.out.println("Car: " + getBrand() + " " + getModel() + " (" + getRegistrationNumber() + ")");
        System.out.println("Seats: " + numberOfSeats + ", Fuel: " + fuelType);
        System.out.printf("Daily Rate: $%.2f\n", dailyRate);
    }
}

// Motorcycle class
public class Motorcycle extends Vehicle {
    private int engineCapacity; // in cc
    
    public Motorcycle(String registrationNumber, String brand, String model,
                     double dailyRate, int engineCapacity) {
        super(registrationNumber, brand, model, dailyRate);
        this.engineCapacity = engineCapacity;
    }
    
    public int getEngineCapacity() {
        return engineCapacity;
    }
    
    @Override
    public double calculateRentalCost(int days) {
        double cost = dailyRate * days;
        // 10% discount for rentals longer than 7 days
        if (days > 7) {
            cost *= 0.9;
        }
        return cost;
    }
    
    @Override
    public void displayInfo() {
        System.out.println("Motorcycle: " + getBrand() + " " + getModel() + " (" + getRegistrationNumber() + ")");
        System.out.println("Engine: " + engineCapacity + "cc");
        System.out.printf("Daily Rate: $%.2f\n", dailyRate);
    }
}

// Truck class
public class Truck extends Vehicle {
    private double cargoCapacity; // in tons
    
    public Truck(String registrationNumber, String brand, String model,
                double dailyRate, double cargoCapacity) {
        super(registrationNumber, brand, model, dailyRate);
        this.cargoCapacity = cargoCapacity;
    }
    
    public double getCargoCapacity() {
        return cargoCapacity;
    }
    
    @Override
    public double calculateRentalCost(int days) {
        // Base cost plus additional fee based on cargo capacity
        double cargoFee = 5 * cargoCapacity * days; // $5 per ton per day
        return (dailyRate * days) + cargoFee;
    }
    
    @Override
    public void displayInfo() {
        System.out.println("Truck: " + getBrand() + " " + getModel() + " (" + getRegistrationNumber() + ")");
        System.out.println("Cargo Capacity: " + cargoCapacity + " tons");
        System.out.printf("Daily Rate: $%.2f\n", dailyRate);
    }
}

// Rental Agency class
public class RentalAgency {
    private static String agencyName;
    private List<Vehicle> vehicles;
    
    public RentalAgency(String name) {
        agencyName = name;
        vehicles = new ArrayList<>();
    }
    
    public static String getAgencyName() {
        return agencyName;
    }
    
    public void addVehicle(Vehicle vehicle) {
        vehicles.add(vehicle);
    }
    
    public Vehicle findVehicle(String registrationNumber) {
        for (Vehicle vehicle : vehicles) {
            if (vehicle.getRegistrationNumber().equals(registrationNumber)) {
                return vehicle;
            }
        }
        return null;
    }
    
    public void displayAvailableVehicles() {
        System.out.println("Available Vehicles:");
        for (Vehicle vehicle : vehicles) {
            if (vehicle.isAvailable()) {
                vehicle.displayInfo();
                System.out.println();
            }
        }
    }
    
    public double rentVehicle(String registrationNumber, int days) {
        Vehicle vehicle = findVehicle(registrationNumber);
        if (vehicle != null && vehicle.isAvailable()) {
            vehicle.setAvailable(false);
            double cost = vehicle.calculateRentalCost(days);
            
            System.out.println("Renting " + vehicle.getBrand() + " " + vehicle.getModel() + " for " + days + " days");
            
            if (vehicle instanceof Motorcycle && days > 7) {
                System.out.printf("Rental Cost: $%.2f (includes 10%% discount)\n", cost);
            } else if (vehicle instanceof Truck) {
                System.out.printf("Rental Cost: $%.2f (includes cargo fee)\n", cost);
            } else {
                System.out.printf("Rental Cost: $%.2f\n", cost);
            }
            
            return cost;
        } else {
            System.out.println("Vehicle not available or not found");
            return 0;
        }
    }
}

// Test class
public class VehicleRentalSystemTest {
    public static void main(String[] args) {
        // Create a rental agency
        RentalAgency agency = new RentalAgency("ABC Rental Agency");
        
        // Add vehicles
        Car car = new Car("ABC123", "Toyota", "Camry", 50, 5, "Gasoline");
        Motorcycle motorcycle = new Motorcycle("XYZ789", "Honda", "CBR", 30, 600);
        Truck truck = new Truck("TRK456", "Ford", "F150", 80, 2.5);
        
        agency.addVehicle(car);
        agency.addVehicle(motorcycle);
        agency.addVehicle(truck);
        
        // Display agency information
        System.out.println("Welcome to " + RentalAgency.getAgencyName());
        System.out.println();
        
        // Display available vehicles
        agency.displayAvailableVehicles();
        
        // Rent vehicles
        System.out.println();
        agency.rentVehicle("ABC123", 5);
        
        System.out.println();
        agency.rentVehicle("XYZ789", 10);
        
        System.out.println();
        agency.rentVehicle("TRK456", 3);
    }
}
```

### 🔄 Code Walkthrough
1. We create an abstract `Vehicle` class with common properties and an abstract method for rental cost calculation
2. We implement three vehicle types with specialized properties and rental cost calculations:
   - `Car`: Basic rental cost (daily rate * days)
   - `Motorcycle`: Includes a 10% discount for rentals longer than 7 days
   - `Truck`: Adds a cargo fee based on capacity
3. We create a `RentalAgency` class to manage vehicles and handle rentals
4. We use polymorphism to store different vehicle types in the same collection
5. We demonstrate creating vehicles, displaying information, and calculating rental costs

### 🚀 Extension Challenge
Add a `LuxuryVehicle` interface that can be implemented by any vehicle type. This interface should add a `calculateInsurance()` method and a `securityDeposit` property. Implement this interface for a luxury car and a luxury motorcycle.

---

## 📚 Key Concepts Practiced

1. **Constructors**
   - Default and parameterized constructors
   - Constructor overloading
   - Constructor chaining with `this()`
   - Calling parent constructors with `super()`

2. **Static Members**
   - Static variables shared across instances
   - Static methods for utility functions
   - Static constants for configuration values
   - Static initialization blocks

3. **Inheritance**
   - Creating parent-child relationships
   - Method overriding with `@Override`
   - Using `super` to access parent methods
   - Implementing abstract methods

4. **Final Keyword**
   - Final variables as constants
   - Final methods that cannot be overridden
   - Final classes that cannot be extended

5. **Polymorphism**
   - Storing subclass objects in parent class references
   - Runtime method resolution based on actual object type
   - Designing flexible and extensible systems

## 🔍 Further Exploration
- Research the concept of composition as an alternative to inheritance
- Explore the use of interfaces for multiple inheritance in Java
- Learn about the Liskov Substitution Principle and how it relates to inheritance
- Study design patterns that leverage inheritance and polymorphism