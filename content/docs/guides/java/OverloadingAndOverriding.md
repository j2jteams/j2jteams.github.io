---
title: "Method Overloading vs Method Overriding"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 6
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
### 🔁 Method Overloading
Having multiple methods with the same name but **different parameters** (type/number/order).

```java
class Calculator {
    int add(int a, int b) { return a + b; }
    int add(int a, int b, int c) { return a + b + c; }
    double add(double a, double b) { return a + b; }
}
```

🧠 **Why use it?**
- It is a direct implementation of **compile-time polymorphism**.
- Keeps the code **cohesive** by logically grouping related operations under one method name.
- Improves **readability and usability** — clients don’t have to remember different method names.
- Enables **type-safe flexibility** for different argument combinations.

> ✅ Overloading enhances flexibility and maintains a clean API without sacrificing type safety — a must for robust method design.

---

### 🔄 Method Overriding
Subclass provides a **new implementation** of a method inherited from its superclass.

```java
class Animal {
    void sound() {
        System.out.println("Generic sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

🧠 **Why use it?**
- It is the foundation of **runtime polymorphism** — actual method execution depends on the object type.
- Enables subclasses to **customize behavior** while keeping the interface consistent.
- Supports **extensibility** — you can introduce new behaviors in derived classes without touching base code.
- Aligns with the **Liskov Substitution Principle**, promoting interchangeable components.

> ✅ Overriding allows dynamic behavior changes, making code more adaptable and maintainable in large-scale systems.

Sure! Here's a well-structured **homework assignment** for **Method Overloading vs Method Overriding**. It encourages hands-on practice and reinforces conceptual clarity through comparison.

---

## 📝 Homework: Method Overloading vs Method Overriding

### 🎯 Objective:
To help students understand the difference between **method overloading** (compile-time polymorphism) and **method overriding** (runtime polymorphism) by implementing both in practical Java classes.

---

### 📌 Tasks

### 🧪 Part 1: Method Overloading

1. Create a class named `MathUtils` with the following overloaded methods:
   - `int multiply(int a, int b)`
   - `double multiply(double a, double b)`
   - `int multiply(int a, int b, int c)`

2. In the `main()` method, call each `multiply` method with appropriate parameters and print the results.

3. Write a brief comment in your code explaining why overloading improves API design and user experience.

---

### 🧪 Part 2: Method Overriding

1. Create a parent class named `Shape` with a method:
   ```java
   void draw() {
       System.out.println("Drawing a shape...");
   }
   ```

2. Create two child classes:
   - `Circle` that overrides `draw()` to print `"Drawing a circle..."`.
   - `Square` that overrides `draw()` to print `"Drawing a square..."`.

3. In the `main()` method:
   - Declare a `Shape` reference and assign it to `new Circle()` and call `draw()`.
   - Repeat with `new Square()`.

4. Add a comment explaining **how and why overriding supports runtime polymorphism**.

---

### 💬 Bonus Discussion (Optional):
- What would happen if the `@Override` annotation is removed?
- Can we overload methods by only changing the return type? Why or why not?

---

### ✅ Submission Requirements:
- Java file with all classes and `main()` method.
- Clear, well-commented code explaining both overloading and overriding.
- Your explanation of **why these concepts are essential to polymorphism**.

---

Let me know if you’d like me to generate a sample solution for this homework too!
---

Would you like me to move to the next core Java topic? Options:
- Access Modifiers
- Static Keyword
- Interfaces and Abstract Classes
- Final Keyword