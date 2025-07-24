# Day 4: Abstraction & Polymorphism

## 🗺️ Learning Roadmap

```
Day 1 (Basics) → Day 2 (Control Flow) → Day 3 (Keywords & Inheritance) → Day 4 (Abstraction & Polymorphism) → Day 5 (Methods & Abstract Classes)
                                                                             ↓
                                                                        You are here
```

## 🎯 Learning Objectives

By the end of this session, you will be able to:
- Understand the concept of abstraction and its importance in OOP
- Implement abstraction using abstract classes and interfaces
- Apply abstraction to hide implementation details
- Understand polymorphism and its types
- Implement runtime and compile-time polymorphism
- Design flexible and extensible systems using polymorphism

## ⏱️ Time Allocation
- Abstraction (45 minutes)
- Polymorphism (45 minutes)

---

## Session 1: Abstraction (45 minutes)

### 1. Abstraction Concepts

#### What is Abstraction?

Abstraction is one of the four fundamental OOP concepts. It is the process of hiding the implementation details and showing only the functionality to the user. It focuses on what an object does rather than how it does it.

#### 🔍 Key Characteristics

- Hides complex implementation details
- Exposes only necessary features
- Reduces complexity and increases efficiency
- Helps in reducing programming complexity and effort

#### 📊 Visual Representation

```
┌───────────────────────────────────────────────────┐
│                                                   │
│                   ABSTRACTION                     │
│                                                   │
│  ┌─────────────────┐        ┌─────────────────┐  │
│  │                 │        │                 │  │
│  │  VISIBLE        │        │  HIDDEN         │  │
│  │  - What it does │        │  - How it works │  │
│  │  - Interface    │        │  - Implementation│  │
│  │  - Methods      │        │  - Complexity   │  │
│  │                 │        │                 │  │
│  └─────────────────┘        └─────────────────┘  │
│                                                   │
└───────────────────────────────────────────────────┘
```

#### Real-World Analogy

Think of a car. As a driver, you interact with the steering wheel, pedals, and dashboard (the interface). You don't need to know how the engine, transmission, or electrical systems work (the implementation). This is abstraction - you know what the car does, but not how it does it.

#### 🤔 Think About It
Why is abstraction important in software development?

<details>
<summary>Answer</summary>
Abstraction is important because it:
<ul>
<li>Reduces complexity by hiding unnecessary details</li>
<li>Makes code more maintainable and easier to understand</li>
<li>Allows developers to focus on what objects do rather than how they work</li>
<li>Enables code reuse and modular design</li>
<li>Facilitates changes to implementation without affecting the interface</li>
</ul>
</details>

---

### 2. Types of Abstraction in Java

Java provides two main ways to achieve abstraction:

#### 1. Abstract Classes

An abstract class is a class that cannot be instantiated and may contain abstract methods (methods without a body). It serves as a blueprint for subclasses.

```java
// Abstract class
public abstract class Shape {
    // Regular method with implementation
    public void display() {
        System.out.println("This is a shape");
    }
    
    // Abstract method (no implementation)
    public abstract double calculateArea();
}

// Concrete subclass
public class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    // Implementation of abstract method
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}
```

**Key Points about Abstract Classes:**
- Cannot be instantiated (cannot create objects)
- May contain abstract methods (methods without a body)
- May contain concrete methods (methods with a body)
- May contain instance variables, constructors, and static methods
- Subclasses must implement all abstract methods or be declared abstract themselves
- A class can extend only one abstract class (single inheritance)

#### 2. Interfaces

An interface is a completely abstract type that contains only abstract method declarations and constants. It defines a contract that implementing classes must follow.

```java
// Interface
public interface Drawable {
    // Abstract methods (implicitly public and abstract)
    void draw();
    void resize(int percentage);
    
    // Constant (implicitly public, static, and final)
    int MAX_SIZE = 100;
}

// Class implementing the interface
public class Rectangle implements Drawable {
    private int width;
    private int height;
    
    public Rectangle(int width, int height) {
        this.width = width;
        this.height = height;
    }
    
    // Implementation of interface methods
    @Override
    public void draw() {
        System.out.println("Drawing a rectangle");
    }
    
    @Override
    public void resize(int percentage) {
        width = width * percentage / 100;
        height = height * percentage / 100;
    }
}
```

