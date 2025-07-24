# Day 4: Abstraction & Polymorphism - Practice Problems

## 🎯 Skills You'll Practice
- Implementing abstraction using abstract classes
- Implementing abstraction using interfaces
- Applying compile-time polymorphism (method overloading)
- Applying runtime polymorphism (method overriding)
- Designing flexible and extensible systems

## 🧠 Learning Journey
Today's problems will help you apply the abstraction and polymorphism concepts you learned in the lecture. You'll start with basic problems implementing abstract classes and interfaces, then progress to more complex scenarios that combine multiple concepts.

---

## Problem 1: Shape Hierarchy with Abstract Class

**Difficulty**: ⭐ Beginner  
**Estimated Time**: 15 minutes  
**Focus**: Abstract classes, method overriding

### 📝 Problem Statement
Create a shape hierarchy using an abstract class:
1. Create an abstract class `Shape` with:
   - An abstract method `calculateArea()`
   - An abstract method `calculatePerimeter()`
   - A concrete method `displayInfo()` that prints the shape's name, area, and perimeter

2. Create concrete subclasses:
   - `Circle` with a radius property
   - `Rectangle` with width and height properties
   - `Triangle` with three side properties

3. Implement the abstract methods in each subclass with the appropriate formulas.

4. Create a test class to demonstrate the functionality.

### 🔍 Example
```
Circle Info:
Shape: Circle
Area: 78.54 square units
Perimeter: 31.42 units

Rectangle Info:
Shape: Rectangle
Area: 24.00 square units
Perimeter: 20.00 units

Triangle Info:
Shape: Triangle
Area: 6.00 square units
Perimeter: 12.00 units
```

### 💡 Hints
<details>
<summary>Click for hints</summary>

1. Use `Math.PI` for circle calculations
2. For the triangle area, you can use Heron's formula: `Area = √(s(s-a)(s-b)(s-c))` where `s = (a+b+c)/2`
3. Override the `displayInfo()` method in each subclass to include the shape's name
4. Format decimal outputs to two decimal places using `String.format("%.2f", value)`
</details>

### ✅ Solution
```java
// Abstract class
public abstract class Shape {
    // Abstract methods
    public abstract double calculateArea();
    public abstract double calculatePerimeter();
    
    // Concrete method
    public void displayInfo() {
        System.out.println("Shape: " + getClass().getSimpleName());
        System.out.printf("Area: %.2f square units\n", calculateArea());
        System.out.printf("Perimeter: %.2f units\n", calculatePerimeter());
    }
}

// Circle class
public class Circle extends Shape {
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

// Rectangle class
public class Rectangle extends Shape {
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

// Triangle class
public class Triangle extends Shape {
    private double side1;
    private double side2;
    private double side3;
    
    public Triangle(double side1, double side2, double side3) {
        this.side1 = side1;
        this.side2 = side2;
        this.side3 = side3;
    }
    
    @Override
    public double calculateArea() {
        // Using Heron's formula
        double s = (side1 + side2 + side3) / 2;
        return Math.sqrt(s * (s - side1) * (s - side2) * (s - side3));
    }
    
    @Override
    public double calculatePerimeter() {
        return side1 + side2 + side3;
    }
}

// Test class
public class ShapeTest {
    public static void main(String[] args) {
        // Create shapes
        Shape circle = new Circle(5);
        Shape rectangle = new Rectangle(4, 6);
        Shape triangle = new Triangle(3, 4, 5);
        
        // Display information
        System.out.println("Circle Info:");
        circle.displayInfo();
        System.out.println();
        
        System.out.println("Rectangle Info:");
        rectangle.displayInfo();
        System.out.println();
        
        System.out.println("Triangle Info:");
        triangle.displayInfo();
    }
}
```

### 🔄 Code Walkthrough
1. We create an abstract `Shape` class with abstract methods for area and perimeter calculations
2. We provide a concrete `displayInfo()` method that uses the abstract methods
3. We implement three concrete shape classes, each with its specific calculation formulas
4. In the test class, we create instances of each shape and demonstrate polymorphism by using the `Shape` reference type
5. Each shape correctly calculates its area and perimeter based on its specific implementation

### 🚀 Extension Challenge
Add a `Square` class that extends `Rectangle`. Ensure that a square always has equal width and height by overriding the appropriate methods.

---

## Problem 2: Media Player with Interfaces

**Difficulty**: ⭐⭐ Intermediate  
**Estimated Time**: 20 minutes  
**Focus**: Interfaces, multiple inheritance

### 📝 Problem Statement
Create a media player system using interfaces:

1. Create an interface `MediaPlayer` with methods:
   - `play(String filename)`
   - `pause()`
   - `stop()`

2. Create an interface `AdvancedMediaPlayer` that extends `MediaPlayer` and adds methods:
   - `playInLoop(String filename)`
   - `setVolume(int volume)`
   - `getVolume()`

3. Create two concrete classes:
   - `AudioPlayer` that implements `MediaPlayer`
   - `VideoPlayer` that implements `AdvancedMediaPlayer`

