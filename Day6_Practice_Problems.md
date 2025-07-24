# Day 6: Practice Problems - Interfaces & Packages

## 🎯 Practice Objectives

By completing these practice problems, you will be able to:
- Define and implement interfaces in Java
- Use default and static methods in interfaces
- Implement multiple interfaces
- Work with functional interfaces and lambda expressions
- Create and use custom packages
- Apply access modifiers appropriately
- Use import statements effectively
- Organize code using packages

## 🔄 Topics Covered

- Interfaces
- Default and static methods
- Multiple interface implementation
- Functional interfaces and lambda expressions
- Packages
- Access modifiers
- Import statements

---

## 🧩 Practice Problem 1: Basic Interface Implementation

### Problem Statement
Create a `Shape` interface with methods for calculating area and perimeter. Then implement this interface for different shapes: `Circle`, `Rectangle`, and `Triangle`.

### Starter Code
```java
// Create the Shape interface here

// Implement the Circle class here

// Implement the Rectangle class here

// Implement the Triangle class here

public class ShapeTest {
    public static void main(String[] args) {
        Shape[] shapes = new Shape[3];
        shapes[0] = new Circle(5.0);
        shapes[1] = new Rectangle(4.0, 6.0);
        shapes[2] = new Triangle(3.0, 4.0, 5.0);
        
        for (Shape shape : shapes) {
            System.out.println("Area: " + shape.calculateArea());
            System.out.println("Perimeter: " + shape.calculatePerimeter());
            System.out.println();
        }
    }
}
```

### Expected Output
```
Area: 78.53981633974483
Perimeter: 31.41592653589793

Area: 24.0
Perimeter: 20.0

Area: 6.0
Perimeter: 12.0
```

### 💡 Hints
<details>
<summary>Hint 1</summary>
The Shape interface should declare two methods: calculateArea() and calculatePerimeter().
</details>

<details>
<summary>Hint 2</summary>
For the Triangle class, you can use Heron's formula to calculate the area: sqrt(s * (s-a) * (s-b) * (s-c)) where s = (a+b+c)/2.
</details>

### ✅ Solution
<details>
<summary>Solution</summary>

```java
// Shape interface
interface Shape {
    double calculateArea();
    double calculatePerimeter();
}

// Circle implementation
class Circle implements Shape {
    private double radius;
    
    public Circle(double radius) {
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
}

// Rectangle implementation
class Rectangle implements Shape {
    private double width;
    private double height;
    
    public Rectangle(double width, double height) {
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
}

// Triangle implementation
class Triangle implements Shape {
    private double a;
    private double b;
    private double c;
    
    public Triangle(double a, double b, double c) {
        this.a = a;
        this.b = b;
        this.c = c;
    }
    
    @Override
    public double calculateArea() {
        // Using Heron's formula
        double s = (a + b + c) / 2;
        return Math.sqrt(s * (s - a) * (s - b) * (s - c));
    }
    
    @Override
    public double calculatePerimeter() {
        return a + b + c;
    }
}

public class ShapeTest {
    public static void main(String[] args) {
        Shape[] shapes = new Shape[3];
        shapes[0] = new Circle(5.0);
        shapes[1] = new Rectangle(4.0, 6.0);
        shapes[2] = new Triangle(3.0, 4.0, 5.0);
        
        for (Shape shape : shapes) {
            System.out.println("Area: " + shape.calculateArea());
            System.out.println("Perimeter: " + shape.calculatePerimeter());
            System.out.println();
        }
    }
}
```
</details>

---

## 🧩 Practice Problem 2: Default and Static Methods in Interfaces

### Problem Statement
Create a `Vehicle` interface with abstract methods for `start()` and `stop()`, a default method for `honk()`, and a static method for `getVehicleInfo()`. Then implement this interface for different vehicle types: `Car`, `Motorcycle`, and `Truck`.

### Starter Code
```java
// Create the Vehicle interface here

// Implement the Car class here

// Implement the Motorcycle class here

// Implement the Truck class here

public class VehicleTest {
    public static void main(String[] args) {
        // Print the vehicle info using the static method
        Vehicle.getVehicleInfo();
        
        // Create and test vehicles
        Vehicle[] vehicles = new Vehicle[3];
        vehicles[0] = new Car("Toyota", "Camry");
        vehicles[1] = new Motorcycle("Honda", "CBR");
        vehicles[2] = new Truck("Ford", "F-150", 5000);
        
        for (Vehicle vehicle : vehicles) {
            System.out.println("\nTesting vehicle:");
            vehicle.start();
            vehicle.honk();  // Using the default method
            vehicle.stop();
        }
    }
}
```

