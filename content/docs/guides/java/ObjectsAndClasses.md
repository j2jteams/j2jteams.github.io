---
title: "Objects and Classes"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 3
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
### 🔍 **What is a Class?**

In Java, a **class** is a user-defined data type that acts as a **blueprint for creating objects**. It defines:
- **State** (via fields/variables)
- **Behavior** (via methods/functions)

It encapsulates data for the object and methods to operate on that data.

---

### 🧱 **What is an Object?**

An **object** is a runtime instance of a class. It represents an actual entity in the real world with:
- Actual values (unlike the abstract nature of a class)
- Ability to invoke methods defined in the class

---

### ✅ **Example: Defining and Using a Class in Java**

We’ll model a real-world entity: `Car`

---

### 🔧 `Car.java` – Blueprint (Class)
```java
public class Car {
    // Fields (attributes)
    String color;
    int speed;

    // Method (behavior)
    void drive() {
        System.out.println("The " + color + " car is driving at " + speed + " km/h.");
    }
}
```

---

### 🚀 `Main.java` – Creating and Using Objects
```java
public class Main {
    public static void main(String[] args) {
        // Creating first object
        Car car1 = new Car();
        car1.color = "Red";
        car1.speed = 60;
        car1.drive();  // Output: The Red car is driving at 60 km/h.

        // Creating second object
        Car car2 = new Car();
        car2.color = "Blue";
        car2.speed = 90;
        car2.drive();  // Output: The Blue car is driving at 90 km/h.
    }
}
```

---

### 🧠 Key Concepts Explained

| Concept | Description |
|--------|-------------|
| `Car car1 = new Car();` | Instantiates a new object using the `Car` class |
| `car1.color = "Red";` | Sets the state of the object |
| `car1.drive();` | Invokes behavior defined in the class |

This approach adheres to **Object-Oriented Programming (OOP)** principles such as **encapsulation** and **modularity**.

---

## 🎯 Why This Matters

- Java is inherently object-oriented. Understanding classes and objects is **foundational** for mastering frameworks like **Spring**, **Hibernate**, and even for working with **design patterns** and **architecture**.
- In real-world projects, everything revolves around modeling real or abstract entities using classes.

---

## 📝 Homework for Students

### Task:
1. Create a `Student` class with:
   - `name` (String)
   - `age` (int)
   - A method `introduce()` that prints:  
     `"Hi, my name is Alex and I am 21 years old."`

2. In your `main` method:
   - Create **two Student objects** with different data
   - Call the `introduce()` method on both

---

✅ Let me know if you’d like to proceed to:
- **Constructors in Java**, or
- **Methods and Parameters** (deeper dive)?