4. Create a `MediaPlayerDemo` class to demonstrate the functionality.

### 🔍 Example
```
Playing audio file: song.mp3
Pausing audio playback
Stopping audio playback

Playing video file: movie.mp4
Setting volume to 80
Current volume: 80
Playing video in loop: movie.mp4
Pausing video playback
Stopping video playback
```

### 💡 Hints
<details>
<summary>Click for hints</summary>

1. Use interface inheritance to establish a hierarchy between `MediaPlayer` and `AdvancedMediaPlayer`
2. Remember that a class implementing an interface must implement all methods from that interface and its parent interfaces
3. For the `VideoPlayer`, maintain a volume field to track the current volume level
4. In the demo class, show how you can use both specific and polymorphic references
</details>

### ✅ Solution
```java
// Basic media player interface
public interface MediaPlayer {
    void play(String filename);
    void pause();
    void stop();
}

// Advanced media player interface that extends the basic one
public interface AdvancedMediaPlayer extends MediaPlayer {
    void playInLoop(String filename);
    void setVolume(int volume);
    int getVolume();
}

// Audio player implementation
public class AudioPlayer implements MediaPlayer {
    private String currentFile;
    private boolean isPlaying;
    
    @Override
    public void play(String filename) {
        currentFile = filename;
        isPlaying = true;
        System.out.println("Playing audio file: " + filename);
    }
    
    @Override
    public void pause() {
        if (isPlaying) {
            isPlaying = false;
            System.out.println("Pausing audio playback");
        }
    }
    
    @Override
    public void stop() {
        if (currentFile != null) {
            isPlaying = false;
            System.out.println("Stopping audio playback");
            currentFile = null;
        }
    }
}

// Video player implementation
public class VideoPlayer implements AdvancedMediaPlayer {
    private String currentFile;
    private boolean isPlaying;
    private boolean isLooping;
    private int volume;
    
    public VideoPlayer() {
        this.volume = 50; // Default volume
    }
    
    @Override
    public void play(String filename) {
        currentFile = filename;
        isPlaying = true;
        isLooping = false;
        System.out.println("Playing video file: " + filename);
    }
    
    @Override
    public void playInLoop(String filename) {
        currentFile = filename;
        isPlaying = true;
        isLooping = true;
        System.out.println("Playing video in loop: " + filename);
    }
    
    @Override
    public void pause() {
        if (isPlaying) {
            isPlaying = false;
            System.out.println("Pausing video playback");
        }
    }
    
    @Override
    public void stop() {
        if (currentFile != null) {
            isPlaying = false;
            isLooping = false;
            System.out.println("Stopping video playback");
            currentFile = null;
        }
    }
    
    @Override
    public void setVolume(int volume) {
        this.volume = Math.max(0, Math.min(100, volume)); // Ensure volume is between 0 and 100
        System.out.println("Setting volume to " + this.volume);
    }
    
    @Override
    public int getVolume() {
        System.out.println("Current volume: " + volume);
        return volume;
    }
}

// Demo class
public class MediaPlayerDemo {
    public static void main(String[] args) {
        // Using AudioPlayer
        MediaPlayer audioPlayer = new AudioPlayer();
        audioPlayer.play("song.mp3");
        audioPlayer.pause();
        audioPlayer.stop();
        
        System.out.println();
        
        // Using VideoPlayer with full functionality
        VideoPlayer videoPlayer = new VideoPlayer();
        videoPlayer.play("movie.mp4");
        videoPlayer.setVolume(80);
        videoPlayer.getVolume();
        videoPlayer.playInLoop("movie.mp4");
        videoPlayer.pause();
        videoPlayer.stop();
        
        System.out.println();
        
        // Using VideoPlayer through MediaPlayer interface (limited functionality)
        MediaPlayer mediaPlayer = new VideoPlayer();
        mediaPlayer.play("clip.mp4");
        mediaPlayer.pause();
        mediaPlayer.stop();
        
        // This won't work because mediaPlayer is of type MediaPlayer
        // mediaPlayer.setVolume(70);
        
        // But we can cast if we know it's actually a VideoPlayer
        if (mediaPlayer instanceof AdvancedMediaPlayer) {
            AdvancedMediaPlayer advancedPlayer = (AdvancedMediaPlayer) mediaPlayer;
            advancedPlayer.setVolume(70);
        }
    }
}
```

### 🔄 Code Walkthrough
1. We define a basic `MediaPlayer` interface with essential playback methods
2. We create an `AdvancedMediaPlayer` interface that extends the basic one and adds more functionality
3. We implement `AudioPlayer` with basic functionality
4. We implement `VideoPlayer` with advanced functionality, including volume control and loop playback
5. In the demo, we show:
   - Using each player with its specific type
   - Using a `VideoPlayer` through a `MediaPlayer` reference (demonstrating interface polymorphism)
   - Safely casting between interface types when needed

### 🚀 Extension Challenge
Add a new interface `RecordablePlayer` with methods for recording media. Create a `DVRPlayer` class that implements both `AdvancedMediaPlayer` and `RecordablePlayer` interfaces.