### Expected Output
```
Vehicle Information: Vehicles are means of transport that can start, stop, and honk.

Testing vehicle:
Starting Toyota Camry's engine.
Beep beep!
Stopping Toyota Camry's engine.

Testing vehicle:
Kickstarting Honda CBR's engine.
Beep beep!
Stopping Honda CBR's engine.

Testing vehicle:
Starting Ford F-150's diesel engine.
HOOOONK!
Stopping Ford F-150's engine and applying air brakes.
```

### 💡 Hints
<details>
<summary>Hint 1</summary>
Use the `default` keyword to define the default honk() method in the interface.
</details>

<details>
<summary>Hint 2</summary>
Use the `static` keyword to define the static getVehicleInfo() method in the interface.
</details>

<details>
<summary>Hint 3</summary>
The Truck class should override the default honk() method to provide a louder honk.
</details>

### ✅ Solution
<details>
<summary>Solution</summary>

```java
// Vehicle interface
interface Vehicle {
    void start();
    void stop();
    
    // Default method
    default void honk() {
        System.out.println("Beep beep!");
    }
    
    // Static method
    static void getVehicleInfo() {
        System.out.println("Vehicle Information: Vehicles are means of transport that can start, stop, and honk.");
    }
}

// Car implementation
class Car implements Vehicle {
    private String make;
    private String model;
    
    public Car(String make, String model) {
        this.make = make;
        this.model = model;
    }
    
    @Override
    public void start() {
        System.out.println("Starting " + make + " " + model + "'s engine.");
    }
    
    @Override
    public void stop() {
        System.out.println("Stopping " + make + " " + model + "'s engine.");
    }
}

// Motorcycle implementation
class Motorcycle implements Vehicle {
    private String make;
    private String model;
    
    public Motorcycle(String make, String model) {
        this.make = make;
        this.model = model;
    }
    
    @Override
    public void start() {
        System.out.println("Kickstarting " + make + " " + model + "'s engine.");
    }
    
    @Override
    public void stop() {
        System.out.println("Stopping " + make + " " + model + "'s engine.");
    }
}

// Truck implementation
class Truck implements Vehicle {
    private String make;
    private String model;
    private int loadCapacity;
    
    public Truck(String make, String model, int loadCapacity) {
        this.make = make;
        this.model = model;
        this.loadCapacity = loadCapacity;
    }
    
    @Override
    public void start() {
        System.out.println("Starting " + make + " " + model + "'s diesel engine.");
    }
    
    @Override
    public void stop() {
        System.out.println("Stopping " + make + " " + model + "'s engine and applying air brakes.");
    }
    
    // Override the default method
    @Override
    public void honk() {
        System.out.println("HOOOONK!");
    }
}

public class VehicleTest {
    public static void main(String[] args) {
        // Print the vehicle info using the static method
        Vehicle.getVehicleInfo();
        
        // Create and test vehicles
        Vehicle[] vehicles = new Vehicle[3];
        vehicles[0] = new Car("Toyota", "Camry");
        vehicles[1] = new Motorcycle("Honda", "CBR");
        vehicles[2] = new Truck("Ford", "F-150", 5000);
        
        for (Vehicle vehicle : vehicles) {
            System.out.println("\nTesting vehicle:");
            vehicle.start();
            vehicle.honk();  // Using the default method
            vehicle.stop();
        }
    }
}
```
</details>

---

## 🧩 Practice Problem 3: Multiple Interface Implementation

### Problem Statement
Create three interfaces: `Swimmer`, `Flyer`, and `Runner`, each with their own methods. Then create classes for different animals that implement one or more of these interfaces: `Duck` (can swim and fly), `Penguin` (can swim and run), and `Eagle` (can fly and run).