**Key Points about Interfaces:**
- Cannot be instantiated
- All methods are implicitly public and abstract (prior to Java 8)
- All variables are implicitly public, static, and final (constants)
- A class can implement multiple interfaces (achieving a form of multiple inheritance)
- Since Java 8, interfaces can have default and static methods with implementations
- Since Java 9, interfaces can have private methods

#### 📊 Comparison: Abstract Class vs Interface

| Feature | Abstract Class | Interface |
|---------|----------------|-----------|
| Instantiation | Cannot be instantiated | Cannot be instantiated |
| Methods | Can have abstract and concrete methods | All methods are abstract (prior to Java 8) |
| Variables | Can have instance variables | Only constants (public static final) |
| Constructors | Can have constructors | Cannot have constructors |
| Inheritance | Single inheritance (extends) | Multiple inheritance (implements) |
| Access Modifiers | Any access modifier | Methods are implicitly public |
| Purpose | "Is-A" relationship | "Can-Do" relationship |

#### 🤔 Think About It
When would you use an abstract class versus an interface?

<details>
<summary>Answer</summary>
<p>Use an abstract class when:</p>
<ul>
<li>You want to share code among several closely related classes</li>
<li>You expect classes that extend your abstract class to have many common methods or fields</li>
<li>You want to declare non-static or non-final fields</li>
<li>You need to provide a common implementation for some methods</li>
</ul>
<p>Use an interface when:</p>
<ul>
<li>You expect unrelated classes to implement your interface</li>
<li>You want to specify the behavior of a particular data type, but not concerned about who implements its behavior</li>
<li>You want to take advantage of multiple inheritance</li>
<li>You want to define a contract for a set of functionality</li>
</ul>
</details>

---

### 3. Abstraction in Action: Real-time Project Example

Let's implement a music player application to demonstrate abstraction:

```java
// Abstract class representing a media player
public abstract class MediaPlayer {
    protected String name;
    protected boolean playing;
    
    public MediaPlayer(String name) {
        this.name = name;
        this.playing = false;
    }
    
    // Common functionality with implementation
    public void turnOn() {
        System.out.println(name + " is turning on...");
    }
    
    public void turnOff() {
        System.out.println(name + " is turning off...");
        if (playing) {
            stop();
        }
    }
    
    // Abstract methods that subclasses must implement
    public abstract void play(String mediaName);
    public abstract void stop();
    public abstract void pause();
    public abstract void next();
    public abstract void previous();
}

// Interface for devices with volume control
public interface VolumeControl {
    void increaseVolume(int level);
    void decreaseVolume(int level);
    void mute();
    
    // Default method (Java 8+)
    default void setToMaxVolume() {
        System.out.println("Setting to maximum volume");
        increaseVolume(100);
    }
}

// Interface for devices with display
public interface Display {
    void showInfo(String info);
    void clearDisplay();
}

// Concrete implementation: Music Player
public class MusicPlayer extends MediaPlayer implements VolumeControl, Display {
    private int volume;
    private String currentSong;
    private List<String> playlist;
    private int currentIndex;
    
    public MusicPlayer(String name) {
        super(name);
        this.volume = 50; // Default volume
        this.playlist = new ArrayList<>();
        this.currentIndex = 0;
    }
    
    public void addToPlaylist(String song) {
        playlist.add(song);
        System.out.println("Added to playlist: " + song);
    }
    
    // Implementation of abstract methods from MediaPlayer
    @Override
    public void play(String songName) {
        if (playlist.contains(songName)) {
            currentSong = songName;
            currentIndex = playlist.indexOf(songName);
            playing = true;
            System.out.println("Playing: " + currentSong);
            showInfo("Now playing: " + currentSong);
        } else {
            System.out.println("Song not found in playlist: " + songName);
        }
    }
    
    @Override
    public void stop() {
        if (playing) {
            playing = false;
            System.out.println("Stopped playing: " + currentSong);
            showInfo("Stopped");
        }
    }
    
    @Override
    public void pause() {
        if (playing) {
            playing = false;
            System.out.println("Paused: " + currentSong);
            showInfo("Paused: " + currentSong);
        }
    }
    
    @Override
    public void next() {
        if (playlist.isEmpty()) {
            System.out.println("Playlist is empty");
            return;
        }
        
        currentIndex = (currentIndex + 1) % playlist.size();
        currentSong = playlist.get(currentIndex);
        System.out.println("Playing next: " + currentSong);
        showInfo("Now playing: " + currentSong);
        playing = true;
    }
    
    @Override
    public void previous() {
        if (playlist.isEmpty()) {
            System.out.println("Playlist is empty");
            return;
        }
        
        currentIndex = (currentIndex - 1 + playlist.size()) % playlist.size();
        currentSong = playlist.get(currentIndex);
        System.out.println("Playing previous: " + currentSong);
        showInfo("Now playing: " + currentSong);
        playing = true;
    }
    
    // Implementation of methods from VolumeControl interface
    @Override
    public void increaseVolume(int level) {
        volume = Math.min(100, volume + level);
        System.out.println("Volume increased to: " + volume);
        showInfo("Volume: " + volume);
    }
    
    @Override
    public void decreaseVolume(int level) {
        volume = Math.max(0, volume - level);
        System.out.println("Volume decreased to: " + volume);
        showInfo("Volume: " + volume);
    }
    
    @Override
    public void mute() {
        int oldVolume = volume;
        volume = 0;
        System.out.println("Muted (previous volume: " + oldVolume + ")");
        showInfo("Muted");
    }
    
    // Implementation of methods from Display interface
    @Override
    public void showInfo(String info) {
        System.out.println("DISPLAY: " + info);
    }
    
    @Override
    public void clearDisplay() {
        System.out.println("DISPLAY: [Cleared]");
    }
}

// Another concrete implementation: Video Player
public class VideoPlayer extends MediaPlayer implements VolumeControl, Display {
    private int volume;
    private String currentVideo;
    private int brightness;
    
    public VideoPlayer(String name) {
        super(name);
        this.volume = 50;
        this.brightness = 70;
    }
    
    // Implementation of abstract methods from MediaPlayer
    @Override
    public void play(String videoName) {
        currentVideo = videoName;
        playing = true;
        System.out.println("Playing video: " + currentVideo);
        showInfo("Now playing: " + currentVideo);
    }
    
    @Override
    public void stop() {
        if (playing) {
            playing = false;
            System.out.println("Stopped playing video: " + currentVideo);
            showInfo("Stopped");
        }
    }
    
    @Override
    public void pause() {
        if (playing) {
            playing = false;
            System.out.println("Paused video: " + currentVideo);
            showInfo("Paused: " + currentVideo);
        }
    }
    
    @Override
    public void next() {
        System.out.println("Next video");
        showInfo("Next video");
    }
    
    @Override
    public void previous() {
        System.out.println("Previous video");
        showInfo("Previous video");
    }
    
    // Implementation of methods from VolumeControl interface
    @Override
    public void increaseVolume(int level) {
        volume = Math.min(100, volume + level);
        System.out.println("Video volume increased to: " + volume);
        showInfo("Volume: " + volume);
    }
    
    @Override
    public void decreaseVolume(int level) {
        volume = Math.max(0, volume - level);
        System.out.println("Video volume decreased to: " + volume);
        showInfo("Volume: " + volume);
    }
    
    @Override
    public void mute() {
        int oldVolume = volume;
        volume = 0;
        System.out.println("Video muted (previous volume: " + oldVolume + ")");
        showInfo("Muted");
    }
    
    // Implementation of methods from Display interface
    @Override
    public void showInfo(String info) {
        System.out.println("VIDEO DISPLAY: " + info);
    }
    
    @Override
    public void clearDisplay() {
        System.out.println("VIDEO DISPLAY: [Cleared]");
    }
    
    // Additional methods specific to VideoPlayer
    public void adjustBrightness(int level) {
        brightness = Math.max(0, Math.min(100, brightness + level));
        System.out.println("Brightness adjusted to: " + brightness);
        showInfo("Brightness: " + brightness);
    }
}

// Demo class
public class MediaPlayerDemo {
    public static void main(String[] args) {
        // Create a music player
        MusicPlayer musicPlayer = new MusicPlayer("MyMusicPlayer");
        musicPlayer.turnOn();
        musicPlayer.addToPlaylist("Song 1");
        musicPlayer.addToPlaylist("Song 2");
        musicPlayer.addToPlaylist("Song 3");
        
        musicPlayer.play("Song 1");
        musicPlayer.increaseVolume(20);
        musicPlayer.next();
        musicPlayer.pause();
        musicPlayer.play("Song 3");
        musicPlayer.mute();
        musicPlayer.stop();
        musicPlayer.turnOff();
        
        System.out.println("\n------------------------\n");
        
        // Create a video player
        VideoPlayer videoPlayer = new VideoPlayer("MyVideoPlayer");
        videoPlayer.turnOn();
        videoPlayer.play("Movie 1");
        videoPlayer.increaseVolume(10);
        videoPlayer.adjustBrightness(15);
        videoPlayer.pause();
        videoPlayer.play("Movie 1");
        videoPlayer.stop();
        videoPlayer.turnOff();
        
        System.out.println("\n------------------------\n");
        
        // Demonstrate abstraction through reference type
        MediaPlayer player = new MusicPlayer("AbstractPlayer");
        player.turnOn();
        player.play("Song 4");
        player.stop();
        player.turnOff();
    }
}
```