---

## Problem 3: Method Overloading Calculator

**Difficulty**: ⭐ Beginner  
**Estimated Time**: 15 minutes  
**Focus**: Compile-time polymorphism (method overloading)

### 📝 Problem Statement
Create a calculator class that demonstrates method overloading:

1. Create a `Calculator` class with multiple overloaded methods:
   - `add(int a, int b)` - returns the sum of two integers
   - `add(int a, int b, int c)` - returns the sum of three integers
   - `add(double a, double b)` - returns the sum of two doubles
   - `add(String a, String b)` - parses the strings as doubles and returns their sum

2. Similarly, implement overloaded methods for `subtract`, `multiply`, and `divide` operations.

3. Create a `CalculatorDemo` class to test all the overloaded methods.

### 🔍 Example
```
Integer addition: 5 + 3 = 8
Triple integer addition: 5 + 3 + 2 = 10
Double addition: 5.5 + 3.5 = 9.0
String addition: "5.5" + "3.5" = 9.0

Integer subtraction: 5 - 3 = 2
Double subtraction: 5.5 - 3.5 = 2.0

Integer multiplication: 5 * 3 = 15
Double multiplication: 5.5 * 3.5 = 19.25

Integer division: 10 / 2 = 5
Double division: 10.0 / 3.0 = 3.33
```

### 💡 Hints
<details>
<summary>Click for hints</summary>

1. Method overloading requires different parameter types or different number of parameters
2. For the string addition, use `Double.parseDouble()` to convert strings to doubles
3. Handle potential exceptions in the string methods
4. For division, consider handling division by zero
</details>

### ✅ Solution
```java
public class Calculator {
    // Addition methods
    public int add(int a, int b) {
        return a + b;
    }
    
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    
    public double add(double a, double b) {
        return a + b;
    }
    
    public double add(String a, String b) {
        try {
            double num1 = Double.parseDouble(a);
            double num2 = Double.parseDouble(b);
            return add(num1, num2);
        } catch (NumberFormatException e) {
            System.out.println("Error: Invalid number format");
            return 0;
        }
    }
    
    // Subtraction methods
    public int subtract(int a, int b) {
        return a - b;
    }
    
    public double subtract(double a, double b) {
        return a - b;
    }
    
    // Multiplication methods
    public int multiply(int a, int b) {
        return a * b;
    }
    
    public double multiply(double a, double b) {
        return a * b;
    }
    
    // Division methods
    public int divide(int a, int b) {
        if (b == 0) {
            throw new ArithmeticException("Division by zero");
        }
        return a / b;
    }
    
    public double divide(double a, double b) {
        if (b == 0) {
            throw new ArithmeticException("Division by zero");
        }
        return a / b;
    }
}

public class CalculatorDemo {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        
        // Test addition
        System.out.println("Integer addition: 5 + 3 = " + calc.add(5, 3));
        System.out.println("Triple integer addition: 5 + 3 + 2 = " + calc.add(5, 3, 2));
        System.out.println("Double addition: 5.5 + 3.5 = " + calc.add(5.5, 3.5));
        System.out.println("String addition: \"5.5\" + \"3.5\" = " + calc.add("5.5", "3.5"));
        System.out.println();
        
        // Test subtraction
        System.out.println("Integer subtraction: 5 - 3 = " + calc.subtract(5, 3));
        System.out.println("Double subtraction: 5.5 - 3.5 = " + calc.subtract(5.5, 3.5));
        System.out.println();
        
        // Test multiplication
        System.out.println("Integer multiplication: 5 * 3 = " + calc.multiply(5, 3));
        System.out.println("Double multiplication: 5.5 * 3.5 = " + calc.multiply(5.5, 3.5));
        System.out.println();
        
        // Test division
        System.out.println("Integer division: 10 / 2 = " + calc.divide(10, 2));
        System.out.println("Double division: 10.0 / 3.0 = " + String.format("%.2f", calc.divide(10.0, 3.0)));
        
        // Test division by zero
        try {
            calc.divide(5, 0);
        } catch (ArithmeticException e) {
            System.out.println("Caught exception: " + e.getMessage());
        }
    }
}
```

### 🔄 Code Walkthrough
1. We create a `Calculator` class with multiple overloaded methods for each operation
2. Each method has the same name but different parameter types or counts
3. For string operations, we parse the strings to doubles and reuse the double methods
4. We handle potential exceptions like number format errors and division by zero
5. In the demo class, we test all the overloaded methods with different types of inputs

### 🚀 Extension Challenge
Add array versions of the methods that can handle an arbitrary number of operands (e.g., `add(int[] numbers)`, `multiply(double[] numbers)`).

---

## Problem 4: Animal Kingdom Simulation

**Difficulty**: ⭐⭐⭐ Advanced  
**Estimated Time**: 30 minutes  
**Focus**: Runtime polymorphism, abstract classes, interfaces

### 📝 Problem Statement
Create an animal kingdom simulation that demonstrates runtime polymorphism:

