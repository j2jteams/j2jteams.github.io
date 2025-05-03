---
title: "Association vs Aggregation vs Composition"
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
Understanding the **nuances between Association, Aggregation, and Composition** is essential for designing **robust, scalable** object-oriented systems. These relationships define how classes **interact** with each other — think of them as the "glue" in your application architecture.

---

# 🔗 Java Core Concept: Association vs Aggregation vs Composition

---

## 🧠 What Are These?

These are all forms of **"has-a" relationships** between objects, but they differ in **strength** of connection and **lifecycle dependency**.

| Concept      | Type of Relationship | Lifespan Dependency | Ownership |
|--------------|----------------------|----------------------|-----------|
| **Association** | "uses-a" / "has-a" | Independent | No |
| **Aggregation** | Whole–Part (loose) | Independent | Shared |
| **Composition** | Whole–Part (strong) | Dependent | Exclusive |

---

## 1️⃣ Association

**Definition**: A general relationship between two classes where one class **uses** or **references** another.

- No ownership.
- Both can exist independently.
- Often implemented via a field, method parameter, or local variable.

### 📦 Example:
```java
class Driver {
    String name;

    Driver(String name) {
        this.name = name;
    }

    void drive(Car car) {
        System.out.println(name + " is driving the car.");
    }
}

class Car {
    String model;

    Car(String model) {
        this.model = model;
    }
}
```

### 🔍 Notes:
- `Driver` and `Car` have an **association**.
- `Driver` uses a `Car`, but doesn’t own or control its lifecycle.

---

## 2️⃣ Aggregation

**Definition**: A specialized form of Association where one class **has** another, but they can live **independently**.

- Represents a **"whole-part"** relationship.
- **Weak ownership**: Parts can belong to multiple wholes.

### 📦 Example:
```java
class Department {
    String name;
    List<Teacher> teachers;

    Department(String name, List<Teacher> teachers) {
        this.name = name;
        this.teachers = teachers;
    }
}

class Teacher {
    String name;

    Teacher(String name) {
        this.name = name;
    }
}
```

### 🔍 Notes:
- `Department` aggregates `Teacher`s.
- If a department is deleted, the teachers still exist.
- One teacher could belong to multiple departments.

---

## 3️⃣ Composition

**Definition**: A strong form of Aggregation where the child object **cannot exist without the parent**.

- **Strong ownership**.
- Lifecycles are tightly coupled.

### 📦 Example:
```java
class House {
    private Room room;

    House() {
        this.room = new Room();
    }

    void show() {
        room.describe();
    }
}

class Room {
    void describe() {
        System.out.println("This is a room in the house.");
    }
}
```

### 🔍 Notes:
- `Room` exists **only** inside `House`.
- If `House` is destroyed, `Room` goes with it.
- You **compose** objects to build complex ones.

---

## 📌 Visual Metaphor

| Relation     | Metaphor |
|--------------|----------|
| Association  | A person **uses** a bike. |
| Aggregation  | A university **has** students. If the university shuts down, students still exist. |
| Composition  | A body **has** a heart. If the body dies, the heart is no longer useful alone. |

---

## 🧠 Why It Matters

Understanding these distinctions helps you:
- **Design better data models**
- Avoid memory leaks and tight coupling
- Clearly define **ownership**, **responsibility**, and **object lifecycles**

---

## 🏠 Homework for Students

### 📌 Tasks:

1. **Association**:
   - Create `Customer` and `Order` classes where:
     - `Customer` has a method `placeOrder(Order order)`
   - Show that `Customer` and `Order` are independent.

2. **Aggregation**:
   - Create a `Library` class and a `Book` class.
   - A library has many books, but books can exist without a library.
   - Delete the library object — books should remain.

3. **Composition**:
   - Create a `Computer` class composed of `CPU` and `RAM` classes.
   - `CPU` and `RAM` should only be created within `Computer`'s constructor.
   - Prove they cannot exist independently.

---

Ready to move into **Static Keyword**, **Final Keyword**, or start OOP design patterns like **Factory Method** or **Singleton**?