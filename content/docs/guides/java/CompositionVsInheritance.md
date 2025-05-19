---
title: "Composition vs Inheritance"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 10
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
**Composition vs Inheritance** is one of the most important design decisions in object-oriented programming. Let’s dive deep with code, principles, and practical examples.
---
## 🧠 What Are They?

| Concept | Description |
|--------|-------------|
| **Inheritance** | One class **inherits** fields and methods from another (a "is-a" relationship). |
| **Composition** | One class contains an instance of another class (a "has-a" relationship). |

They are both ways to **reuse** and **organize code**, but they serve different design goals.

---

## 🧬 Inheritance — “is-a” Relationship

### ✅ When to Use:
- When two classes share a **hierarchical** relationship.
- When you want to **extend** or specialize the behavior of a base class.

### 📦 Example:
```java
class Engine {
    void start() {
        System.out.println("Engine started");
    }
}

class Car extends Engine {
    void drive() {
        System.out.println("Car is driving");
    }
}
```

### 🔎 What's happening?
- `Car` **inherits** `start()` from `Engine`.
- But this creates a **tight coupling** — `Car` is forced to **be an Engine**, which might not be accurate.

---

## 🔧 Composition — “has-a” Relationship

### ✅ When to Use:
- When two classes are not logically in a strict hierarchy.
- When you want **flexibility**, **loose coupling**, and better testability.

### 📦 Example:
```java
class Engine {
    void start() {
        System.out.println("Engine started");
    }
}

class Car {
    private Engine engine = new Engine(); // Composition

    void startCar() {
        engine.start(); // Delegation
        System.out.println("Car is ready to drive");
    }
}
```

### 🔎 What's happening?
- `Car` **has an** `Engine`, but is **not** an engine.
- This is **cleaner** and more **realistic** — behavior is **delegated** rather than inherited.

---

## ⚖️ Composition vs Inheritance: Design Comparison

| Aspect | Inheritance | Composition |
|--------|-------------|-------------|
| Relationship | `is-a` | `has-a` |
| Coupling | Tight | Loose |
| Flexibility | Low | High |
| Reusability | In base-child chains | Through delegation |
| Testing | Harder | Easier (swap components) |
| Real-world modeling | Often forced | More accurate |

---

## 💡 Design Principle

> **Favor composition over inheritance** — (Effective Java, Item 18)

Inheritance is powerful but often **overused**. Composition leads to **better encapsulation**, **flexibility**, and **maintainability**.

---

## 🔁 Hybrid Example

You can even mix both:
```java
class Engine {
    void start() {
        System.out.println("Engine starts...");
    }
}

class Vehicle {
    Engine engine = new Engine(); // Composition

    void startVehicle() {
        engine.start();
    }
}

class Car extends Vehicle {
    void drive() {
        System.out.println("Car is driving...");
    }
}
```

Here, `Car` inherits from `Vehicle`, and `Vehicle` uses composition for `Engine`.

---

## 🏠 Homework for Students

### 📌 Task:

1. **Inheritance**:
   - Create a class `Bird` with a method `fly()`.
   - Create a subclass `Sparrow` that adds method `sing()`.

2. **Composition**:
   - Create a class `Speaker` with a method `playMusic()`.
   - Create a class `Smartphone` that has a `Speaker` object and a method `playSong()` which delegates to `playMusic()`.

3. **Questions to Answer** (in comments or writeup):
   - Which approach is more flexible and why?
   - What happens if you need to change `Speaker` logic in the future?
   - Can you reuse `Speaker` in another class without changing it?

---

Would you like to move next into **Static Keyword**, **Final Keyword**, or **Association vs Aggregation vs Composition** for a deeper dive into OOP relationships?