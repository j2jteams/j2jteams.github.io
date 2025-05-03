---
title: "Constructors and the `this` Keyword"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 4
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
These concepts are crucial for object creation and understanding how to manage object state properly.

---

## 🏗️ 1. **What is a Constructor?**

### ✅ Definition:
A **constructor** in Java is a **special method** that is automatically called **when an object is created**. It is used to **initialize objects**.

### 🔧 Syntax:
```java
ClassName() {
    // initialization code
}
```

### 🔍 Key Rules:
- Constructor **has no return type** (not even `void`)
- The **name of the constructor must match the class name**
- It can be **parameterized** (to initialize values) or **default** (no parameters)

---

### ✅ Example: Default Constructor
```java
public class Car {
    String brand;

    // Default constructor
    Car() {
        brand = "Tesla";
    }

    void showBrand() {
        System.out.println("Brand: " + brand);
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Car c = new Car();   // constructor called automatically
        c.showBrand();       // Output: Brand: Tesla
    }
}
```

---

### ✅ Example: Parameterized Constructor
```java
public class Car {
    String brand;
    int speed;

    // Parameterized constructor
    Car(String brand, int speed) {
        this.brand = brand;
        this.speed = speed;
    }

    void showDetails() {
        System.out.println("Brand: " + brand + ", Speed: " + speed + " km/h");
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Car car1 = new Car("Toyota", 120);
        Car car2 = new Car("BMW", 180);

        car1.showDetails(); // Output: Brand: Toyota, Speed: 120 km/h
        car2.showDetails(); // Output: Brand: BMW, Speed: 180 km/h
    }
}
```

---

## 🔁 2. **The `this` Keyword**

### ✅ Definition:
`this` is a reference **to the current object** inside a class. It is used to:
- Refer to **current object’s fields**
- Invoke **current class methods or constructors**
- Pass the current object as a parameter

### 🧠 Why use `this`?
When **parameter names and field names are the same**, `this` helps distinguish between them.

---

### 🔧 Example:
```java
public class Student {
    String name;
    int age;

    Student(String name, int age) {
        this.name = name; // distinguishes between parameter and instance variable
        this.age = age;
    }

    void show() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Student s1 = new Student("Alice", 22);
        s1.show();  // Output: Name: Alice, Age: 22
    }
}
```

---

## 🔄 Constructor Overloading

Just like method overloading, Java allows **multiple constructors** in the same class with different parameters.

### 🔧 Example:
```java
public class Book {
    String title;
    int pages;

    // Default constructor
    Book() {
        this.title = "Untitled";
        this.pages = 0;
    }

    // Parameterized constructor
    Book(String title, int pages) {
        this.title = title;
        this.pages = pages;
    }

    void info() {
        System.out.println("Title: " + title + ", Pages: " + pages);
    }
}
```

---

## 🧠 When to Use `this()` Constructor Call
You can use `this()` to call another constructor **inside the same class**.

```java
public class Book {
    String title;
    int pages;

    Book() {
        this("Unknown", 0);  // Calls the parameterized constructor
    }

    Book(String title, int pages) {
        this.title = title;
        this.pages = pages;
    }

    void info() {
        System.out.println("Title: " + title + ", Pages: " + pages);
    }
}
```

---

## 📝 Summary Table

| Concept             | Use                                  |
|---------------------|---------------------------------------|
| Constructor         | Initializes objects                   |
| Default Constructor | No parameters, sets default values    |
| Parameterized       | Sets values via arguments             |
| Constructor Overloading | Multiple ways to initialize        |
| `this` keyword      | Refers to current object or constructor |

---

## 🎓 Homework for Students

1. **Create a `Laptop` class** with fields:
   - `brand` (String)
   - `ram` (int)
   - Constructors:
     - Default constructor → sets brand = "Dell", ram = 8
     - Parameterized constructor → sets custom values
   - Method: `display()` to print details

2. **Create 2 objects** in `main`:
   - One using the default constructor
   - One using the parameterized constructor

3. Bonus: Use `this()` to call one constructor from the other.

---

Would you like the next topic to be:
- **Static Keyword & Static Blocks**
- **Method Overloading vs Overriding**
- **Access Modifiers (public, private, etc.)**

You’re steering the ship, coach 🚢👨‍🏫