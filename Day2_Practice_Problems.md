# Day 2: Control Structures & Loops - Practice Problems

## 🎯 Skills You'll Practice
- Implementing conditional logic with if-else statements
- Using switch cases for multi-path decision making
- Creating and controlling loops (for, while, do-while)
- Applying break and continue statements
- Implementing nested control structures

## 🧠 Learning Journey
Today's problems will help you apply the control structures and loops you learned in the lecture. You'll start with basic problems and progress to more complex scenarios that combine multiple concepts.

---

## Problem 1: Number Classifier

**Difficulty**: ⭐ Beginner  
**Estimated Time**: 15 minutes  
**Focus**: If-else statements, logical operators

### 📝 Problem Statement
Create a program that classifies a number based on multiple criteria:
- If the number is divisible by both 2 and 3, print "Divisible by 6"
- If the number is divisible by 2 but not 3, print "Even but not divisible by 3"
- If the number is divisible by 3 but not 2, print "Odd and divisible by 3"
- If the number is neither divisible by 2 nor 3, print "Odd and not divisible by 3"

### 🔍 Example
```
Enter a number: 12
Divisible by 6

Enter a number: 8
Even but not divisible by 3

Enter a number: 9
Odd and divisible by 3

Enter a number: 7
Odd and not divisible by 3
```

### 💡 Hints
<details>
<summary>Click for hints</summary>

1. Use the modulo operator (%) to check divisibility
2. A number is divisible by another if the remainder is 0
3. Structure your if-else statements from most specific to most general condition
</details>

### ✅ Solution
```java
import java.util.Scanner;

public class NumberClassifier {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.print("Enter a number: ");
        int number = scanner.nextInt();
        
        if (number % 2 == 0 && number % 3 == 0) {
            System.out.println("Divisible by 6");
        } else if (number % 2 == 0) {
            System.out.println("Even but not divisible by 3");
        } else if (number % 3 == 0) {
            System.out.println("Odd and divisible by 3");
        } else {
            System.out.println("Odd and not divisible by 3");
        }
        
        scanner.close();
    }
}
```

### 🔄 Code Walkthrough
1. We get user input using the Scanner class
2. We check the most specific condition first: if the number is divisible by both 2 and 3
3. Then we check if it's divisible by 2 only
4. Then we check if it's divisible by 3 only
5. If none of these conditions are met, we know it's not divisible by either

### 🚀 Extension Challenge
Modify the program to also check if the number is a prime number.

---

## Problem 2: Season Detector

**Difficulty**: ⭐ Beginner  
**Estimated Time**: 15 minutes  
**Focus**: Switch statements

### 📝 Problem Statement
Create a program that takes a month number (1-12) and determines the season based on the following:
- Winter: December (12), January (1), February (2)
- Spring: March (3), April (4), May (5)
- Summer: June (6), July (7), August (8)
- Fall: September (9), October (10), November (11)

Additionally, ask the user if they are in the Northern or Southern hemisphere, and reverse the seasons if they are in the Southern hemisphere.

### 🔍 Example
```
Enter month (1-12): 4
Northern or Southern hemisphere? (N/S): N
The season is Spring

Enter month (1-12): 4
Northern or Southern hemisphere? (N/S): S
The season is Fall
```

### 💡 Hints
<details>
<summary>Click for hints</summary>

1. Use a switch statement to categorize the months
2. Use the fall-through feature of switch cases for months in the same season
3. For the Southern hemisphere, you can map: Winter→Summer, Spring→Fall, Summer→Winter, Fall→Spring
</details>

