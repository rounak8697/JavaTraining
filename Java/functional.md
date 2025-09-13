
# Functional Interfaces in Java

## What is a Functional Interface?

A **Functional Interface** in Java is an interface that contains exactly **one abstract method**. These interfaces are intended to represent **functional programming constructs** and are often used as **lambda expressions** or **method references**.

---

## Characteristics of a Functional Interface

1. Contains **exactly one abstract method**.
2. Can have multiple **default or static methods**.
3. Annotated with `@FunctionalInterface` (optional but recommended).

---

## Why Use Functional Interfaces?

- Enable **lambda expressions**.
- Simplify code and improve **readability**.
- Encourage **functional programming** styles in Java.

---

## Built-in Functional Interfaces (java.util.function package)

| Interface | Description |
|-----------|-------------|
| `Function<T, R>` | Takes a parameter of type T and returns a result of type R |
| `Consumer<T>`    | Takes a parameter of type T and returns nothing |
| `Supplier<T>`    | Supplies a result of type T with no input |
| `Predicate<T>`   | Takes a parameter of type T and returns a boolean |
| `UnaryOperator<T>` | A special case of Function where input and output types are the same |
| `BinaryOperator<T>` | Takes two arguments of the same type and returns a result of the same type |

---

## Example 1: Custom Functional Interface

```java
@FunctionalInterface
interface MyOperation {
    int operate(int a, int b);
}

public class Main {
    public static void main(String[] args) {
        MyOperation addition = (a, b) -> a + b;
        System.out.println("Result: " + addition.operate(5, 3)); // Output: 8
    }
}
```

---

## Example 2: Using Built-in Functional Interface

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<String, Integer> stringLength = str -> str.length();
        System.out.println("Length: " + stringLength.apply("Hello")); // Output: 5
    }
}
```

---

## Rules for Functional Interface

- Must have **one and only one** abstract method.
- Can have any number of **default** and **static** methods.
- `@FunctionalInterface` helps **prevent accidental breaking** of the contract.

---

## When to Use

Use Functional Interfaces when:

- Writing **lambda expressions**.
- Working with **Streams API**.
- Creating **callback mechanisms**.