### Starter Code
```java
// Create the Swimmer interface here

// Create the Flyer interface here

// Create the Runner interface here

// Implement the Duck class here

// Implement the Penguin class here

// Implement the Eagle class here

public class AnimalTest {
    public static void main(String[] args) {
        System.out.println("Testing Duck:");
        Duck duck = new Duck();
        testSwimmer(duck);
        testFlyer(duck);
        
        System.out.println("\nTesting Penguin:");
        Penguin penguin = new Penguin();
        testSwimmer(penguin);
        testRunner(penguin);
        
        System.out.println("\nTesting Eagle:");
        Eagle eagle = new Eagle();
        testFlyer(eagle);
        testRunner(eagle);
    }
    
    public static void testSwimmer(Swimmer swimmer) {
        System.out.println("Testing swimming abilities:");
        swimmer.swim();
        swimmer.dive();
    }
    
    public static void testFlyer(Flyer flyer) {
        System.out.println("Testing flying abilities:");
        flyer.fly();
        flyer.land();
    }
    
    public static void testRunner(Runner runner) {
        System.out.println("Testing running abilities:");
        runner.run();
        runner.sprint();
    }
}
```

### Expected Output
```
Testing Duck:
Testing swimming abilities:
Duck is swimming
Duck is diving underwater
Testing flying abilities:
Duck is flying
Duck is landing on water

Testing Penguin:
Testing swimming abilities:
Penguin is swimming fast
Penguin is diving deep underwater
Testing running abilities:
Penguin is running on ice
Penguin is waddling quickly

Testing Eagle:
Testing flying abilities:
Eagle is soaring high
Eagle is landing on a tree branch
Testing running abilities:
Eagle is running on the ground
Eagle is sprinting to catch prey
```

### 💡 Hints
<details>
<summary>Hint 1</summary>
Each interface should have at least two methods (e.g., Swimmer should have swim() and dive()).
</details>

<details>
<summary>Hint 2</summary>
Use the implements keyword with multiple interfaces separated by commas (e.g., class Duck implements Swimmer, Flyer).
</details>

<details>
<summary>Hint 3</summary>
Make each animal's implementation unique to reflect its natural abilities.
</details>

### ✅ Solution
<details>
<summary>Solution</summary>

```java
// Swimmer interface
interface Swimmer {
    void swim();
    void dive();
}

// Flyer interface
interface Flyer {
    void fly();
    void land();
}

// Runner interface
interface Runner {
    void run();
    void sprint();
}

// Duck class implements Swimmer and Flyer
class Duck implements Swimmer, Flyer {
    @Override
    public void swim() {
        System.out.println("Duck is swimming");
    }
    
    @Override
    public void dive() {
        System.out.println("Duck is diving underwater");
    }
    
    @Override
    public void fly() {
        System.out.println("Duck is flying");
    }
    
    @Override
    public void land() {
        System.out.println("Duck is landing on water");
    }
}

// Penguin class implements Swimmer and Runner
class Penguin implements Swimmer, Runner {
    @Override
    public void swim() {
        System.out.println("Penguin is swimming fast");
    }
    
    @Override
    public void dive() {
        System.out.println("Penguin is diving deep underwater");
    }
    
    @Override
    public void run() {
        System.out.println("Penguin is running on ice");
    }
    
    @Override
    public void sprint() {
        System.out.println("Penguin is waddling quickly");
    }
}

// Eagle class implements Flyer and Runner
class Eagle implements Flyer, Runner {
    @Override
    public void fly() {
        System.out.println("Eagle is soaring high");
    }
    
    @Override
    public void land() {
        System.out.println("Eagle is landing on a tree branch");
    }
    
    @Override
    public void run() {
        System.out.println("Eagle is running on the ground");
    }
    
    @Override
    public void sprint() {
        System.out.println("Eagle is sprinting to catch prey");
    }
}

public class AnimalTest {
    public static void main(String[] args) {
        System.out.println("Testing Duck:");
        Duck duck = new Duck();
        testSwimmer(duck);
        testFlyer(duck);
        
        System.out.println("\nTesting Penguin:");
        Penguin penguin = new Penguin();
        testSwimmer(penguin);
        testRunner(penguin);
        
        System.out.println("\nTesting Eagle:");
        Eagle eagle = new Eagle();
        testFlyer(eagle);
        testRunner(eagle);
    }
    
    public static void testSwimmer(Swimmer swimmer) {
        System.out.println("Testing swimming abilities:");
        swimmer.swim();
        swimmer.dive();
    }
    
    public static void testFlyer(Flyer flyer) {
        System.out.println("Testing flying abilities:");
        flyer.fly();
        flyer.land();
    }
    
    public static void testRunner(Runner runner) {
        System.out.println("Testing running abilities:");
        runner.run();
        runner.sprint();
    }
}
```
</details>