### ✅ Solution
```java
import java.util.Scanner;

public class SeasonDetector {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.print("Enter month (1-12): ");
        int month = scanner.nextInt();
        
        System.out.print("Northern or Southern hemisphere? (N/S): ");
        char hemisphere = scanner.next().charAt(0);
        
        String season;
        
        switch (month) {
            case 12:
            case 1:
            case 2:
                season = "Winter";
                break;
            case 3:
            case 4:
            case 5:
                season = "Spring";
                break;
            case 6:
            case 7:
            case 8:
                season = "Summer";
                break;
            case 9:
            case 10:
            case 11:
                season = "Fall";
                break;
            default:
                season = "Invalid month";
                break;
        }
        
        // Reverse seasons for Southern hemisphere
        if (hemisphere == 'S' || hemisphere == 's') {
            switch (season) {
                case "Winter":
                    season = "Summer";
                    break;
                case "Spring":
                    season = "Fall";
                    break;
                case "Summer":
                    season = "Winter";
                    break;
                case "Fall":
                    season = "Spring";
                    break;
            }
        }
        
        System.out.println("The season is " + season);
        
        scanner.close();
    }
}
```

### 🔄 Code Walkthrough
1. We get the month number and hemisphere choice from the user
2. We use a switch statement with fall-through cases to determine the season
3. If the user is in the Southern hemisphere, we use another switch to reverse the season
4. We output the final season

### 🚀 Extension Challenge
Enhance the program to also display the typical temperature range and weather characteristics for the determined season.

---

## Problem 3: Pattern Generator

**Difficulty**: ⭐⭐ Intermediate  
**Estimated Time**: 20 minutes  
**Focus**: Nested loops, pattern printing

### 📝 Problem Statement
Create a program that generates different patterns based on user input. The program should:
1. Ask the user for a pattern type (1, 2, or 3)
2. Ask for the number of rows (n)
3. Print the corresponding pattern:

**Pattern 1**: Right triangle
```
*
* *
* * *
* * * *
```

**Pattern 2**: Inverted right triangle
```
* * * *
* * *
* *
*
```

**Pattern 3**: Pyramid
```
    *
   ***
  *****
 *******
```

### 🔍 Example
```
Enter pattern type (1, 2, or 3): 1
Enter number of rows: 4
*
* *
* * *
* * * *
```

### 💡 Hints
<details>
<summary>Click for hints</summary>

1. Use nested loops - outer loop for rows, inner loop(s) for columns
2. For Pattern 3 (pyramid), you'll need to print spaces before the stars
3. The formula for stars in a pyramid: 2*i-1 (where i is the row number)
</details>

### ✅ Solution
```java
import java.util.Scanner;

public class PatternGenerator {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.print("Enter pattern type (1, 2, or 3): ");
        int patternType = scanner.nextInt();
        
        System.out.print("Enter number of rows: ");
        int rows = scanner.nextInt();
        
        switch (patternType) {
            case 1:
                printRightTriangle(rows);
                break;
            case 2:
                printInvertedRightTriangle(rows);
                break;
            case 3:
                printPyramid(rows);
                break;
            default:
                System.out.println("Invalid pattern type!");
        }
        
        scanner.close();
    }
    
    public static void printRightTriangle(int rows) {
        for (int i = 1; i <= rows; i++) {
            for (int j = 1; j <= i; j++) {
                System.out.print("* ");
            }
            System.out.println();
        }
    }
    
    public static void printInvertedRightTriangle(int rows) {
        for (int i = rows; i >= 1; i--) {
            for (int j = 1; j <= i; j++) {
                System.out.print("* ");
            }
            System.out.println();
        }
    }
    
    public static void printPyramid(int rows) {
        for (int i = 1; i <= rows; i++) {
            // Print spaces
            for (int j = 1; j <= rows - i; j++) {
                System.out.print(" ");
            }
            
            // Print stars
            for (int k = 1; k <= 2 * i - 1; k++) {
                System.out.print("*");
            }
            
            System.out.println();
        }
    }
}
```

### 🔄 Code Walkthrough
1. We get the pattern type and number of rows from the user
2. Based on the pattern type, we call the appropriate method
3. Each method uses nested loops to print the pattern:
   - For the right triangle, we print i stars in row i
   - For the inverted triangle, we print i stars in row (rows-i+1)
   - For the pyramid, we print spaces first, then stars