1. Create an abstract class `Animal` with:
   - Properties for `name` and `age`
   - Abstract methods `makeSound()` and `move()`
   - Concrete method `displayInfo()`

2. Create interfaces:
   - `Carnivore` with method `hunt()`
   - `Herbivore` with method `graze()`
   - `Aquatic` with methods `swim()` and `canLiveOnLand()`

3. Create concrete animal classes:
   - `Lion` (extends `Animal`, implements `Carnivore`)
   - `Elephant` (extends `Animal`, implements `Herbivore`)
   - `Frog` (extends `Animal`, implements `Carnivore`, `Aquatic`)
   - `Dolphin` (extends `Animal`, implements `Carnivore`, `Aquatic`)

4. Create a `Zoo` class that can:
   - Add animals to the zoo
   - Display all animals
   - Feed all animals (calls appropriate method based on diet)
   - Make all animals move and make sounds

### 🔍 Example
```
Zoo Animals:
Lion (5 years old) - Carnivore
Elephant (10 years old) - Herbivore
Frog (2 years old) - Carnivore, Aquatic
Dolphin (7 years old) - Carnivore, Aquatic

Animal Sounds:
Lion roars: ROAR!
Elephant trumpets: TRUMPET!
Frog croaks: RIBBIT!
Dolphin clicks: CLICK CLICK!

Animal Movements:
Lion is running on four legs
Elephant is walking slowly
Frog is jumping
Dolphin is swimming fast

Feeding Time:
Lion is hunting prey
Elephant is grazing on grass
Frog is hunting insects
Dolphin is hunting fish

Aquatic Animals:
Frog is swimming in water. Can live on land: true
Dolphin is swimming in water. Can live on land: false
```

### 💡 Hints
<details>
<summary>Click for hints</summary>

1. Use the abstract `Animal` class to define common properties and behaviors
2. Use interfaces to represent different capabilities and diets
3. Implement runtime polymorphism by storing all animals in a common collection
4. Use `instanceof` to check which interfaces an animal implements
5. Create specialized methods in the `Zoo` class to handle different animal types
</details>

### ✅ Solution
```java
// Abstract class
public abstract class Animal {
    protected String name;
    protected int age;
    
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // Abstract methods
    public abstract void makeSound();
    public abstract void move();
    
    // Concrete method
    public void displayInfo() {
        System.out.println(name + " (" + age + " years old)");
    }
    
    public String getName() {
        return name;
    }
    
    public int getAge() {
        return age;
    }
}

// Interfaces
public interface Carnivore {
    void hunt();
}

public interface Herbivore {
    void graze();
}

public interface Aquatic {
    void swim();
    boolean canLiveOnLand();
}

// Concrete animal classes
public class Lion extends Animal implements Carnivore {
    public Lion(String name, int age) {
        super(name, age);
    }
    
    @Override
    public void makeSound() {
        System.out.println(name + " roars: ROAR!");
    }
    
    @Override
    public void move() {
        System.out.println(name + " is running on four legs");
    }
    
    @Override
    public void hunt() {
        System.out.println(name + " is hunting prey");
    }
}

public class Elephant extends Animal implements Herbivore {
    public Elephant(String name, int age) {
        super(name, age);
    }
    
    @Override
    public void makeSound() {
        System.out.println(name + " trumpets: TRUMPET!");
    }
    
    @Override
    public void move() {
        System.out.println(name + " is walking slowly");
    }
    
    @Override
    public void graze() {
        System.out.println(name + " is grazing on grass");
    }
}

public class Frog extends Animal implements Carnivore, Aquatic {
    public Frog(String name, int age) {
        super(name, age);
    }
    
    @Override
    public void makeSound() {
        System.out.println(name + " croaks: RIBBIT!");
    }
    
    @Override
    public void move() {
        System.out.println(name + " is jumping");
    }
    
    @Override
    public void hunt() {
        System.out.println(name + " is hunting insects");
    }
    
    @Override
    public void swim() {
        System.out.println(name + " is swimming in water");
    }
    
    @Override
    public boolean canLiveOnLand() {
        return true;
    }
}

public class Dolphin extends Animal implements Carnivore, Aquatic {
    public Dolphin(String name, int age) {
        super(name, age);
    }
    
    @Override
    public void makeSound() {
        System.out.println(name + " clicks: CLICK CLICK!");
    }
    
    @Override
    public void move() {
        System.out.println(name + " is swimming fast");
    }
    
    @Override
    public void hunt() {
        System.out.println(name + " is hunting fish");
    }
    
    @Override
    public void swim() {
        System.out.println(name + " is swimming in water");
    }
    
    @Override
    public boolean canLiveOnLand() {
        return false;
    }
}

// Zoo class
public class Zoo {
    private List<Animal> animals;
    
    public Zoo() {
        animals = new ArrayList<>();
    }
    
    public void addAnimal(Animal animal) {
        animals.add(animal);
    }
    
    public void displayAllAnimals() {
        System.out.println("Zoo Animals:");
        for (Animal animal : animals) {
            animal.displayInfo();
            
            // Determine animal type(s)
            String types = "";
            if (animal instanceof Carnivore) {
                types += "Carnivore";
            }
            if (animal instanceof Herbivore) {
                if (!types.isEmpty()) types += ", ";
                types += "Herbivore";
            }
            if (animal instanceof Aquatic) {
                if (!types.isEmpty()) types += ", ";
                types += "Aquatic";
            }
            
            System.out.println(" - " + types);
        }
        System.out.println();
    }
    
    public void makeAllAnimalsSounds() {
        System.out.println("Animal Sounds:");
        for (Animal animal : animals) {
            animal.makeSound();
        }
        System.out.println();
    }
    
    public void makeAllAnimalsMove() {
        System.out.println("Animal Movements:");
        for (Animal animal : animals) {
            animal.move();
        }
        System.out.println();
    }
    
    public void feedAllAnimals() {
        System.out.println("Feeding Time:");
        for (Animal animal : animals) {
            if (animal instanceof Carnivore) {
                ((Carnivore) animal).hunt();
            }
            if (animal instanceof Herbivore) {
                ((Herbivore) animal).graze();
            }
        }
        System.out.println();
    }
    
    public void showAquaticAnimals() {
        System.out.println("Aquatic Animals:");
        for (Animal animal : animals) {
            if (animal instanceof Aquatic) {
                Aquatic aquaticAnimal = (Aquatic) animal;
                aquaticAnimal.swim();
                System.out.println("Can live on land: " + aquaticAnimal.canLiveOnLand());
            }
        }
        System.out.println();
    }
}

// Demo class
public class ZooDemo {
    public static void main(String[] args) {
        Zoo zoo = new Zoo();
        
        // Add animals to the zoo
        zoo.addAnimal(new Lion("Simba", 5));
        zoo.addAnimal(new Elephant("Dumbo", 10));
        zoo.addAnimal(new Frog("Kermit", 2));
        zoo.addAnimal(new Dolphin("Flipper", 7));
        
        // Display all animals
        zoo.displayAllAnimals();
        
        // Make all animals produce sounds
        zoo.makeAllAnimalsSounds();
        
        // Make all animals move
        zoo.makeAllAnimalsMove();
        
        // Feed all animals
        zoo.feedAllAnimals();
        
        // Show aquatic animals
        zoo.showAquaticAnimals();
    }
}
```

