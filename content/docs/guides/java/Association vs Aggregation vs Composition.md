---
title: "Static and Final modifiers"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 12
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
Let’s tackle two foundational modifiers in Java that have a huge impact on **memory management**, **immutability**, and **class behavior**: `static` and `final`.

---
## 1️⃣ `static` Keyword — Belongs to the Class, Not the Object

### 🧠 What it does:
When you declare something as `static`, it becomes **class-level** — not tied to any individual object.

---

### ✅ Use Cases of `static`:
| Element | Meaning |
|--------|---------|
| `static variable` | Shared across all instances |
| `static method` | Can be called without creating an object |
| `static block` | Executes once when class is loaded |
| `static class` | Used for nested classes only |

---

### 📦 Example: Static Variable & Method

```java
class Counter {
    static int count = 0;  // Shared by all objects

    Counter() {
        count++;  // Increment shared count
    }

    static void showCount() {
        System.out.println("Objects created: " + count);
    }
}

public class Main {
    public static void main(String[] args) {
        new Counter();
        new Counter();
        Counter.showCount();  // Outputs: 2
    }
}
```

### 🎯 Why Use `static`?

- **Memory-efficient**: One copy shared across all instances.
- Useful for **utility** methods (e.g., `Math.sqrt()`, `Collections.sort()`).
- Implements constants (`static final`) that don’t change.

---

## 2️⃣ `final` Keyword — Cannot Be Changed

### 🧠 What it does:
When something is declared `final`, it means it **cannot be changed or overridden** after it's assigned or defined.

---

### ✅ Use Cases of `final`:
| Element | Meaning |
|--------|---------|
| `final variable` | Value cannot be reassigned |
| `final method` | Cannot be overridden |
| `final class` | Cannot be subclassed |

---

### 📦 Example: Final Variable

```java
class Constants {
    static final double PI = 3.14159;  // Constant value
}
```

### 📦 Example: Final Method

```java
class Vehicle {
    final void start() {
        System.out.println("Vehicle is starting");
    }
}

class Car extends Vehicle {
    // Cannot override start() here
}
```

### 📦 Example: Final Class

```java
final class Calculator {
    int add(int a, int b) {
        return a + b;
    }
}

// class AdvancedCalculator extends Calculator {}  // ❌ Error
```

---

## 🔎 `static final` Combo — Constant

```java
class Config {
    public static final String APP_NAME = "MyApp";
}
```

This is how Java defines **constants**:
- `static`: shared by all
- `final`: value cannot change

---

## 🧠 Why Use `final`?

- Ensures **immutability**.
- Helps prevent **accidental overrides** or changes.
- Often used in **security**, **constants**, and **thread-safety**.

---

## 🧩 Deeper Design Insight

### `static` relates to **class-level design**:
- Shared resources
- Singleton patterns
- Utility classes

### `final` supports **immutability** and **safe OOP**:
- Final variables = safe data
- Final methods/classes = control behavior

---

## 🏠 Homework for Students

### 📌 Tasks:

1. Create a `Bank` class with:
   - A `static` variable `totalAccounts` that increments every time a new account is created.
   - A `final` variable `accountNumber` initialized in the constructor.

2. Create a `Utility` class with only:
   - `static` methods: `add`, `subtract`, etc.
   - Prove that you can use them without instantiating the class.

3. Create a `final` class `SecurityProtocol` with a method `validate()`.

4. Try overriding a `final` method in a subclass. Document the compiler error.

---

Ready to move into **Exception Handling**, or should we continue with keywords like `this`, `super`, or access control like `public/private/protected` in depth?