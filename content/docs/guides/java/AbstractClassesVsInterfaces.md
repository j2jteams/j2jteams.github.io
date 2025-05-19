---
title: "Abstract Classes Vs Interfaces"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 9
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
Understanding the difference between **abstract classes** and **interfaces** is key to mastering **polymorphism**, **code extensibility**, and **design principles** like **"program to an interface"**.
---

### 🧠 The Big Picture

Both **abstract classes** and **interfaces** are used to achieve **abstraction** — hiding implementation details and exposing behavior. But they serve different purposes in Java’s object-oriented design.

| Feature | Abstract Class | Interface |
|--------|----------------|-----------|
| Inheritance | `extends` (single) | `implements` (multiple allowed) |
| Methods | Can have abstract and concrete methods | Only abstract methods (Java 7), also default/static methods (Java 8+) |
| Constructors | Yes | No |
| Fields | Can have variables with any access level | Only `public static final` constants |
| Access Modifiers | Can use all modifiers | Members are implicitly `public` |
| Purpose | "Is-a" relationship | "Can-do" or capability-based contracts |

---

## 🧪 Abstract Class: Example & Use Case

### 📦 Code Example

```java
abstract class Animal {
    String name;

    Animal(String name) {
        this.name = name;
    }

    abstract void makeSound(); // abstract method

    void sleep() {
        System.out.println(name + " is sleeping...");
    }
}

class Dog extends Animal {
    Dog(String name) {
        super(name);
    }

    @Override
    void makeSound() {
        System.out.println(name + " says: Woof!");
    }
}
```

### 🎯 When to Use Abstract Classes
- When you want to provide **shared implementation**.
- When there's a strong **"is-a" inheritance relationship**.
- When you want to include **state (fields)** and **constructors**.

---

## 🧪 Interface: Example & Use Case

### 📦 Code Example

```java
interface Flyable {
    void fly(); // implicitly public and abstract
}

interface Swimmable {
    void swim();
}

class Duck implements Flyable, Swimmable {
    @Override
    public void fly() {
        System.out.println("Duck is flying!");
    }

    @Override
    public void swim() {
        System.out.println("Duck is swimming!");
    }
}
```

### 🎯 When to Use Interfaces
- To define **capabilities** that can be shared across unrelated classes.
- When you need **multiple inheritance** of behavior.
- To program to **contracts**, allowing for **loose coupling**.

---

## 🔬 Under the Hood: What Changed After Java 8+

| Version | Interface Capability |
|--------|-----------------------|
| Java 7 | Only abstract methods |
| Java 8 | `default` and `static` methods allowed |
| Java 9+ | `private` methods inside interfaces also allowed |

---

### 🧠 Why Use One Over the Other?

#### Abstract Class = **partial abstraction + inheritance**
- Great when you need **common base functionality**, shared state, and constructors.

#### Interface = **pure abstraction + capability definition**
- Encourages **flexibility and loose coupling**.
- Supports **multiple inheritance** of type (one class can implement multiple interfaces).

---

## 🔄 Combined Use

Often, you'll see both used **together**:
- An **abstract class** defines shared base behavior.
- **Interfaces** define extra capabilities.

```java
abstract class Vehicle {
    abstract void start();
}

interface Electric {
    void charge();
}

class Tesla extends Vehicle implements Electric {
    void start() {
        System.out.println("Tesla is starting silently...");
    }

    public void charge() {
        System.out.println("Tesla is charging...");
    }
}
```

---

## 🏠 Homework for Students

### 📌 Task:

1. Create an **abstract class `Shape`** with:
   - An abstract method `area()`
   - A concrete method `display()` that prints “Calculating area…”

2. Create concrete subclasses:
   - `Circle` with a radius
   - `Rectangle` with length and width

3. Create an **interface `Colorable`** with:
   - A method `void setColor(String color)`

4. Implement `Colorable` in the subclasses and allow setting and displaying color.

### 📌 Deliverables:

- Demonstrate:
  - Abstract method override
  - Inherited method usage
  - Interface method implementation
  - Polymorphism by treating different shapes as `Shape` references

---

Ready to tackle **Static Keyword** or move into **Composition vs Inheritance** next?