### 🔄 Code Walkthrough
1. We create an abstract `Animal` class with common properties and behaviors
2. We define interfaces for different animal capabilities: `Carnivore`, `Herbivore`, and `Aquatic`
3. We implement concrete animal classes that extend `Animal` and implement appropriate interfaces
4. We create a `Zoo` class that manages a collection of animals and provides methods to interact with them
5. We demonstrate runtime polymorphism by:
   - Storing different animal types in a common `Animal` collection
   - Using `instanceof` to check which interfaces an animal implements
   - Casting to appropriate types to call interface-specific methods

### 🚀 Extension Challenge
Add a `Migratory` interface with methods for seasonal migration. Implement it in appropriate animal classes and add functionality to the `Zoo` class to track which animals migrate during different seasons.

---

## Problem 5: E-Commerce System

**Difficulty**: ⭐⭐⭐ Advanced  
**Estimated Time**: 35 minutes  
**Focus**: Abstraction, polymorphism, interfaces, real-world application

### 📝 Problem Statement
Create a simplified e-commerce system that demonstrates abstraction and polymorphism:

1. Create an abstract class `Product` with:
   - Properties for `id`, `name`, `price`, and `stockQuantity`
   - Abstract method `calculateDiscount()`
   - Concrete methods `getPrice()`, `applyDiscount()`, and `displayInfo()`

2. Create interfaces:
   - `Shippable` with methods `calculateShippingCost(String destination)` and `getEstimatedDeliveryDays(String destination)`
   - `Taxable` with method `calculateTax()`
   - `Reviewable` with methods `addReview(String review, int rating)` and `getAverageRating()`

3. Create concrete product classes:
   - `Electronics` (extends `Product`, implements `Shippable`, `Taxable`, `Reviewable`)
   - `Clothing` (extends `Product`, implements `Shippable`, `Taxable`, `Reviewable`)
   - `Book` (extends `Product`, implements `Shippable`, `Reviewable`)
   - `DigitalProduct` (extends `Product`, implements `Taxable`, `Reviewable`)

4. Create a `ShoppingCart` class that can:
   - Add products to the cart
   - Remove products from the cart
   - Calculate total price (including discounts)
   - Calculate total shipping cost
   - Calculate total tax
   - Display cart contents