#### 🔄 Abstraction Benefits Demonstrated

1. **Code Organization**: The abstract `MediaPlayer` class provides a common structure for all media players.
2. **Implementation Hiding**: Users of `MusicPlayer` and `VideoPlayer` don't need to know how they work internally.
3. **Contract Enforcement**: The interfaces ensure that all implementing classes provide the required functionality.
4. **Flexibility**: New types of media players can be added by extending `MediaPlayer` and implementing the interfaces.
5. **Polymorphism**: Different media players can be treated uniformly through the `MediaPlayer` reference.

#### ⚠️ Common Abstraction Pitfalls

1. **Over-abstraction**: Creating too many abstract layers can make the code hard to understand.
2. **Under-abstraction**: Not abstracting enough can lead to code duplication and maintenance issues.
3. **Leaky Abstraction**: When implementation details leak through the abstraction layer.
4. **Rigid Abstraction**: Creating abstractions that are too specific and difficult to extend.

#### 🌟 Best Practices

1. Design abstractions based on behavior, not on status or properties
2. Keep interfaces small and focused (Interface Segregation Principle)
3. Program to interfaces, not implementations
4. Use abstract classes for "is-a" relationships with common functionality
5. Use interfaces for "can-do" capabilities that can apply to different class hierarchies

---

## Session 2: Polymorphism (45 minutes)

### 1. Polymorphism Concepts

#### What is Polymorphism?

Polymorphism is derived from Greek words: "poly" meaning many and "morphs" meaning forms. In OOP, polymorphism allows objects of different classes to be treated as objects of a common superclass. It enables one interface to be used for a general class of actions.

#### 🔍 Key Characteristics

- Allows objects to be treated as instances of their parent class
- Enables methods to do different things based on the object they are acting upon
- Provides flexibility and extensibility to the code
- Reduces coupling between different parts of the code

#### 📊 Visual Representation

```
┌───────────────────────────────────────────────────┐
│                                                   │
│                  POLYMORPHISM                     │
│                                                   │
│  ┌─────────────┐                                  │
│  │   Animal    │                                  │
│  │  makeSound()│                                  │
│  └──────┬──────┘                                  │
│         │                                         │
│    ┌────┴────┐                                    │
│    │         │                                    │
│┌───▼───┐ ┌───▼───┐                               │
││  Dog  │ │  Cat  │                               │
││"Woof!"│ │"Meow!"│                               │
│└───────┘ └───────┘                               │
│                                                   │
│  Animal animal1 = new Dog();  // "Woof!"          │
│  Animal animal2 = new Cat();  // "Meow!"          │
│                                                   │
└───────────────────────────────────────────────────┘
```

#### Real-World Analogy

Think of a TV remote control. The "Volume" button works for different TV brands and models. You press the same button (same interface), but different TVs respond differently (different implementations). This is polymorphism - same interface, different behaviors.

#### 🤔 Think About It
How does polymorphism help in creating flexible and extensible code?

<details>
<summary>Answer</summary>
Polymorphism helps create flexible and extensible code by:
<ul>
<li>Allowing new classes to be added with minimal changes to existing code</li>
<li>Enabling code to work with objects of different types through a common interface</li>
<li>Reducing dependencies between different parts of the code</li>
<li>Supporting the "Open/Closed Principle" - open for extension but closed for modification</li>
<li>Facilitating code reuse and reducing redundancy</li>
</ul>
</details>

---

### 2. Types of Polymorphism in Java

Java supports two main types of polymorphism:

#### 1. Compile-time Polymorphism (Static Binding)

Also known as method overloading, this occurs when multiple methods in the same class have the same name but different parameters.