---

## 🧩 Practice Problem 4: Functional Interfaces and Lambda Expressions

### Problem Statement
Create a functional interface called `StringOperation` with a method that takes a string and returns a string. Then use lambda expressions to create different string operations: converting to uppercase, reversing the string, and removing spaces.

### Starter Code
```java
// Create the StringOperation functional interface here

public class StringOperationTest {
    public static void main(String[] args) {
        // Create lambda expressions for different string operations
        StringOperation toUpperCase = // Your lambda expression here
        StringOperation reverse = // Your lambda expression here
        StringOperation removeSpaces = // Your lambda expression here
        
        // Test string
        String test = "Hello World";
        
        // Apply and print the results of each operation
        System.out.println("Original: " + test);
        System.out.println("Uppercase: " + operate(test, toUpperCase));
        System.out.println("Reversed: " + operate(test, reverse));
        System.out.println("No spaces: " + operate(test, removeSpaces));
        
        // Create and test a composed operation (first remove spaces, then uppercase)
        StringOperation noSpacesAndUppercase = // Your composed operation here
        System.out.println("No spaces + Uppercase: " + operate(test, noSpacesAndUppercase));
    }
    
    // Method that applies a StringOperation to a string
    public static String operate(String str, StringOperation operation) {
        return operation.apply(str);
    }
}
```

### Expected Output
```
Original: Hello World
Uppercase: HELLO WORLD
Reversed: dlroW olleH
No spaces: HelloWorld
No spaces + Uppercase: HELLOWORLD
```

### 💡 Hints
<details>
<summary>Hint 1</summary>
Use the @FunctionalInterface annotation to mark your interface as a functional interface.
</details>

<details>
<summary>Hint 2</summary>
For the reverse operation, you can use StringBuilder's reverse() method.
</details>

<details>
<summary>Hint 3</summary>
For the composed operation, you can use a lambda that applies one operation and then the other.
</details>

### ✅ Solution
<details>
<summary>Solution</summary>

```java
// Functional interface
@FunctionalInterface
interface StringOperation {
    String apply(String str);
}

public class StringOperationTest {
    public static void main(String[] args) {
        // Create lambda expressions for different string operations
        StringOperation toUpperCase = s -> s.toUpperCase();
        StringOperation reverse = s -> new StringBuilder(s).reverse().toString();
        StringOperation removeSpaces = s -> s.replaceAll("\\s+", "");
        
        // Test string
        String test = "Hello World";
        
        // Apply and print the results of each operation
        System.out.println("Original: " + test);
        System.out.println("Uppercase: " + operate(test, toUpperCase));
        System.out.println("Reversed: " + operate(test, reverse));
        System.out.println("No spaces: " + operate(test, removeSpaces));
        
        // Create and test a composed operation (first remove spaces, then uppercase)
        StringOperation noSpacesAndUppercase = s -> toUpperCase.apply(removeSpaces.apply(s));
        System.out.println("No spaces + Uppercase: " + operate(test, noSpacesAndUppercase));
    }
    
    // Method that applies a StringOperation to a string
    public static String operate(String str, StringOperation operation) {
        return operation.apply(str);
    }
}
```
</details>

---

## 🧩 Practice Problem 5: Creating and Using Packages

### Problem Statement
Create a simple banking system with the following package structure:
- `com.bank.model`: Contains the `Account` and `Customer` classes
- `com.bank.service`: Contains the `BankService` class for operations like deposit, withdraw, and transfer
- `com.bank.util`: Contains the `ValidationUtil` class for validating inputs