### 🚀 Extension Challenge
Add a fourth pattern option that prints a diamond shape.

---

## Problem 4: Number Guessing Game

**Difficulty**: ⭐⭐ Intermediate  
**Estimated Time**: 25 minutes  
**Focus**: While loops, if-else, user input validation

### 📝 Problem Statement
Create a number guessing game where:
1. The program generates a random number between 1 and 100
2. The player has to guess the number
3. After each guess, the program gives a hint: "Too high" or "Too low"
4. The game continues until the player guesses correctly
5. Display the number of attempts it took to guess correctly
6. Ask if the player wants to play again

### 🔍 Example
```
I'm thinking of a number between 1 and 100.
Enter your guess: 50
Too low!
Enter your guess: 75
Too high!
Enter your guess: 62
Too low!
Enter your guess: 68
Correct! You guessed the number in 4 attempts.
Do you want to play again? (y/n): n
Thanks for playing!
```

### 💡 Hints
<details>
<summary>Click for hints</summary>

1. Use `Math.random()` to generate a random number
2. Use a while loop to continue the game until the correct guess
3. Use a counter variable to track the number of attempts
4. Use a do-while loop to implement the "play again" feature
</details>

### ✅ Solution
```java
import java.util.Scanner;
import java.util.Random;

public class NumberGuessingGame {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Random random = new Random();
        
        char playAgain;
        
        do {
            // Generate random number between 1 and 100
            int secretNumber = random.nextInt(100) + 1;
            int guess;
            int attempts = 0;
            
            System.out.println("I'm thinking of a number between 1 and 100.");
            
            // Game loop
            do {
                System.out.print("Enter your guess: ");
                guess = scanner.nextInt();
                attempts++;
                
                if (guess < secretNumber) {
                    System.out.println("Too low!");
                } else if (guess > secretNumber) {
                    System.out.println("Too high!");
                } else {
                    System.out.println("Correct! You guessed the number in " + attempts + " attempts.");
                }
                
            } while (guess != secretNumber);
            
            System.out.print("Do you want to play again? (y/n): ");
            playAgain = scanner.next().charAt(0);
            
        } while (playAgain == 'y' || playAgain == 'Y');
        
        System.out.println("Thanks for playing!");
        scanner.close();
    }
}
```

### 🔄 Code Walkthrough
1. We use a do-while loop to implement the "play again" feature
2. Inside, we generate a random number between 1 and 100
3. We use another do-while loop for the guessing process
4. We increment the attempts counter with each guess
5. We provide feedback based on the guess (too high, too low, or correct)
6. After the player guesses correctly, we ask if they want to play again

### 🚀 Extension Challenge
Add difficulty levels to the game:
- Easy: 1-50 with unlimited guesses
- Medium: 1-100 with unlimited guesses
- Hard: 1-100 with a maximum of 7 guesses

---

## Problem 5: Prime Number Finder

**Difficulty**: ⭐⭐⭐ Advanced  
**Estimated Time**: 30 minutes  
**Focus**: Loops, break statement, algorithm implementation

### 📝 Problem Statement
Create a program that:
1. Asks the user for a range (start and end numbers)
2. Finds and displays all prime numbers within that range
3. Counts and displays the total number of prime numbers found
4. Calculates and displays the execution time of the algorithm

A prime number is a natural number greater than 1 that is not divisible by any number other than 1 and itself.

### 🔍 Example
```
Enter start of range: 10
Enter end of range: 50
Prime numbers between 10 and 50:
11 13 17 19 23 29 31 37 41 43 47
Total prime numbers found: 11
Execution time: 0.003 seconds
```

### 💡 Hints
<details>
<summary>Click for hints</summary>

1. To check if a number is prime, you only need to check divisibility up to the square root of the number
2. Use the break statement to exit the checking loop early if a divisor is found
3. Use `System.currentTimeMillis()` to measure execution time
4. Consider using a boolean flag to track if a number is prime
</details>