```java
public class Calculator {
    // Method with two int parameters
    public int add(int a, int b) {
        return a + b;
    }
    
    // Method with three int parameters
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    
    // Method with two double parameters
    public double add(double a, double b) {
        return a + b;
    }
    
    // Method with an array parameter
    public int add(int[] numbers) {
        int sum = 0;
        for (int num : numbers) {
            sum += num;
        }
        return sum;
    }
}
```

**Key Points about Method Overloading:**
- Methods must have the same name but different parameter lists
- Differences can be in the number, type, or order of parameters
- Return type alone is not sufficient to distinguish overloaded methods
- Determined at compile time, hence "compile-time polymorphism"

#### 2. Runtime Polymorphism (Dynamic Binding)

Also known as method overriding, this occurs when a subclass provides a specific implementation of a method that is already defined in its superclass.

```java
// Parent class
public class Animal {
    public void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

// Child class
public class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Dog barks: Woof! Woof!");
    }
}

// Child class
public class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Cat meows: Meow!");
    }
}

// Usage
public class Main {
    public static void main(String[] args) {
        Animal animal1 = new Animal();
        Animal animal2 = new Dog();
        Animal animal3 = new Cat();
        
        animal1.makeSound();  // Output: Animal makes a sound
        animal2.makeSound();  // Output: Dog barks: Woof! Woof!
        animal3.makeSound();  // Output: Cat meows: Meow!
    }
}
```

**Key Points about Method Overriding:**
- Subclass provides a specific implementation of a method defined in the superclass
- Method must have the same name, return type, and parameter list
- Access modifier can be the same or less restrictive
- Determined at runtime, hence "runtime polymorphism"
- Achieved through inheritance and method overriding

#### 📊 Comparison: Overloading vs Overriding

| Feature | Method Overloading | Method Overriding |
|---------|-------------------|-------------------|
| Definition | Same method name, different parameters | Same method name, same parameters |
| Occurrence | Within the same class | Between superclass and subclass |
| Return Type | Can be different | Must be the same or covariant |
| Access Modifier | Can be different | Can be same or less restrictive |
| Exception | Can throw different exceptions | Can throw same or subclass exceptions |
| Binding | Compile-time (static) | Runtime (dynamic) |
| Purpose | Provides multiple ways to call a method | Provides specific implementation of a method |

#### 🤔 Think About It
Why is runtime polymorphism considered more powerful than compile-time polymorphism?

<details>
<summary>Answer</summary>
Runtime polymorphism is considered more powerful because:
<ul>
<li>It allows for more flexible and extensible code</li>
<li>It enables true "one interface, multiple implementations" behavior</li>
<li>It supports the Open/Closed Principle by allowing new functionality to be added without modifying existing code</li>
<li>It facilitates loose coupling between different parts of the code</li>
<li>It allows for dynamic method resolution based on the actual object type at runtime</li>
</ul>
</details>

---

### 3. Polymorphism in Action: Real-time Project Example

Let's implement a payment processing system to demonstrate polymorphism:

```java
// Payment interface
public interface Payment {
    boolean processPayment(double amount);
    void getPaymentStatus();
    String getPaymentMethod();
}

// Credit Card Payment
public class CreditCardPayment implements Payment {
    private String cardNumber;
    private String cardHolderName;
    private String expiryDate;
    private String cvv;
    private boolean paymentProcessed;
    
    public CreditCardPayment(String cardNumber, String cardHolderName, 
                            String expiryDate, String cvv) {
        this.cardNumber = cardNumber;
        this.cardHolderName = cardHolderName;
        this.expiryDate = expiryDate;
        this.cvv = cvv;
        this.paymentProcessed = false;
    }
    
    @Override
    public boolean processPayment(double amount) {
        // In a real system, this would connect to a payment gateway
        System.out.println("Processing credit card payment of $" + amount);
        System.out.println("Card Number: " + maskCardNumber(cardNumber));
        System.out.println("Card Holder: " + cardHolderName);
        
        // Simulate payment processing
        boolean success = validateCard() && amount > 0;
        
        if (success) {
            System.out.println("Credit card payment successful");
            paymentProcessed = true;
        } else {
            System.out.println("Credit card payment failed");
        }
        
        return success;
    }
    
    @Override
    public void getPaymentStatus() {
        if (paymentProcessed) {
            System.out.println("Credit card payment has been processed");
        } else {
            System.out.println("Credit card payment has not been processed");
        }
    }
    
    @Override
    public String getPaymentMethod() {
        return "Credit Card";
    }
    
    private boolean validateCard() {
        // In a real system, this would validate the card details
        return cardNumber != null && cardNumber.length() >= 13 && 
               !expiryDate.isEmpty() && !cvv.isEmpty();
    }
    
    private String maskCardNumber(String cardNumber) {
        // Show only last 4 digits
        int length = cardNumber.length();
        if (length <= 4) return cardNumber;
        
        StringBuilder masked = new StringBuilder();
        for (int i = 0; i < length - 4; i++) {
            masked.append("*");
        }
        masked.append(cardNumber.substring(length - 4));
        
        return masked.toString();
    }
}

// PayPal Payment
public class PayPalPayment implements Payment {
    private String email;
    private String password;
    private boolean paymentProcessed;
    
    public PayPalPayment(String email, String password) {
        this.email = email;
        this.password = password;
        this.paymentProcessed = false;
    }
    
    @Override
    public boolean processPayment(double amount) {
        // In a real system, this would connect to PayPal API
        System.out.println("Processing PayPal payment of $" + amount);
        System.out.println("PayPal Account: " + email);
        
        // Simulate payment processing
        boolean success = validateAccount() && amount > 0;
        
        if (success) {
            System.out.println("PayPal payment successful");
            paymentProcessed = true;
        } else {
            System.out.println("PayPal payment failed");
        }
        
        return success;
    }
    
    @Override
    public void getPaymentStatus() {
        if (paymentProcessed) {
            System.out.println("PayPal payment has been processed");
        } else {
            System.out.println("PayPal payment has not been processed");
        }
    }
    
    @Override
    public String getPaymentMethod() {
        return "PayPal";
    }
    
    private boolean validateAccount() {
        // In a real system, this would validate the PayPal account
        return email != null && email.contains("@") && !password.isEmpty();
    }
}

// Bank Transfer Payment
public class BankTransferPayment implements Payment {
    private String accountNumber;
    private String bankCode;
    private String accountHolderName;
    private boolean paymentProcessed;
    
    public BankTransferPayment(String accountNumber, String bankCode, String accountHolderName) {
        this.accountNumber = accountNumber;
        this.bankCode = bankCode;
        this.accountHolderName = accountHolderName;
        this.paymentProcessed = false;
    }
    
    @Override
    public boolean processPayment(double amount) {
        // In a real system, this would connect to bank API
        System.out.println("Processing bank transfer of $" + amount);
        System.out.println("Account Number: " + accountNumber);
        System.out.println("Bank Code: " + bankCode);
        System.out.println("Account Holder: " + accountHolderName);
        
        // Simulate payment processing
        boolean success = validateAccount() && amount > 0;
        
        if (success) {
            System.out.println("Bank transfer successful");
            paymentProcessed = true;
        } else {
            System.out.println("Bank transfer failed");
        }
        
        return success;
    }
    
    @Override
    public void getPaymentStatus() {
        if (paymentProcessed) {
            System.out.println("Bank transfer has been processed");
        } else {
            System.out.println("Bank transfer has not been processed");
        }
    }
    
    @Override
    public String getPaymentMethod() {
        return "Bank Transfer";
    }
    
    private boolean validateAccount() {
        // In a real system, this would validate the bank account
        return accountNumber != null && !accountNumber.isEmpty() && 
               !bankCode.isEmpty() && !accountHolderName.isEmpty();
    }
}

// Order class that uses payment processing
public class Order {
    private int orderId;
    private double amount;
    private Payment paymentMethod;
    private boolean isPaid;
    
    public Order(int orderId, double amount) {
        this.orderId = orderId;
        this.amount = amount;
        this.isPaid = false;
    }
    
    public void setPaymentMethod(Payment paymentMethod) {
        this.paymentMethod = paymentMethod;
    }
    
    public boolean checkout() {
        if (paymentMethod == null) {
            System.out.println("No payment method selected");
            return false;
        }
        
        System.out.println("Processing order #" + orderId + " for $" + amount);
        System.out.println("Payment method: " + paymentMethod.getPaymentMethod());
        
        boolean success = paymentMethod.processPayment(amount);
        if (success) {
            isPaid = true;
            System.out.println("Order #" + orderId + " has been paid");
        } else {
            System.out.println("Payment for order #" + orderId + " failed");
        }
        
        return success;
    }
    
    public void getOrderStatus() {
        System.out.println("Order #" + orderId + " status:");
        System.out.println("Amount: $" + amount);
        System.out.println("Paid: " + (isPaid ? "Yes" : "No"));
        
        if (paymentMethod != null) {
            System.out.println("Payment method: " + paymentMethod.getPaymentMethod());
            paymentMethod.getPaymentStatus();
        } else {
            System.out.println("No payment method selected");
        }
    }
}

// Payment Processor - demonstrates polymorphism
public class PaymentProcessor {
    public static void processPayment(Payment payment, double amount) {
        System.out.println("\nProcessing payment using " + payment.getPaymentMethod());
        payment.processPayment(amount);
        payment.getPaymentStatus();
    }
    
    public static void main(String[] args) {
        // Create different payment methods
        Payment creditCard = new CreditCardPayment("1234567890123456", "John Doe", "12/25", "123");
        Payment payPal = new PayPalPayment("john.doe@example.com", "password123");
        Payment bankTransfer = new BankTransferPayment("987654321", "ABCBANK", "John Doe");
        
        // Process payments using polymorphism
        processPayment(creditCard, 100.50);
        processPayment(payPal, 75.25);
        processPayment(bankTransfer, 200.00);
        
        System.out.println("\n------------------------\n");
        
        // Create and process an order
        Order order = new Order(12345, 150.75);
        
        // Try different payment methods
        order.setPaymentMethod(creditCard);
        boolean success = order.checkout();
        order.getOrderStatus();
        
        System.out.println("\n------------------------\n");
        
        // Create an array of payment methods (polymorphism)
        Payment[] paymentMethods = {
            new CreditCardPayment("9876543210987654", "Jane Smith", "06/24", "456"),
            new PayPalPayment("jane.smith@example.com", "securepass"),
            new BankTransferPayment("123456789", "XYZBANK", "Jane Smith")
        };
        
        // Process all payments
        for (Payment payment : paymentMethods) {
            System.out.println("\nProcessing payment using " + payment.getPaymentMethod());
            payment.processPayment(50.00);
        }
    }
}
```