### Starter Code
```java
// File: com/bank/model/Account.java
package com.bank.model;

// Implement the Account class here

// File: com/bank/model/Customer.java
package com.bank.model;

// Implement the Customer class here

// File: com/bank/service/BankService.java
package com.bank.service;

// Import necessary classes
// ...

// Implement the BankService class here

// File: com/bank/util/ValidationUtil.java
package com.bank.util;

// Implement the ValidationUtil class here

// File: com/bank/Main.java
package com.bank;

// Import necessary classes
// ...

public class Main {
    public static void main(String[] args) {
        // Create customers and accounts
        Customer customer1 = new Customer("John", "Doe", "123456789");
        Customer customer2 = new Customer("Jane", "Smith", "987654321");
        
        Account account1 = new Account("A001", customer1, 1000.0);
        Account account2 = new Account("A002", customer2, 2000.0);
        
        // Create bank service
        BankService bankService = new BankService();
        
        // Perform operations
        System.out.println("Initial state:");
        System.out.println(account1);
        System.out.println(account2);
        
        System.out.println("\nAfter deposit:");
        bankService.deposit(account1, 500.0);
        System.out.println(account1);
        
        System.out.println("\nAfter withdrawal:");
        bankService.withdraw(account1, 200.0);
        System.out.println(account1);
        
        System.out.println("\nAfter transfer:");
        bankService.transfer(account1, account2, 300.0);
        System.out.println(account1);
        System.out.println(account2);
        
        // Test validation
        System.out.println("\nValidation tests:");
        try {
            bankService.withdraw(account1, -100.0);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
        
        try {
            bankService.withdraw(account1, 5000.0);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

### Expected Output
```
Initial state:
Account{accountNumber='A001', customer=Customer{firstName='John', lastName='Doe'}, balance=1000.0}
Account{accountNumber='A002', customer=Customer{firstName='Jane', lastName='Smith'}, balance=2000.0}

After deposit:
Account{accountNumber='A001', customer=Customer{firstName='John', lastName='Doe'}, balance=1500.0}

After withdrawal:
Account{accountNumber='A001', customer=Customer{firstName='John', lastName='Doe'}, balance=1300.0}

After transfer:
Account{accountNumber='A001', customer=Customer{firstName='John', lastName='Doe'}, balance=1000.0}
Account{accountNumber='A002', customer=Customer{firstName='Jane', lastName='Smith'}, balance=2300.0}

Validation tests:
Error: Amount must be positive
Error: Insufficient funds
```

### 💡 Hints
<details>
<summary>Hint 1</summary>
Use appropriate access modifiers for your classes and methods. Make fields private and provide public getters and setters as needed.
</details>

<details>
<summary>Hint 2</summary>
In the ValidationUtil class, create static methods for validating amounts and balances.
</details>

<details>
<summary>Hint 3</summary>
In the BankService class, use the ValidationUtil methods to validate inputs before performing operations.
</details>

### ✅ Solution
<details>
<summary>Solution</summary>

```java
// File: com/bank/model/Account.java
package com.bank.model;

public class Account {
    private String accountNumber;
    private Customer customer;
    private double balance;
    
    public Account(String accountNumber, Customer customer, double balance) {
        this.accountNumber = accountNumber;
        this.customer = customer;
        this.balance = balance;
    }
    
    public String getAccountNumber() {
        return accountNumber;
    }
    
    public Customer getCustomer() {
        return customer;
    }
    
    public double getBalance() {
        return balance;
    }
    
    public void setBalance(double balance) {
        this.balance = balance;
    }
    
    @Override
    public String toString() {
        return "Account{" +
                "accountNumber='" + accountNumber + '\'' +
                ", customer=" + customer +
                ", balance=" + balance +
                '}';
    }
}

// File: com/bank/model/Customer.java
package com.bank.model;

public class Customer {
    private String firstName;
    private String lastName;
    private String ssn;
    
    public Customer(String firstName, String lastName, String ssn) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.ssn = ssn;
    }
    
    public String getFirstName() {
        return firstName;
    }
    
    public String getLastName() {
        return lastName;
    }
    
    public String getSsn() {
        return ssn;
    }
    
    @Override
    public String toString() {
        return "Customer{" +
                "firstName='" + firstName + '\'' +
                ", lastName='" + lastName + '\'' +
                '}';
    }
}

// File: com/bank/service/BankService.java
package com.bank.service;

import com.bank.model.Account;
import com.bank.util.ValidationUtil;

public class BankService {
    public void deposit(Account account, double amount) {
        ValidationUtil.validateAmount(amount);
        account.setBalance(account.getBalance() + amount);
    }
    
