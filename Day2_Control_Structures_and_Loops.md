# Day 2: Control Structures & Loops

## Session 1: Java Loops (45 minutes)

### 1. If-Else Statements

**Basic If Statement:**
```java
int age = 18;
if (age >= 18) {
    System.out.println("You are eligible to vote!");
}
```

**If-Else Statement:**
```java
int number = 15;
if (number % 2 == 0) {
    System.out.println(number + " is even");
} else {
    System.out.println(number + " is odd");
}
```

**If-Else-If Ladder:**
```java
int marks = 85;
if (marks >= 90) {
    System.out.println("Grade A+");
} else if (marks >= 80) {
    System.out.println("Grade A");
} else if (marks >= 70) {
    System.out.println("Grade B");
} else if (marks >= 60) {
    System.out.println("Grade C");
} else {
    System.out.println("Grade F");
}
```

**Nested If Statements:**
```java
int age = 25;
boolean hasLicense = true;

if (age >= 18) {
    if (hasLicense) {
        System.out.println("You can drive!");
    } else {
        System.out.println("You need a license to drive.");
    }
} else {
    System.out.println("You are too young to drive.");
}
```

### 2. Switch Case

**Basic Switch Statement:**
```java
int day = 3;
switch (day) {
    case 1:
        System.out.println("Monday");
        break;
    case 2:
        System.out.println("Tuesday");
        break;
    case 3:
        System.out.println("Wednesday");
        break;
    case 4:
        System.out.println("Thursday");
        break;
    case 5:
        System.out.println("Friday");
        break;
    case 6:
        System.out.println("Saturday");
        break;
    case 7:
        System.out.println("Sunday");
        break;
    default:
        System.out.println("Invalid day");
}
```

**Switch with String (Java 7+):**
```java
String grade = "A";
switch (grade) {
    case "A":
        System.out.println("Excellent!");
        break;
    case "B":
        System.out.println("Good!");
        break;
    case "C":
        System.out.println("Average");
        break;
    case "D":
        System.out.println("Below Average");
        break;
    case "F":
        System.out.println("Fail");
        break;
    default:
        System.out.println("Invalid grade");
}
```

**Switch without Break (Fall-through):**
```java
int month = 2;
switch (month) {
    case 12:
    case 1:
    case 2:
        System.out.println("Winter");
        break;
    case 3:
    case 4:
    case 5:
        System.out.println("Spring");
        break;
    case 6:
    case 7:
    case 8:
        System.out.println("Summer");
        break;
    case 9:
    case 10:
    case 11:
        System.out.println("Autumn");
        break;
    default:
        System.out.println("Invalid month");
}
```

### 3. For Loop

**Basic For Loop:**
```java
// Print numbers 1 to 10
for (int i = 1; i <= 10; i++) {
    System.out.println("Number: " + i);
}
```

**For Loop with Different Increments:**
```java
// Print even numbers from 2 to 20
for (int i = 2; i <= 20; i += 2) {
    System.out.println("Even number: " + i);
}
```

**Reverse For Loop:**
```java
// Print numbers from 10 to 1
for (int i = 10; i >= 1; i--) {
    System.out.println("Countdown: " + i);
}
```

**Enhanced For Loop (For-Each):**
```java
int[] numbers = {1, 2, 3, 4, 5};
for (int num : numbers) {
    System.out.println("Array element: " + num);
}
```

### 4. While Loop

**Basic While Loop:**
```java
int i = 1;
while (i <= 5) {
    System.out.println("Iteration: " + i);
    i++;
}
```

**While Loop with User Input:**
```java
Scanner scanner = new Scanner(System.in);
String input = "";
while (!input.equals("exit")) {
    System.out.print("Enter command (type 'exit' to quit): ");
    input = scanner.nextLine();
    System.out.println("You entered: " + input);
}
```

**Infinite While Loop (with break):**
```java
int counter = 0;
while (true) {
    System.out.println("Counter: " + counter);
    counter++;
    if (counter >= 5) {
        break;
    }
}
```

### 5. Do-While Loop

**Basic Do-While Loop:**
```java
int i = 1;
do {
    System.out.println("Iteration: " + i);
    i++;
} while (i <= 5);
```

**Do-While vs While Comparison:**
```java
// This will execute at least once
int x = 10;
do {
    System.out.println("Do-While: " + x);
    x++;
} while (x < 10);

// This will not execute at all
int y = 10;
while (y < 10) {
    System.out.println("While: " + y);
    y++;
}
```

---

## Session 2: Control Flow Statements (45 minutes)

### 1. Break Statement

**Break in For Loop:**
```java
for (int i = 1; i <= 10; i++) {
    if (i == 5) {
        break;  // Exit loop when i equals 5
    }
    System.out.println("Number: " + i);
}
// Output: 1, 2, 3, 4
```