### 🔍 Example
```
Shopping Cart Contents:
1. Laptop (Electronics) - $999.99 (10% discount: $899.99)
   Shipping to New York: $15.00, Estimated delivery: 3 days
   Tax: $72.00
   Rating: 4.5/5 (2 reviews)

2. T-Shirt (Clothing) - $29.99 (5% discount: $28.49)
   Shipping to New York: $5.00, Estimated delivery: 2 days
   Tax: $2.28
   Rating: 4.0/5 (3 reviews)

3. Java Programming Book (Book) - $49.99 (0% discount: $49.99)
   Shipping to New York: $7.50, Estimated delivery: 4 days
   Rating: 4.8/5 (5 reviews)

4. Music Album (Digital Product) - $9.99 (0% discount: $9.99)
   Tax: $0.80
   Rating: 4.2/5 (10 reviews)

Cart Summary:
Subtotal: $1089.96
Total Discount: $101.50
Total After Discounts: $988.46
Total Shipping: $27.50
Total Tax: $75.08
Grand Total: $1091.04
```

### 💡 Hints
<details>
<summary>Click for hints</summary>

1. Use the abstract `Product` class to define common properties and behaviors
2. Use interfaces to represent different product capabilities
3. Implement different discount strategies for each product type
4. For the `ShoppingCart`, use a collection to store products
5. Use polymorphism to calculate totals based on product types and capabilities
</details>