    public void withdraw(Account account, double amount) {
        ValidationUtil.validateAmount(amount);
        ValidationUtil.validateSufficientFunds(account, amount);
        account.setBalance(account.getBalance() - amount);
    }
    
    public void transfer(Account fromAccount, Account toAccount, double amount) {
        withdraw(fromAccount, amount);
        deposit(toAccount, amount);
    }
}

// File: com/bank/util/ValidationUtil.java
package com.bank.util;

import com.bank.model.Account;

public class ValidationUtil {
    private ValidationUtil() {
        // Private constructor to prevent instantiation
    }
    
    public static void validateAmount(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Amount must be positive");
        }
    }
    
    public static void validateSufficientFunds(Account account, double amount) {
        if (account.getBalance() < amount) {
            throw new IllegalArgumentException("Insufficient funds");
        }
    }
}

// File: com/bank/Main.java
package com.bank;

import com.bank.model.Account;
import com.bank.model.Customer;
import com.bank.service.BankService;

public class Main {
    public static void main(String[] args) {
        // Create customers and accounts
        Customer customer1 = new Customer("John", "Doe", "123456789");
        Customer customer2 = new Customer("Jane", "Smith", "987654321");
        
        Account account1 = new Account("A001", customer1, 1000.0);
        Account account2 = new Account("A002", customer2, 2000.0);
        
        // Create bank service
        BankService bankService = new BankService();
        
        // Perform operations
        System.out.println("Initial state:");
        System.out.println(account1);
        System.out.println(account2);
        
        System.out.println("\nAfter deposit:");
        bankService.deposit(account1, 500.0);
        System.out.println(account1);
        
        System.out.println("\nAfter withdrawal:");
        bankService.withdraw(account1, 200.0);
        System.out.println(account1);
        
        System.out.println("\nAfter transfer:");
        bankService.transfer(account1, account2, 300.0);
        System.out.println(account1);
        System.out.println(account2);
        
        // Test validation
        System.out.println("\nValidation tests:");
        try {
            bankService.withdraw(account1, -100.0);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
        
        try {
            bankService.withdraw(account1, 5000.0);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```
</details>

---

## 🏆 Challenge Problem: Media Player with Plugins

### Problem Statement
Create a media player application that uses interfaces and packages to support plugins. The application should have the following structure:

1. A `MediaPlayer` interface with methods for playing, pausing, and stopping media.
2. A `Plugin` interface that can be implemented by different plugins.
3. Concrete implementations of the `MediaPlayer` interface for different media types (audio, video).
4. Concrete implementations of the `Plugin` interface for different functionalities (equalizer, visualizer, subtitle).
5. A package structure that organizes these components logically.

### Starter Code
```java
// File: com/mediaplayer/core/MediaPlayer.java
package com.mediaplayer.core;

// Create the MediaPlayer interface here

// File: com/mediaplayer/core/Plugin.java
package com.mediaplayer.core;

// Create the Plugin interface here

// File: com/mediaplayer/players/AudioPlayer.java
package com.mediaplayer.players;

// Import necessary classes
// ...

// Implement the AudioPlayer class here

// File: com/mediaplayer/players/VideoPlayer.java
package com.mediaplayer.players;

// Import necessary classes
// ...

// Implement the VideoPlayer class here

// File: com/mediaplayer/plugins/EqualizerPlugin.java
package com.mediaplayer.plugins;

// Import necessary classes
// ...

// Implement the EqualizerPlugin class here

// File: com/mediaplayer/plugins/VisualizerPlugin.java
package com.mediaplayer.plugins;

// Import necessary classes
// ...

// Implement the VisualizerPlugin class here

// File: com/mediaplayer/plugins/SubtitlePlugin.java
package com.mediaplayer.plugins;

// Import necessary classes
// ...

// Implement the SubtitlePlugin class here

// File: com/mediaplayer/Main.java
package com.mediaplayer;

// Import necessary classes
// ...

public class Main {
    public static void main(String[] args) {
        // Create media players
        MediaPlayer audioPlayer = new AudioPlayer("Song.mp3");
        MediaPlayer videoPlayer = new VideoPlayer("Movie.mp4");
        
        // Create plugins
        Plugin equalizer = new EqualizerPlugin();
        Plugin visualizer = new VisualizerPlugin();
        Plugin subtitle = new SubtitlePlugin();
        
        // Add plugins to players
        audioPlayer.addPlugin(equalizer);
        audioPlayer.addPlugin(visualizer);
        
        videoPlayer.addPlugin(equalizer);
        videoPlayer.addPlugin(subtitle);
        
        // Test audio player
        System.out.println("Testing Audio Player:");
        audioPlayer.play();
.activatePlugins();
        audioPlayer.pause();
        audioPlayer.stop();
        
        // Test video player
        System.out.println("\nTesting Video Player:");
        videoPlayer.play();
        videoPlayer.activatePlugins();
        videoPlayer.pause();
        videoPlayer.stop();
    }
}
```

### 💡 Hints
<details>
<summary>Hint 1</summary>
The MediaPlayer interface should include methods for play(), pause(), stop(), addPlugin(), and activatePlugins().
</details>

<details>
<summary>Hint 2</summary>
The Plugin interface should have an activate() method that will be called by the media player.
</details>

<details>
<summary>Hint 3</summary>
Each media player implementation should maintain a list of plugins and activate them when requested.
</details>

### ✅ Solution
<details>
<summary>Solution</summary>

```java
// File: com/mediaplayer/core/MediaPlayer.java
package com.mediaplayer.core;

public interface MediaPlayer {
    void play();
    void pause();
    void stop();
    void addPlugin(Plugin plugin);
    void activatePlugins();
}

// File: com/mediaplayer/core/Plugin.java
package com.mediaplayer.core;

public interface Plugin {
    void activate(String mediaType, String mediaName);
    String getName();
}

// File: com/mediaplayer/players/AudioPlayer.java
package com.mediaplayer.players;

import com.mediaplayer.core.MediaPlayer;
import com.mediaplayer.core.Plugin;
import java.util.ArrayList;
import java.util.List;

public class AudioPlayer implements MediaPlayer {
    private String audioFile;
    private List<Plugin> plugins;
    
    public AudioPlayer(String audioFile) {
        this.audioFile = audioFile;
        this.plugins = new ArrayList<>();
    }
    
    @Override
    public void play() {
        System.out.println("Playing audio: " + audioFile);
    }
    
    @Override
    public void pause() {
        System.out.println("Pausing audio: " + audioFile);
    }
    
    @Override
    public void stop() {
        System.out.println("Stopping audio: " + audioFile);
    }
    
    @Override
    public void addPlugin(Plugin plugin) {
        plugins.add(plugin);
        System.out.println("Added " + plugin.getName() + " plugin to audio player");
    }
    
    @Override
    public void activatePlugins() {
        System.out.println("Activating plugins for audio player:");
        for (Plugin plugin : plugins) {
            plugin.activate("audio", audioFile);
        }
    }
}

// File: com/mediaplayer/players/VideoPlayer.java
package com.mediaplayer.players;

import com.mediaplayer.core.MediaPlayer;
import com.mediaplayer.core.Plugin;
import java.util.ArrayList;
import java.util.List;

public class VideoPlayer implements MediaPlayer {
    private String videoFile;
    private List<Plugin> plugins;
    
    public VideoPlayer(String videoFile) {
        this.videoFile = videoFile;
        this.plugins = new ArrayList<>();
    }
    
    @Override
    public void play() {
        System.out.println("Playing video: " + videoFile);
    }
    
    @Override
    public void pause() {
        System.out.println("Pausing video: " + videoFile);
    }
    
    @Override
    public void stop() {
        System.out.println("Stopping video: " + videoFile);
    }
    
    @Override
    public void addPlugin(Plugin plugin) {
        plugins.add(plugin);
        System.out.println("Added " + plugin.getName() + " plugin to video player");
    }
    
    @Override
    public void activatePlugins() {
        System.out.println("Activating plugins for video player:");
        for (Plugin plugin : plugins) {
            plugin.activate("video", videoFile);
        }
    }
}

// File: com/mediaplayer/plugins/EqualizerPlugin.java
package com.mediaplayer.plugins;

import com.mediaplayer.core.Plugin;

public class EqualizerPlugin implements Plugin {
    @Override
    public void activate(String mediaType, String mediaName) {
        System.out.println("Equalizer plugin activated for " + mediaType + ": " + mediaName);
        System.out.println("Adjusting bass and treble levels");
    }
    
    @Override
    public String getName() {
        return "Equalizer";
    }
}

// File: com/mediaplayer/plugins/VisualizerPlugin.java
package com.mediaplayer.plugins;

import com.mediaplayer.core.Plugin;

public class VisualizerPlugin implements Plugin {
    @Override
    public void activate(String mediaType, String mediaName) {
        if (mediaType.equals("audio")) {
            System.out.println("Visualizer plugin activated for " + mediaType + ": " + mediaName);
            System.out.println("Displaying audio waveforms and patterns");
        } else {
            System.out.println("Visualizer plugin is only compatible with audio files");
        }
    }
    
    @Override
    public String getName() {
        return "Visualizer";
    }
}

// File: com/mediaplayer/plugins/SubtitlePlugin.java
package com.mediaplayer.plugins;

import com.mediaplayer.core.Plugin;

public class SubtitlePlugin implements Plugin {
    @Override
    public void activate(String mediaType, String mediaName) {
        if (mediaType.equals("video")) {
            System.out.println("Subtitle plugin activated for " + mediaType + ": " + mediaName);
            System.out.println("Loading subtitles for " + mediaName);
        } else {
            System.out.println("Subtitle plugin is only compatible with video files");
        }
    }
    
    @Override
    public String getName() {
        return "Subtitle";
    }
}

// File: com/mediaplayer/Main.java
package com.mediaplayer;

import com.mediaplayer.core.MediaPlayer;
import com.mediaplayer.core.Plugin;
import com.mediaplayer.players.AudioPlayer;
import com.mediaplayer.players.VideoPlayer;
import com.mediaplayer.plugins.EqualizerPlugin;
import com.mediaplayer.plugins.VisualizerPlugin;
import com.mediaplayer.plugins.SubtitlePlugin;

public class Main {
    public static void main(String[] args) {
        // Create media players
        MediaPlayer audioPlayer = new AudioPlayer("Song.mp3");
        MediaPlayer videoPlayer = new VideoPlayer("Movie.mp4");
        
        // Create plugins
        Plugin equalizer = new EqualizerPlugin();
        Plugin visualizer = new VisualizerPlugin();
        Plugin subtitle = new SubtitlePlugin();
        
        // Add plugins to players
        audioPlayer.addPlugin(equalizer);
        audioPlayer.addPlugin(visualizer);
        
        videoPlayer.addPlugin(equalizer);
        videoPlayer.addPlugin(subtitle);
        
        // Test audio player
        System.out.println("Testing Audio Player:");
        audioPlayer.play();
        audioPlayer.activatePlugins();
        audioPlayer.pause();
        audioPlayer.stop();
        
        // Test video player
        System.out.println("\nTesting Video Player:");
        videoPlayer.play();
        videoPlayer.activatePlugins();
        videoPlayer.pause();
        videoPlayer.stop();
    }
}
```
</details>

---

## 📚 Additional Resources

- [Oracle Java Documentation - Interfaces](https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html)
- [Oracle Java Documentation - Packages](https://docs.oracle.com/javase/tutorial/java/package/index.html)
- [Oracle Java Documentation - Lambda Expressions](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)
- [Baeldung - Java Interfaces](https://www.baeldung.com/java-interfaces)
- [Baeldung - Java Packages](https://www.baeldung.com/java-packages)
- [GeeksforGeeks - Interfaces in Java](https://www.geeksforgeeks.org/interfaces-in-java/)
- [JavaTpoint - Java Package](https://www.javatpoint.com/package)

## 🔍 Key Takeaways

- Interfaces define a contract that classes must fulfill, enabling polymorphism and multiple type inheritance
- Default and static methods in interfaces (Java 8+) provide implementation within interfaces
- Functional interfaces with a single abstract method are the foundation for lambda expressions
- Lambda expressions provide a concise way to implement functional interfaces
- Packages organize related classes and interfaces, providing namespace management and access control
- Import statements make classes from other packages accessible in your code
- Access modifiers control the visibility of classes, methods, and fields at different levels
- Custom packages help organize your code in a logical and maintainable structure
        audioPlayer