#### 🔄 Polymorphism Benefits Demonstrated

1. **Code Flexibility**: The `Order` class works with any payment method that implements the `Payment` interface.
2. **Extensibility**: New payment methods can be added without changing the `Order` or `PaymentProcessor` classes.
3. **Simplified Code**: The `processPayment` method handles different payment types with a single implementation.
4. **Reduced Coupling**: The `Order` class doesn't need to know the specific payment implementation details.
5. **Runtime Behavior Selection**: The appropriate payment processing behavior is selected at runtime based on the actual object type.

#### ⚠️ Common Polymorphism Pitfalls

1. **Type Casting Issues**: Incorrect casting can lead to `ClassCastException` at runtime.
2. **Overuse**: Using polymorphism when simpler solutions would suffice can add unnecessary complexity.
3. **Performance Overhead**: Dynamic method dispatch has a slight performance overhead compared to static binding.
4. **Difficult Debugging**: Runtime polymorphism can make code harder to debug as the actual method called is determined at runtime.

#### 🌟 Best Practices

1. Design interfaces based on behavior, not on implementation details
2. Use the "Program to an interface, not an implementation" principle
3. Keep inheritance hierarchies shallow and focused
4. Use method overriding to provide specialized behavior in subclasses
5. Use method overloading judiciously and consistently
6. Document the expected behavior of polymorphic methods

---

## 🧠 Key Takeaways

1. **Abstraction** hides implementation details and shows only functionality
2. **Abstract classes** provide a common base with some implementation
3. **Interfaces** define a contract that implementing classes must follow
4. **Polymorphism** allows objects to be treated as instances of their parent class
5. **Method overloading** provides compile-time polymorphism
6. **Method overriding** enables runtime polymorphism
7. **Both concepts** are fundamental to creating flexible, maintainable OOP code

## 📚 Further Reading

- Java Documentation: [Abstract Methods and Classes](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)
- Java Documentation: [Interfaces](https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html)
- Book: "Effective Java" by Joshua Bloch (Chapter on Interfaces vs. Abstract Classes)
- Article: "Understanding Polymorphism in Java"

## 🔄 Connection to Next Session

In the next session, we'll explore Methods and Abstract Classes in more depth, building on the abstraction and polymorphism concepts we've learned today. We'll see how to design effective abstract classes and how to use method overloading and overriding to create flexible and extensible code.