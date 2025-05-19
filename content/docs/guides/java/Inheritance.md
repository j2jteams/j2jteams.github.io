---
title: "Inheritance"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 5
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
### 🧠 What is Inheritance?

**Inheritance** is a core object-oriented programming principle that allows one class (called a **subclass** or **child class**) to inherit the fields and methods of another class (called a **superclass** or **parent class**).

It enables the **reuse of code** and supports **hierarchical classification**.

---

### 🔍 Real-world Analogy:

Imagine a **Vehicle** class. A **Car**, **Bike**, and **Truck** are all vehicles — they share common properties like speed, engine, and methods like `start()` or `stop()`.

Instead of duplicating code in each class, we define these common features once in the `Vehicle` class and have `Car`, `Bike`, and `Truck` inherit from it.

---

### 🔧 Java Syntax Example

```java
// Parent Class
class Vehicle {
    int speed = 50;

    void start() {
        System.out.println("Vehicle is starting...");
    }
}

// Child Class
class Car extends Vehicle {
    void honk() {
        System.out.println("Car horn: Beep beep!");
    }
}

// Main Class
public class Main {
    public static void main(String[] args) {
        Car myCar = new Car();
        myCar.start();    // Inherited method
        myCar.honk();     // Child method
        System.out.println("Speed: " + myCar.speed);  // Inherited field
    }
}
```

---

### 🎯 Why Use Inheritance?

Inheritance isn't just about reusing code — it plays a **fundamental role in software architecture**.

#### ✅ Code Reusability
You avoid writing redundant code by placing shared logic in the parent class.

#### ✅ Extensibility
New classes can be built on existing ones, following the **Open/Closed Principle** (open for extension, closed for modification).

#### ✅ Polymorphism
Inheritance allows subclasses to override parent methods, enabling **dynamic behavior** at runtime.

#### ✅ Hierarchical Modeling
You can represent real-world relationships in a class hierarchy, which enhances **code readability and maintainability**.

---

### 🔥 Key Rules of Inheritance in Java

| Rule | Description |
|------|-------------|
| `extends` keyword | Used to inherit a class |
| Single inheritance | Java only supports single class inheritance (one parent) |
| Constructors | Not inherited, but `super()` can call parent constructor |
| Private members | Not directly accessible in child classes |
| Static members | Inherited but not overridden |

---

### ❗ Common Pitfalls

- **Overusing inheritance** can lead to rigid and tightly coupled systems.
- Prefer **composition** over inheritance when “has-a” makes more sense than “is-a”.

---

### 🏠 Homework for Students

**1. Create a class hierarchy** where:  
- `Employee` is the base class with name, id, and a method `work()`.  
- `Manager` extends Employee and adds a method `conductMeeting()`.  
- `Developer` extends Employee and adds a method `writeCode()`.

**2. Create objects of both child classes and demonstrate:**
- Inherited methods and fields.
- Child-specific behavior.
- Print outputs showing inheritance in action.

---

Would you like to proceed with **Access Modifiers** next, or go into **Method Overriding inside inheritance** (deeper dive)?