**Break in While Loop:**
```java
int i = 1;
while (true) {
    if (i > 5) {
        break;
    }
    System.out.println("Count: " + i);
    i++;
}
```

**Break in Switch Statement:**
```java
int choice = 2;
switch (choice) {
    case 1:
        System.out.println("Option 1");
        break;  // Prevents fall-through
    case 2:
        System.out.println("Option 2");
        break;
    case 3:
        System.out.println("Option 3");
        break;
    default:
        System.out.println("Invalid option");
}
```

### 2. Continue Statement

**Continue in For Loop:**
```java
for (int i = 1; i <= 10; i++) {
    if (i % 2 == 0) {
        continue;  // Skip even numbers
    }
    System.out.println("Odd number: " + i);
}
// Output: 1, 3, 5, 7, 9
```

**Continue in While Loop:**
```java
int i = 0;
while (i < 10) {
    i++;
    if (i % 3 == 0) {
        continue;  // Skip multiples of 3
    }
    System.out.println("Number: " + i);
}
```

### 3. Nested Loops

**Nested For Loops:**
```java
// Print multiplication table
for (int i = 1; i <= 5; i++) {
    for (int j = 1; j <= 5; j++) {
        System.out.print((i * j) + "\t");
    }
    System.out.println();
}
```

**Pattern Printing:**
```java
// Print star pattern
for (int i = 1; i <= 5; i++) {
    for (int j = 1; j <= i; j++) {
        System.out.print("* ");
    }
    System.out.println();
}
/*
Output:
* 
* * 
* * * 
* * * * 
* * * * * 
*/
```

### 4. Labeled Break and Continue

**Labeled Break:**
```java
outer: for (int i = 1; i <= 3; i++) {
    for (int j = 1; j <= 3; j++) {
        if (i == 2 && j == 2) {
            break outer;  // Break out of both loops
        }
        System.out.println("i=" + i + ", j=" + j);
    }
}
```

**Labeled Continue:**
```java
outer: for (int i = 1; i <= 3; i++) {
    for (int j = 1; j <= 3; j++) {
        if (j == 2) {
            continue outer;  // Continue with next iteration of outer loop
        }
        System.out.println("i=" + i + ", j=" + j);
    }
}
```

### 5. Loop Control Best Practices

**1. Avoid Infinite Loops:**
```java
// Bad - potential infinite loop
int i = 0;
while (i < 10) {
    System.out.println(i);
    // Forgot to increment i
}

// Good - proper increment
int i = 0;
while (i < 10) {
    System.out.println(i);
    i++;
}
```

**2. Use Appropriate Loop Type:**
```java
// Use for loop when you know the number of iterations
for (int i = 0; i < array.length; i++) {
    // Process array elements
}

// Use while loop when condition-based
while (scanner.hasNext()) {
    // Process input
}

// Use do-while when you need at least one execution
do {
    // Get user input
} while (input.equals("continue"));
```

**3. Keep Loop Body Simple:**
```java
// Good - simple loop body
for (int i = 0; i < numbers.length; i++) {
    processNumber(numbers[i]);
}

// Better - extract complex logic to methods
private void processNumber(int number) {
    // Complex processing logic here
}
```

---

## Common Loop Patterns

### 1. Input Validation Loop
```java
Scanner scanner = new Scanner(System.in);
int number;
do {
    System.out.print("Enter a positive number: ");
    number = scanner.nextInt();
    if (number <= 0) {
        System.out.println("Please enter a positive number!");
    }
} while (number <= 0);
```

### 2. Menu-Driven Program
```java
Scanner scanner = new Scanner(System.in);
int choice;
do {
    System.out.println("1. Add");
    System.out.println("2. Subtract");
    System.out.println("3. Exit");
    System.out.print("Enter your choice: ");
    choice = scanner.nextInt();
    
    switch (choice) {
        case 1:
            // Add operation
            break;
        case 2:
            // Subtract operation
            break;
        case 3:
            System.out.println("Goodbye!");
            break;
        default:
            System.out.println("Invalid choice!");
    }
} while (choice != 3);
```

### 3. Array Processing
```java
int[] numbers = {1, 2, 3, 4, 5};
int sum = 0;

// Calculate sum using for loop
for (int i = 0; i < numbers.length; i++) {
    sum += numbers[i];
}

// Find maximum using enhanced for loop
int max = numbers[0];
for (int num : numbers) {
    if (num > max) {
        max = num;
    }
}
```

---

## Key Takeaways
1. **If-Else**: Use for conditional execution
2. **Switch**: Better for multiple discrete values
3. **For Loop**: Best when you know iteration count
4. **While Loop**: Best for condition-based iteration
5. **Do-While**: Guarantees at least one execution
6. **Break**: Exits the loop completely
7. **Continue**: Skips current iteration
8. **Nested Loops**: Useful for 2D operations
9. **Labeled Statements**: Control nested loop flow
10. **Best Practices**: Keep loops simple and avoid infinite loops