### ✅ Solution
```java
import java.util.Scanner;

public class PrimeNumberFinder {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.print("Enter start of range: ");
        int start = scanner.nextInt();
        
        System.out.print("Enter end of range: ");
        int end = scanner.nextInt();
        
        // Ensure start is at least 2 (smallest prime)
        start = Math.max(start, 2);
        
        System.out.println("Prime numbers between " + start + " and " + end + ":");
        
        long startTime = System.currentTimeMillis();
        
        int count = 0;
        for (int num = start; num <= end; num++) {
            boolean isPrime = true;
            
            // Check if num is prime
            for (int i = 2; i <= Math.sqrt(num); i++) {
                if (num % i == 0) {
                    isPrime = false;
                    break;  // Exit the loop early if divisor found
                }
            }
            
            if (isPrime) {
                System.out.print(num + " ");
                count++;
            }
        }
        
        long endTime = System.currentTimeMillis();
        double executionTime = (endTime - startTime) / 1000.0;
        
        System.out.println("\nTotal prime numbers found: " + count);
        System.out.println("Execution time: " + executionTime + " seconds");
        
        scanner.close();
    }
}
```

### 🔄 Code Walkthrough
1. We get the range from the user and ensure the start is at least 2
2. We measure the start time before beginning the algorithm
3. For each number in the range, we check if it's prime:
   - We assume it's prime (isPrime = true)
   - We check divisibility from 2 up to the square root of the number
   - If we find a divisor, we set isPrime to false and break out of the loop
4. If the number is prime, we print it and increment our counter
5. We calculate and display the execution time and total count

### 🚀 Extension Challenge
Implement the Sieve of Eratosthenes algorithm, which is more efficient for finding prime numbers in a range, and compare its execution time with your original solution.

---

## 🏆 Comprehensive Challenge: Menu-Driven Calculator

**Difficulty**: ⭐⭐⭐ Advanced  
**Estimated Time**: 40 minutes  
**Focus**: All control structures, loops, and user input validation

### 📝 Problem Statement
Create a menu-driven calculator that:
1. Displays a menu of operations:
   - Basic operations (add, subtract, multiply, divide)
   - Advanced operations (power, square root, factorial)
   - Number series (Fibonacci, prime numbers up to n)
   - Exit
2. Takes user input for the operation choice
3. Takes appropriate inputs based on the operation
4. Performs the calculation and displays the result
5. Loops back to the menu until the user chooses to exit
6. Implements proper input validation for all inputs

### 🔍 Example
```
===== CALCULATOR MENU =====
1. Addition
2. Subtraction
3. Multiplication
4. Division
5. Power
6. Square Root
7. Factorial
8. Fibonacci Series
9. Prime Numbers
0. Exit

Enter your choice: 7
Enter a number: 5
Factorial of 5 is 120

===== CALCULATOR MENU =====
...
Enter your choice: 8
Enter the number of terms: 6
Fibonacci Series: 0 1 1 2 3 5

===== CALCULATOR MENU =====
...
Enter your choice: 0
Thank you for using the calculator!
```

### 💡 Hints
<details>
<summary>Click for hints</summary>

1. Use a do-while loop for the main menu
2. Use switch-case to handle different operations
3. Create separate methods for each operation
4. Implement input validation using loops
5. For division, check for division by zero
6. For factorial and Fibonacci, handle edge cases
</details>

