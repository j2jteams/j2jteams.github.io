---
title: "This, Super and other Keywords"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 13
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
Let's now focus on **important Java keywords** that give you fine-grained control over object behavior and class design. These are subtle, powerful, and **interview-favorite** topics too.

---
We'll cover:

- `this`
- `super`
- `instanceof`
- `return`
- `break` / `continue` (control flow)
- `new`
- `null`

---

## 1️⃣ `this` Keyword — Refers to the Current Object

### ✅ Purpose:
- To distinguish between **instance variables and parameters** when they have the same name.
- To call **another constructor** from a constructor.
- To pass current object as a reference.

### 📦 Example:

```java
class Student {
    String name;

    Student(String name) {
        this.name = name; // "this" refers to current object
    }

    void display() {
        System.out.println("Name: " + this.name);
    }
}
```

### 🔁 Constructor Chaining:

```java
class Student {
    String name;
    int age;

    Student() {
        this("Unknown", 0); // Calls another constructor
    }

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

---

## 2️⃣ `super` Keyword — Refers to Parent Class

### ✅ Purpose:
- To call **superclass constructor**.
- To access **superclass methods or variables** that are overridden.

### 📦 Example:

```java
class Animal {
    void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {
    void sound() {
        super.sound(); // Calls Animal's sound()
        System.out.println("Dog barks");
    }
}
```

### 🏗 Super Constructor Call:

```java
class Animal {
    Animal(String type) {
        System.out.println("Animal: " + type);
    }
}

class Dog extends Animal {
    Dog() {
        super("Mammal"); // Calls Animal's constructor
        System.out.println("Dog created");
    }
}
```

---

## 3️⃣ `instanceof` — Type Check

### ✅ Purpose:
Used to check **object type at runtime**.

```java
class Animal {}
class Dog extends Animal {}

public class Main {
    public static void main(String[] args) {
        Animal a = new Dog();
        System.out.println(a instanceof Dog); // true
        System.out.println(a instanceof Animal); // true
    }
}
```

---

## 4️⃣ `return` — Exits from Method

### ✅ Purpose:
Returns a value from a method or exits early.

```java
int square(int x) {
    return x * x;
}
```

```java
void printEven(int x) {
    if (x % 2 != 0) return; // Exit if not even
    System.out.println(x + " is even");
}
```

---

## 5️⃣ `break` and `continue` — Control Loop Execution

### 🔄 `break`: Exit the loop immediately  
### 🔄 `continue`: Skip the current iteration

```java
for (int i = 0; i < 5; i++) {
    if (i == 3) break;
    System.out.println(i);  // Output: 0 1 2
}

for (int i = 0; i < 5; i++) {
    if (i == 3) continue;
    System.out.println(i);  // Output: 0 1 2 4
}
```

---

## 6️⃣ `new` Keyword — Object Creation

### ✅ Purpose:
Creates a new instance (object) of a class.

```java
Student s1 = new Student("Alex");
```

This allocates memory and calls the constructor.

---

## 7️⃣ `null` Keyword — Represents Absence of Object

### ✅ Purpose:
`null` means a reference variable doesn’t point to any object.

```java
String name = null;

if (name == null) {
    System.out.println("Name is not assigned");
}
```

🛑 Be careful: calling methods on a `null` reference leads to a **NullPointerException**.

---

## 🎓 Summary Table

| Keyword | Purpose |
|---------|---------|
| `this` | Refers to current object |
| `super` | Refers to parent class |
| `instanceof` | Checks object type |
| `return` | Exits method or returns a value |
| `break` | Exits loop/switch |
| `continue` | Skips current iteration |
| `new` | Creates new object |
| `null` | Indicates no object reference |

---

## 🏠 Homework for Students

### 📌 Tasks:

1. Create a class `Employee` with:
   - `String name`, `int id`
   - Use `this` to resolve name conflict in constructor.
   - Create another constructor that chains using `this(...)`

2. Create class `Vehicle` and subclass `Bike`:
   - Override method `run()`, but call parent version using `super`.

3. Create a class `Shape` and subclass `Circle`.
   - Check with `instanceof` whether a `Shape` reference points to a `Circle`.

4. Write a loop from 1 to 10:
   - Use `break` when the number is 5.
   - Use `continue` to skip even numbers.

---

Would you like to now dive into **Exception Handling**, or explore **Object Class Methods** like `equals()`, `hashCode()`, `toString()` next?