### ✅ Solution
```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

// Abstract Product class
public abstract class Product {
    protected String id;
    protected String name;
    protected double price;
    protected int stockQuantity;
    
    public Product(String id, String name, double price, int stockQuantity) {
        this.id = id;
        this.name = name;
        this.price = price;
        this.stockQuantity = stockQuantity;
    }
    
    // Abstract method
    public abstract double calculateDiscount();
    
    // Concrete methods
    public double getPrice() {
        return price;
    }
    
    public double applyDiscount() {
        return price - calculateDiscount();
    }
    
    public void displayInfo() {
        System.out.println(name + " - $" + String.format("%.2f", price) +
                          " (" + (calculateDiscount() > 0 ?
                                String.format("%.0f", (calculateDiscount() / price * 100)) + "%% discount: $" +
                                String.format("%.2f", applyDiscount()) :
                                "0% discount: $" + String.format("%.2f", applyDiscount())) + ")");
    }
    
    public String getId() {
        return id;
    }
    
    public String getName() {
        return name;
    }
    
    public int getStockQuantity() {
        return stockQuantity;
    }
    
    public void setStockQuantity(int stockQuantity) {
        this.stockQuantity = stockQuantity;
    }
}

// Interfaces
public interface Shippable {
    double calculateShippingCost(String destination);
    int getEstimatedDeliveryDays(String destination);
}

public interface Taxable {
    double calculateTax();
}

public interface Reviewable {
    void addReview(String review, int rating);
    double getAverageRating();
    int getReviewCount();
}

// Electronics class
public class Electronics extends Product implements Shippable, Taxable, Reviewable {
    private String brand;
    private String model;
    private int warrantyPeriod; // in months
    private List<String> reviews;
    private List<Integer> ratings;
    
    public Electronics(String id, String name, double price, int stockQuantity,
                      String brand, String model, int warrantyPeriod) {
        super(id, name, price, stockQuantity);
        this.brand = brand;
        this.model = model;
        this.warrantyPeriod = warrantyPeriod;
        this.reviews = new ArrayList<>();
        this.ratings = new ArrayList<>();
    }
    
    @Override
    public double calculateDiscount() {
        // 10% discount for electronics
        return price * 0.10;
    }
    
    @Override
    public double calculateShippingCost(String destination) {
        // Base shipping cost is $15
        return 15.0;
    }
    
    @Override
    public int getEstimatedDeliveryDays(String destination) {
        // Standard delivery time is 3 days
        return 3;
    }
    
    @Override
    public double calculateTax() {
        // 8% tax for electronics
        return applyDiscount() * 0.08;
    }
    
    @Override
    public void addReview(String review, int rating) {
        reviews.add(review);
        ratings.add(rating);
    }
    
    @Override
    public double getAverageRating() {
        if (ratings.isEmpty()) {
            return 0;
        }
        
        int sum = 0;
        for (int rating : ratings) {
            sum += rating;
        }
        
        return (double) sum / ratings.size();
    }
    
    @Override
    public int getReviewCount() {
        return reviews.size();
    }
}

// Clothing class
public class Clothing extends Product implements Shippable, Taxable, Reviewable {
    private String size;
    private String color;
    private String material;
    private List<String> reviews;
    private List<Integer> ratings;
    
    public Clothing(String id, String name, double price, int stockQuantity,
                   String size, String color, String material) {
        super(id, name, price, stockQuantity);
        this.size = size;
        this.color = color;
        this.material = material;
        this.reviews = new ArrayList<>();
        this.ratings = new ArrayList<>();
    }
    
    @Override
    public double calculateDiscount() {
        // 5% discount for clothing
        return price * 0.05;
    }
    
    @Override
    public double calculateShippingCost(String destination) {
        // Base shipping cost is $5
        return 5.0;
    }
    
    @Override
    public int getEstimatedDeliveryDays(String destination) {
        // Standard delivery time is 2 days
        return 2;
    }
    
    @Override
    public double calculateTax() {
        // 8% tax for clothing
        return applyDiscount() * 0.08;
    }
    
    @Override
    public void addReview(String review, int rating) {
        reviews.add(review);
        ratings.add(rating);
    }
    
    @Override
    public double getAverageRating() {
        if (ratings.isEmpty()) {
            return 0;
        }
        
        int sum = 0;
        for (int rating : ratings) {
            sum += rating;
        }
        
        return (double) sum / ratings.size();
    }
    
    @Override
    public int getReviewCount() {
        return reviews.size();
    }
}

// Book class
public class Book extends Product implements Shippable, Reviewable {
    private String author;
    private String isbn;
    private int pages;
    private List<String> reviews;
    private List<Integer> ratings;
    
    public Book(String id, String name, double price, int stockQuantity,
               String author, String isbn, int pages) {
        super(id, name, price, stockQuantity);
        this.author = author;
        this.isbn = isbn;
        this.pages = pages;
        this.reviews = new ArrayList<>();
        this.ratings = new ArrayList<>();
    }
    
    @Override
    public double calculateDiscount() {
        // No discount for books
        return 0;
    }
    
    @Override
    public double calculateShippingCost(String destination) {
        // Base shipping cost is $7.50
        return 7.50;
    }
    
    @Override
    public int getEstimatedDeliveryDays(String destination) {
        // Standard delivery time is 4 days
        return 4;
    }
    
    @Override
    public void addReview(String review, int rating) {
        reviews.add(review);
        ratings.add(rating);
    }
    
    @Override
    public double getAverageRating() {
        if (ratings.isEmpty()) {
            return 0;
        }
        
        int sum = 0;
        for (int rating : ratings) {
            sum += rating;
        }
        
        return (double) sum / ratings.size();
    }
    
    @Override
    public int getReviewCount() {
        return reviews.size();
    }
}

// Digital Product class
public class DigitalProduct extends Product implements Taxable, Reviewable {
    private String downloadLink;
    private double fileSizeMB;
    private List<String> reviews;
    private List<Integer> ratings;
    
    public DigitalProduct(String id, String name, double price, int stockQuantity,
                         String downloadLink, double fileSizeMB) {
        super(id, name, price, stockQuantity);
        this.downloadLink = downloadLink;
        this.fileSizeMB = fileSizeMB;
        this.reviews = new ArrayList<>();
        this.ratings = new ArrayList<>();
    }
    
    @Override
    public double calculateDiscount() {
        // No discount for digital products
        return 0;
    }
    
    @Override
    public double calculateTax() {
        // 8% tax for digital products
        return applyDiscount() * 0.08;
    }
    
    @Override
    public void addReview(String review, int rating) {
        reviews.add(review);
        ratings.add(rating);
    }
    
    @Override
    public double getAverageRating() {
        if (ratings.isEmpty()) {
            return 0;
        }
        
        int sum = 0;
        for (int rating : ratings) {
            sum += rating;
        }
        
        return (double) sum / ratings.size();
    }
    
    @Override
    public int getReviewCount() {
        return reviews.size();
    }
}

// Shopping Cart class
public class ShoppingCart {
    private List<Product> products;
    private String shippingDestination;
    
    public ShoppingCart(String shippingDestination) {
        this.products = new ArrayList<>();
        this.shippingDestination = shippingDestination;
    }
    
    public void addProduct(Product product) {
        products.add(product);
    }
    
    public void removeProduct(String productId) {
        products.removeIf(product -> product.getId().equals(productId));
    }
    
    public double calculateSubtotal() {
        double subtotal = 0;
        for (Product product : products) {
            subtotal += product.getPrice();
        }
        return subtotal;
    }
    
    public double calculateTotalDiscount() {
        double totalDiscount = 0;
        for (Product product : products) {
            totalDiscount += product.calculateDiscount();
        }
        return totalDiscount;
    }
    
    public double calculateTotalAfterDiscounts() {
        double total = 0;
        for (Product product : products) {
            total += product.applyDiscount();
        }
        return total;
    }
    
    public double calculateTotalShippingCost() {
        double totalShipping = 0;
        for (Product product : products) {
            if (product instanceof Shippable) {
                totalShipping += ((Shippable) product).calculateShippingCost(shippingDestination);
            }
        }
        return totalShipping;
    }
    
    public double calculateTotalTax() {
        double totalTax = 0;
        for (Product product : products) {
            if (product instanceof Taxable) {
                totalTax += ((Taxable) product).calculateTax();
            }
        }
        return totalTax;
    }
    
    public double calculateGrandTotal() {
        return calculateTotalAfterDiscounts() + calculateTotalShippingCost() + calculateTotalTax();
    }
    
    public void displayCartContents() {
        System.out.println("Shopping Cart Contents:");
        
        for (int i = 0; i < products.size(); i++) {
            Product product = products.get(i);
            
            System.out.print((i + 1) + ". ");
            product.displayInfo();
            
            // Display shipping info if applicable
            if (product instanceof Shippable) {
                Shippable shippable = (Shippable) product;
                System.out.printf("   Shipping to %s: $%.2f, Estimated delivery: %d days\n",
                                 shippingDestination,
                                 shippable.calculateShippingCost(shippingDestination),
                                 shippable.getEstimatedDeliveryDays(shippingDestination));
            }
            
            // Display tax info if applicable
            if (product instanceof Taxable) {
                Taxable taxable = (Taxable) product;
                System.out.printf("   Tax: $%.2f\n", taxable.calculateTax());
            }
            
            // Display review info if applicable
            if (product instanceof Reviewable) {
                Reviewable reviewable = (Reviewable) product;
                System.out.printf("   Rating: %.1f/5 (%d reviews)\n",
                                 reviewable.getAverageRating(),
                                 reviewable.getReviewCount());
            }
            
            System.out.println();
        }
        
        // Display cart summary
        System.out.println("Cart Summary:");
        System.out.printf("Subtotal: $%.2f\n", calculateSubtotal());
        System.out.printf("Total Discount: $%.2f\n", calculateTotalDiscount());
        System.out.printf("Total After Discounts: $%.2f\n", calculateTotalAfterDiscounts());
        System.out.printf("Total Shipping: $%.2f\n", calculateTotalShippingCost());
        System.out.printf("Total Tax: $%.2f\n", calculateTotalTax());
        System.out.printf("Grand Total: $%.2f\n", calculateGrandTotal());
    }
}

// Demo class
public class ECommerceDemo {
    public static void main(String[] args) {
        // Create products
        Electronics laptop = new Electronics("E001", "Laptop", 999.99, 10, "Dell", "XPS 13", 24);
        Clothing tshirt = new Clothing("C001", "T-Shirt", 29.99, 50, "M", "Blue", "Cotton");
        Book book = new Book("B001", "Java Programming Book", 49.99, 30, "John Doe", "123-456-789", 500);
        DigitalProduct music = new DigitalProduct("D001", "Music Album", 9.99, 999, "download.example.com/album", 150.5);
        
        // Add reviews
        laptop.addReview("Great laptop!", 5);
        laptop.addReview("Good performance but battery life could be better.", 4);
        
        tshirt.addReview("Nice fit and comfortable.", 4);
        tshirt.addReview("Good quality.", 4);
        tshirt.addReview("Color faded after washing.", 3);
        
        book.addReview("Excellent for beginners.", 5);
        book.addReview("Well explained concepts.", 5);
        book.addReview("Good examples.", 5);
        book.addReview("Some typos but overall good.", 4);
        book.addReview("Very helpful.", 5);
        
        music.addReview("Great album!", 5);
        music.addReview("Love track 3.", 4);
        music.addReview("Not their best work.", 3);
        music.addReview("Amazing!", 5);
        music.addReview("Good but not great.", 4);
        music.addReview("Decent.", 4);
        music.addReview("Love it!", 5);
        music.addReview("Not bad.", 4);
        music.addReview("Pretty good.", 4);
        music.addReview("Worth the money.", 4);
        
        // Create shopping cart
        ShoppingCart cart = new ShoppingCart("New York");
        
        // Add products to cart
        cart.addProduct(laptop);
        cart.addProduct(tshirt);
        cart.addProduct(book);
        cart.addProduct(music);
        
        // Display cart contents
        cart.displayCartContents();
    }
}
```