### ✅ Solution
```java
import java.util.Scanner;

public class MenuDrivenCalculator {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int choice;
        
        do {
            displayMenu();
            System.out.print("Enter your choice: ");
            
            // Input validation for menu choice
            while (!scanner.hasNextInt()) {
                System.out.println("Invalid input! Please enter a number.");
                scanner.next(); // Consume invalid input
                System.out.print("Enter your choice: ");
            }
            
            choice = scanner.nextInt();
            
            switch (choice) {
                case 1: // Addition
                    performAddition(scanner);
                    break;
                case 2: // Subtraction
                    performSubtraction(scanner);
                    break;
                case 3: // Multiplication
                    performMultiplication(scanner);
                    break;
                case 4: // Division
                    performDivision(scanner);
                    break;
                case 5: // Power
                    performPower(scanner);
                    break;
                case 6: // Square Root
                    performSquareRoot(scanner);
                    break;
                case 7: // Factorial
                    performFactorial(scanner);
                    break;
                case 8: // Fibonacci Series
                    performFibonacci(scanner);
                    break;
                case 9: // Prime Numbers
                    performPrimeNumbers(scanner);
                    break;
                case 0: // Exit
                    System.out.println("Thank you for using the calculator!");
                    break;
                default:
                    System.out.println("Invalid choice! Please try again.");
            }
            
            // Add a line break for better readability
            if (choice != 0) {
                System.out.println("\nPress Enter to continue...");
                scanner.nextLine(); // Consume newline
                scanner.nextLine(); // Wait for user input
            }
            
        } while (choice != 0);
        
        scanner.close();
    }
    
    public static void displayMenu() {
        System.out.println("===== CALCULATOR MENU =====");
        System.out.println("1. Addition");
        System.out.println("2. Subtraction");
        System.out.println("3. Multiplication");
        System.out.println("4. Division");
        System.out.println("5. Power");
        System.out.println("6. Square Root");
        System.out.println("7. Factorial");
        System.out.println("8. Fibonacci Series");
        System.out.println("9. Prime Numbers");
        System.out.println("0. Exit");
        System.out.println("===========================");
    }
    
    public static void performAddition(Scanner scanner) {
        System.out.print("Enter first number: ");
        double num1 = getValidDoubleInput(scanner);
        
        System.out.print("Enter second number: ");
        double num2 = getValidDoubleInput(scanner);
        
        double result = num1 + num2;
        System.out.println(num1 + " + " + num2 + " = " + result);
    }
    
    public static void performSubtraction(Scanner scanner) {
        System.out.print("Enter first number: ");
        double num1 = getValidDoubleInput(scanner);
        
        System.out.print("Enter second number: ");
        double num2 = getValidDoubleInput(scanner);
        
        double result = num1 - num2;
        System.out.println(num1 + " - " + num2 + " = " + result);
    }
    
    public static void performMultiplication(Scanner scanner) {
        System.out.print("Enter first number: ");
        double num1 = getValidDoubleInput(scanner);
        
        System.out.print("Enter second number: ");
        double num2 = getValidDoubleInput(scanner);
        
        double result = num1 * num2;
        System.out.println(num1 + " * " + num2 + " = " + result);
    }
    
    public static void performDivision(Scanner scanner) {
        System.out.print("Enter numerator: ");
        double num1 = getValidDoubleInput(scanner);
        
        double num2;
        do {
            System.out.print("Enter denominator (non-zero): ");
            num2 = getValidDoubleInput(scanner);
            if (num2 == 0) {
                System.out.println("Error: Division by zero is not allowed!");
            }
        } while (num2 == 0);
        
        double result = num1 / num2;
        System.out.println(num1 + " / " + num2 + " = " + result);
    }
    
    public static void performPower(Scanner scanner) {
        System.out.print("Enter base: ");
        double base = getValidDoubleInput(scanner);
        
        System.out.print("Enter exponent: ");
        double exponent = getValidDoubleInput(scanner);
        
        double result = Math.pow(base, exponent);
        System.out.println(base + " ^ " + exponent + " = " + result);
    }
    
    public static void performSquareRoot(Scanner scanner) {
        double num;
        do {
            System.out.print("Enter a non-negative number: ");
            num = getValidDoubleInput(scanner);
            if (num < 0) {
                System.out.println("Error: Cannot calculate square root of a negative number!");
            }
        } while (num < 0);
        
        double result = Math.sqrt(num);
        System.out.println("Square root of " + num + " = " + result);
    }
    
    public static void performFactorial(Scanner scanner) {
        int num;
        do {
            System.out.print("Enter a non-negative integer: ");
            num = getValidIntInput(scanner);
            if (num < 0) {
                System.out.println("Error: Factorial is not defined for negative numbers!");
            }
        } while (num < 0);
        
        long factorial = 1;
        for (int i = 2; i <= num; i++) {
            factorial *= i;
        }
        
        System.out.println("Factorial of " + num + " is " + factorial);
    }
    
    public static void performFibonacci(Scanner scanner) {
        int terms;
        do {
            System.out.print("Enter the number of terms (positive): ");
            terms = getValidIntInput(scanner);
            if (terms <= 0) {
                System.out.println("Error: Number of terms must be positive!");
            }
        } while (terms <= 0);
        
        System.out.print("Fibonacci Series: ");
        
        int a = 0, b = 1;
        System.out.print(a);
        
        if (terms > 1) {
            System.out.print(" " + b);
        }
        
        for (int i = 3; i <= terms; i++) {
            int next = a + b;
            System.out.print(" " + next);
            a = b;
            b = next;
        }
        
        System.out.println();
    }
    
    public static void performPrimeNumbers(Scanner scanner) {
        int limit;
        do {
            System.out.print("Enter the upper limit (>= 2): ");
            limit = getValidIntInput(scanner);
            if (limit < 2) {
                System.out.println("Error: Limit must be at least 2!");
            }
        } while (limit < 2);
        
        System.out.println("Prime numbers up to " + limit + ":");
        
        int count = 0;
        for (int num = 2; num <= limit; num++) {
            boolean isPrime = true;
            
            for (int i = 2; i <= Math.sqrt(num); i++) {
                if (num % i == 0) {
                    isPrime = false;
                    break;
                }
            }
            
            if (isPrime) {
                System.out.print(num + " ");
                count++;
            }
        }
        
        System.out.println("\nTotal prime numbers found: " + count);
    }
    
    public static double getValidDoubleInput(Scanner scanner) {
        while (!scanner.hasNextDouble()) {
            System.out.println("Invalid input! Please enter a number.");
            scanner.next(); // Consume invalid input
            System.out.print("Try again: ");
        }
        return scanner.nextDouble();
    }
    
    public static int getValidIntInput(Scanner scanner) {
        while (!scanner.hasNextInt()) {
            System.out.println("Invalid input! Please enter an integer.");
            scanner.next(); // Consume invalid input
            System.out.print("Try again: ");
        }
        return scanner.nextInt();
    }
}
```

