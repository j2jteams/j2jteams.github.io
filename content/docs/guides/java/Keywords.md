---
title: "Keywords"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 14
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
Java has **50+ reserved keywords** (as of Java 17). These are **reserved by the language** and cannot be used as identifiers (variable names, class names, etc.).

We'll break them into **logical groups** and explain **purpose, usage, and nuances** of each with code examples.

---

## 📦 1. **Data Types & Literals**
These define the type of data.

| Keyword       | Meaning |
|---------------|---------|
| `int`         | 32-bit integer |
| `long`        | 64-bit integer |
| `short`       | 16-bit integer |
| `byte`        | 8-bit integer |
| `float`       | 32-bit floating point |
| `double`      | 64-bit floating point |
| `char`        | 16-bit Unicode character |
| `boolean`     | true or false |
| `true`, `false`, `null` | literals |

🔸 `null` is **not** a keyword but a **reserved literal** in Java.

---

## 🧱 2. **Access Modifiers**
These control visibility.

| Keyword       | Purpose |
|---------------|---------|
| `public`      | Visible everywhere |
| `private`     | Visible only in the same class |
| `protected`   | Visible in the same package + subclasses |
| `default`     | (No keyword) Package-private |

---

## 🏗 3. **Class & Object Related**
| Keyword       | Purpose |
|---------------|---------|
| `class`       | Defines a class |
| `interface`   | Declares an interface |
| `enum`        | Declares an enumeration |
| `extends`     | Inherits from a class |
| `implements`  | Implements an interface |
| `new`         | Creates an object |
| `this`        | Refers to current object |
| `super`       | Refers to parent class |

---

## 🔁 4. **Control Flow**
| Keyword       | Purpose |
|---------------|---------|
| `if` / `else` | Branching |
| `switch`, `case`, `default` | Multi-way branching |
| `while`, `do`, `for` | Loops |
| `break`, `continue` | Control loop execution |
| `return`      | Exit method |
| `yield`       | Return value from a switch expression (Java 14+) |

---

## ⚙️ 5. **Exception Handling**
| Keyword       | Purpose |
|---------------|---------|
| `try`         | Start of exception block |
| `catch`       | Handle the exception |
| `finally`     | Always executes |
| `throw`       | Used to throw exception |
| `throws`      | Declares possible exceptions |

---

## 🔐 6. **Object Behavior Modifiers**
| Keyword       | Purpose |
|---------------|---------|
| `static`      | Belongs to the class, not instance |
| `final`       | Cannot be changed/overridden |
| `abstract`    | Declares incomplete method/class |
| `synchronized`| Used in multithreading to control access |
| `volatile`    | Prevents thread caching for variables |
| `transient`   | Skips variable during serialization |
| `native`      | Calls code written in another language (C/C++) |
| `strictfp`    | Ensures consistent floating-point calculations across platforms |

---

## 🧬 7. **Inheritance and Polymorphism**
| Keyword       | Purpose |
|---------------|---------|
| `extends`     | Subclassing (class inheritance) |
| `implements`  | Interface implementation |
| `instanceof`  | Checks runtime type |
| `super`       | Access parent class methods/constructors |

---

## 👨‍👩‍👧 8. **Concurrency (Threads)**
| Keyword       | Purpose |
|---------------|---------|
| `synchronized`| Ensures atomic access to code block |
| `volatile`    | Guarantees visibility of changes across threads |

---

## 📦 9. **Package and Import**
| Keyword       | Purpose |
|---------------|---------|
| `package`     | Declares package name |
| `import`      | Imports other packages/classes |

---

## 📐 10. **Miscellaneous**
| Keyword       | Purpose |
|---------------|---------|
| `assert`      | Debugging tool for assumptions |
| `instanceof`  | Check if object is of a specific type |
| `goto`        | Reserved but **not used** in Java |
| `const`       | Reserved but **not used** (use `final` instead) |

---

## 🆕 Bonus: **New Keywords in Recent Java Versions**
| Keyword       | Purpose |
|---------------|---------|
| `var`         | Local variable type inference (Java 10+) |
| `record`      | Immutable data class (Java 14+) |
| `sealed`, `permits`, `non-sealed` | Restricted class hierarchy (Java 15+) |

---