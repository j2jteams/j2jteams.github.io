---
title: "AccessModifiers"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 8
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
Let’s break down **Access Modifiers** in Java — these are key to **encapsulation** and **security** in object-oriented programming.

---
### 🧠 What Are Access Modifiers?

**Access modifiers** control **where** (from which classes/packages) a class, method, or variable can be accessed. They are used to **enforce encapsulation**, **protect internal details**, and guide API usage.

In Java, there are **four access modifiers**:

| Modifier     | Class | Package | Subclass | World |
|--------------|:-----:|:-------:|:--------:|:-----:|
| `public`     | ✅    | ✅      | ✅       | ✅    |
| `protected`  | ✅    | ✅      | ✅       | ❌    |
| *(default)*  | ✅    | ✅      | ❌       | ❌    |
| `private`    | ✅    | ❌      | ❌       | ❌    |

---

### 🔍 Why Use Access Modifiers?

Access modifiers **enforce boundaries** between components of a system. Here's what they help achieve:

- ✅ **Encapsulation**: Hide internal implementation details.
- ✅ **Security**: Restrict unauthorized access.
- ✅ **Clean API Design**: Clearly define what should and shouldn't be accessed from outside.

---

### 📦 The Four Access Modifiers Explained

#### 1. `public`
- **Accessible from anywhere.**
- Used for APIs and interfaces that are intended for widespread use.

```java
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
}
```

#### 2. *(default)* — package-private
- **No modifier** means it's accessible **only within the same package**.
- Useful when you want to expose functionality internally but hide it externally.

```java
class PackageHelper {
    void help() {
        System.out.println("Helping within the package.");
    }
}
```

#### 3. `protected`
- Accessible within the **same package** and in **subclasses**, even if they're in a different package.
- Often used in class hierarchies.

```java
public class Animal {
    protected void makeSound() {
        System.out.println("Animal makes a sound");
    }
}
```

#### 4. `private`
- **Only accessible within the same class.**
- Ideal for encapsulating data and enforcing access through getter/setter methods.

```java
public class Person {
    private String name;

    public void setName(String n) {
        name = n;
    }

    public String getName() {
        return name;
    }
}
```

---

### 🧠 Access Modifier vs OOP Principles

| Modifier | OOP Principle | Explanation |
|----------|----------------|-------------|
| `private` | Encapsulation | Hide internal state from the outside world. |
| `protected` | Inheritance | Allow subclasses to use or override specific behavior. |
| `public` | Abstraction / API Design | Expose only necessary behaviors to users. |

---

### 🚫 Common Mistakes

- Making everything `public` just to "make it work".
- Forgetting to set access modifiers on classes (default might not be what you want).
- Using `protected` in places where encapsulation is more appropriate.

---

### 🏠 Homework for Students

1. Create a class `BankAccount` with the following:
   - A `private` balance field.
   - `public` methods `deposit()` and `getBalance()`.
   - A `default` method that prints account status.
   - A `protected` method `calculateInterest()`.

2. Create a class in the **same package** and access all members.

3. Create a **subclass in a different package** and try accessing:
   - `protected` method
   - `default` method
   - `private` field

4. Observe:
   - Which members are accessible and which aren't?
   - Try changing access modifiers to see the effects.

---

Would you like to proceed to **Static Keyword**, **Final Keyword**, or dive into **Abstract Classes vs Interfaces** next?