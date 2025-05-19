---
title: "Abstraction, Encapsulation, Inheritance, and Polymorphism in Java"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 2
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Let's cover the **4 Pillars of Object-Oriented Programming (OOP)** in Java. These are fundamental concepts every Java developer must understand deeply, especially for interviews, design patterns, and building scalable systems.

# 🧱 1. Abstraction

### ✅ Definition:
Abstraction is the process of **hiding internal implementation** and **showing only the necessary details** to the user.

Think of it like **driving a car** — you use the steering wheel and pedals without needing to understand how the engine works.

### 🧠 Why it's important:
Defines a contract without worrying about implementation.

Promotes interface-based programming.

Decouples high-level logic from low-level implementations.

### ✅ Java Implementation:
In Java, abstraction is achieved using:
- **Abstract classes**
- **Interfaces**

---

### 🔧 Example: Using an Interface
```java
interface Animal {
    void makeSound();  // abstract method
}

class Dog implements Animal {
    public void makeSound() {
        System.out.println("Dog barks");
    }
}

class Cat implements Animal {
    public void makeSound() {
        System.out.println("Cat meows");
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Animal a = new Dog();  // Abstraction in action
        a.makeSound();         // Output: Dog barks
    }
}
```

---

# 🔒 2. Encapsulation

### ✅ Definition:
Encapsulation is the process of **binding data (fields)** and **methods (behaviors)** into a single unit and **restricting direct access** to some components.

This is typically done using:
- **Private fields**
- **Public getters/setters**

### 🧠 Why it's important:
- Improves security
- Ensures control over data
- Makes code easier to maintain

---

### 🔧 Example:
```java
public class BankAccount {
    private double balance;  // can't be accessed directly

    public void deposit(double amount) {
        if(amount > 0)
            balance += amount;
    }

    public double getBalance() {
        return balance;
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        BankAccount acc = new BankAccount();
        acc.deposit(500);
        System.out.println("Balance: " + acc.getBalance());
    }
}
```

---

# 👨‍👩‍👧 3. Inheritance

### ✅ Definition:
Inheritance allows one class to **inherit properties and methods** from another class. It promotes **code reuse** and enables **hierarchical classification**.

- `extends` keyword is used for class inheritance.

### 🧠 Why it's important:
- Promotes DRY (Don't Repeat Yourself)
- Simplifies code structure
- Helps model real-world hierarchies

---

### 🔧 Example:
```java
class Animal {
    void eat() {
        System.out.println("This animal eats food.");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Dog barks.");
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.eat();  // Inherited method
        d.bark(); // Own method
    }
}
```

---

# 🔁 4. Polymorphism

### ✅ Definition:
Polymorphism means **"many forms"**. In Java, it allows **one interface to be used for a general class of actions**, with specific behavior depending on the object.

Types:
- **Compile-time polymorphism (method overloading)**
- **Runtime polymorphism (method overriding)**

---

### 🔧 Example of Runtime Polymorphism:
```java
class Animal {
    void makeSound() {
        System.out.println("Some sound...");
    }
}

class Dog extends Animal {
    void makeSound() {
        System.out.println("Dog barks");
    }
}

class Cat extends Animal {
    void makeSound() {
        System.out.println("Cat meows");
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Animal a;

        a = new Dog();
        a.makeSound();  // Output: Dog barks

        a = new Cat();
        a.makeSound();  // Output: Cat meows
    }
}
```

---

## 📝 Summary Table

| Concept       | Meaning                                         | Achieved Using               |
|---------------|--------------------------------------------------|------------------------------|
| **Abstraction** | Hides complex details; shows essentials         | Abstract class / Interface   |
| **Encapsulation** | Hides data using access modifiers             | Private fields + Getters/Setters |
| **Inheritance** | One class inherits from another                | `extends` keyword            |
| **Polymorphism** | One name, many forms                           | Overriding / Overloading     |

---

## 🎓 Homework for Students

1. **Create an abstract class** `Shape` with an abstract method `area()`.  
   - Implement `Rectangle` and `Circle` classes.
   - Each should compute and print its area.

2. **Create a class `Employee`** with private fields `name`, `salary`.  
   - Provide getter/setter methods.
   - Create two objects and display their data.

3. **Implement runtime polymorphism** using a base class `Vehicle` and two subclasses `Bike` and `Car`.  
   - Call the overridden method using a parent reference.

---