### 🔄 Code Walkthrough
1. We create a menu-driven interface using a do-while loop
2. We use a switch statement to handle different operations
3. Each operation is implemented in its own method for better organization
4. We implement input validation for all user inputs
5. We handle edge cases like division by zero and negative inputs for square root and factorial
6. We provide a "Press Enter to continue" prompt between operations

### 🚀 Extension Challenge
Add memory functionality to the calculator:
- Add options to store a result in memory
- Add options to recall a value from memory
- Add options to clear the memory
- Allow using the memory value in calculations

---

## 📚 Key Concepts Practiced

1. **Conditional Logic**
   - If-else statements for decision making
   - Switch statements for multi-path branching
   - Nested conditionals for complex decisions

2. **Loops**
   - For loops for counting and iterating
   - While loops for condition-based iteration
   - Do-while loops for at least one execution
   - Nested loops for multi-dimensional operations

3. **Control Flow**
   - Break statements to exit loops
   - Continue statements to skip iterations
   - Return statements to exit methods

4. **Input Validation**
   - Checking for valid numeric input
   - Handling edge cases and constraints
   - Providing user feedback for invalid inputs

5. **Algorithm Implementation**
   - Prime number checking
   - Fibonacci sequence generation
   - Factorial calculation

## 🔍 Further Exploration
- Research the efficiency of different looping structures
- Explore Java's enhanced for loop with more complex data structures
- Learn about recursion as an alternative to iteration
- Study the time complexity of algorithms implemented with loops