### 🔄 Code Walkthrough
1. We create an abstract `Product` class with common properties and an abstract method for discount calculation
2. We define interfaces for different product capabilities: `Shippable`, `Taxable`, and `Reviewable`
3. We implement concrete product classes that extend `Product` and implement appropriate interfaces
4. Each product type has its own discount strategy and specific properties
5. We create a `ShoppingCart` class that manages products and calculates various totals
6. We demonstrate polymorphism by:
   - Storing different product types in a common collection
   - Using `instanceof` to check which interfaces a product implements
   - Calling interface-specific methods based on product capabilities

### 🚀 Extension Challenge
Add a `Subscription` class that extends `Product` and represents a recurring payment product. Implement a method to calculate the annual cost and add functionality to the `ShoppingCart` to handle subscription products.

---

## 📚 Key Concepts Practiced

1. **Abstraction**
   - Using abstract classes to define common structure and behavior
   - Using interfaces to define capabilities
   - Hiding implementation details behind well-defined interfaces

2. **Polymorphism**
   - Compile-time polymorphism through method overloading
   - Runtime polymorphism through method overriding
   - Interface polymorphism by implementing multiple interfaces

3. **Object-Oriented Design**
   - Inheritance hierarchies
   - Interface segregation
   - "Is-a" vs "Can-do" relationships

4. **Java Language Features**
   - Abstract classes and methods
   - Interfaces and default methods
   - Type checking with `instanceof`
   - Type casting

5. **Design Principles**
   - Single Responsibility Principle
   - Open/Closed Principle
   - Interface Segregation Principle
   - Programming to interfaces, not implementations

## 🔍 Further Exploration
- Research the Liskov Substitution Principle and how it relates to inheritance
- Explore the Decorator pattern as an alternative to inheritance for extending functionality
- Learn about the Adapter pattern for making incompatible interfaces work together
- Study the Composite pattern for building tree structures of objects