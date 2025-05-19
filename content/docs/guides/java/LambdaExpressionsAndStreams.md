---
title: "Lambda Expressions & Streams"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 19
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Let’s jump into one of the most exciting and modern parts of Java — introduced in **Java 8** — that really transforms how you write clean, expressive, and functional-style code:

---

# ⚡ Lambda Expressions & Streams in Java

---

## 🧠 What Are Lambda Expressions?

Lambda expressions are a **shorthand way to represent an anonymous function** — a block of code that can be passed around and executed later.

### ✅ Before Java 8:
To pass behavior, you had to create an anonymous class:
```java
Runnable r = new Runnable() {
    public void run() {
        System.out.println("Running...");
    }
};
```

### ✅ With Lambda (Java 8+):
```java
Runnable r = () -> System.out.println("Running...");
```

---

## ✅ Why Use Lambdas?

| Benefit              | Description |
|----------------------|-------------|
| Concise Code         | Less boilerplate than anonymous classes |
| Functional Style      | Makes code easier to read and reason about |
| Used in Streams, Collections | Enables powerful operations like filter, map, reduce |
| Foundation of functional interfaces | Used in APIs like `Comparator`, `Runnable`, `Predicate` |

---

## 🔧 Syntax

```java
(parameters) -> { body }
```

### Examples:

```java
// No parameters
() -> System.out.println("Hi")

// One parameter
name -> System.out.println("Hello " + name)

// Multiple parameters
(a, b) -> a + b
```

---

## 🎯 Functional Interface

A **functional interface** is an interface with only **one abstract method**.  
It can have multiple `default` or `static` methods.

### Examples from Java:
- `Runnable`
- `Callable`
- `Comparator<T>`
- `Predicate<T>`
- `Function<T, R>`
- `Consumer<T>`

You can use the `@FunctionalInterface` annotation to enforce this.

```java
@FunctionalInterface
interface MyFunction {
    void apply();
}
```

---

## 📦 Lambda Example with Functional Interface

```java
@FunctionalInterface
interface Greeting {
    void sayHello(String name);
}

public class LambdaDemo {
    public static void main(String[] args) {
        Greeting g = (name) -> System.out.println("Hello " + name);
        g.sayHello("Alice");
    }
}
```

---

# 🌊 Java Streams API

---

## 🔍 What Are Streams?

Streams are a **new way to process data in a functional style**.  
You can perform complex data processing like **filtering, mapping, sorting, and reducing** with clean, readable code.

### ✅ Key Characteristics:
- Declarative (What to do, not how)
- Chainable
- Lazy (operations run only when needed)
- Don’t modify the original collection

---

## 📦 Stream Pipeline

```
Collection -> Stream -> Intermediate Ops -> Terminal Op
```

---

## 🛠 Common Stream Operations

### 🔹 Intermediate Operations

| Operation  | Description                    |
|------------|--------------------------------|
| `filter`   | Keep elements matching a condition |
| `map`      | Transform each element          |
| `sorted`   | Sort stream                     |
| `distinct` | Remove duplicates               |
| `limit`    | Take only N items               |

### 🔹 Terminal Operations

| Operation  | Description                  |
|------------|------------------------------|
| `forEach`  | Iterate over elements        |
| `collect`  | Convert to List, Set, Map    |
| `count`    | Count elements               |
| `reduce`   | Combine into single result   |

---

## 🔧 Stream Example

```java
import java.util.*;

public class StreamExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Alex", "Brian");

        names.stream()
             .filter(name -> name.startsWith("A"))
             .map(String::toUpperCase)
             .sorted()
             .forEach(System.out::println);  // Output: ALEX, ALICE
    }
}
```

---

## 🧠 Real-World Use-Cases

| Task                       | Traditional Java            | Stream API              |
|----------------------------|-----------------------------|--------------------------|
| Filter records             | Loops and ifs               | `filter()`               |
| Transform objects          | Manual mapping              | `map()`                  |
| Aggregate values           | Loop and manual total       | `reduce()` or `collect()`|
| Count, max, min            | Manual comparison           | `count()`, `max()`       |

---

## 🔥 Stream + Collectors

```java
List<String> names = Arrays.asList("Sam", "Samantha", "Sara");

List<String> result = names.stream()
    .filter(name -> name.startsWith("S"))
    .collect(Collectors.toList());
```

---

## 🧠 Summary: Lambda vs Stream

| Feature     | Lambda                        | Stream                             |
|-------------|-------------------------------|-------------------------------------|
| Concept     | Anonymous function             | Sequence of data operations         |
| Goal        | Define behavior                | Process data in a declarative way   |
| Common Usage| Functional Interfaces          | Collections, Maps, Arrays           |

---

## 🏠 Homework for Students

### 1. Functional Interface & Lambda
- Create an interface `MathOperation` with a method `int operate(int a, int b)`
- Implement `add`, `subtract`, `multiply` as lambda expressions

---

### 2. Streams Practice
Given a list of names:
```java
List<String> names = Arrays.asList("John", "Jane", "Jake", "Jill", "Jack");
```

Use Stream API to:
- Filter names starting with `J`
- Convert to uppercase
- Sort alphabetically
- Collect into a new `List<String>`

---

### 3. Use `reduce()` to calculate the sum of a list of integers.

---

### 4. Bonus: Create a method that returns the longest string from a list using Streams.

---

Would you like to continue into **Method References and Optional**, or dive into **Collections Framework deep-dive**, or go toward **File I/O**, **Threads**, or **JDBC** next?