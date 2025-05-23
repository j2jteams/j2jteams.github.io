---
title: "Enum And Annotations"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 17
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
Let’s explore two very powerful features in Java that often go underutilized by beginners but are essential in **enterprise Java**, frameworks like **Spring**, and **clean design**:  
✅ **Enums** (Enumerations)  
✅ **Annotations**

---

# 🔢 Java Enums (Enumerations)

---

## 🔍 What is an Enum?

An **enum** is a **special class that represents a group of constant values**.  
Instead of writing:
```java
int MONDAY = 1;
```
You write:
```java
enum Day { MONDAY, TUESDAY, ... }
```

This adds **type safety**, **readability**, and lets you bundle logic with constants.

---

## ✅ Why Use It?

- Enforces **fixed set of values**
- Avoids errors with invalid values (type-safe!)
- Allows behavior like a class (can have fields, methods, constructors!)
- Great for **state machines**, **commands**, **config**, etc.
- Used heavily in **Spring Boot**, **JPA**, and **REST APIs**

---

## 📦 Basic Enum Example

```java
public enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY;
}

public class TestEnum {
    public static void main(String[] args) {
        Day today = Day.MONDAY;

        switch (today) {
            case MONDAY:
                System.out.println("Start of the week");
                break;
            case FRIDAY:
                System.out.println("Weekend loading...");
                break;
        }
    }
}
```

---

## 🚀 Enum with Fields & Methods

```java
public enum Status {
    SUCCESS(200), ERROR(500);

    private final int code;

    Status(int code) {
        this.code = code;
    }

    public int getCode() {
        return code;
    }
}

public class Test {
    public static void main(String[] args) {
        System.out.println(Status.SUCCESS.getCode()); // 200
    }
}
```

You can even override methods per enum constant if needed.

---

## 🎯 Enterprise Use-Cases

- REST API: map `enum` to HTTP status or JSON value
- JPA: map enum fields to DB columns
- State Machines: manage state transitions (ORDER_STATUS)
- Avoid `String` or `int` constants everywhere

---

# 🧷 Java Annotations

---

## 🔍 What is an Annotation?

An **annotation is metadata** that provides information about your code.  
It **does not change logic** but gives instructions to:
- **Compiler**
- **Frameworks** (e.g., Spring, Hibernate)
- **Tools** (e.g., Lombok, Swagger)

Think of it like tagging a method or class with a special **label**.

---

## ✅ Why Use It?

- Reduces boilerplate
- Declarative programming
- Drives frameworks like Spring, JUnit, JPA
- Helps with validation, logging, DI, etc.
- Build **cleaner, more readable** code

---

## 📦 Built-in Annotations

| Annotation        | Use-case |
|-------------------|----------|
| `@Override`        | Ensures you’re overriding a method |
| `@Deprecated`      | Marks something as outdated |
| `@SuppressWarnings`| Suppresses compiler warnings |
| `@FunctionalInterface` | Enforces one-method functional interface |

### Example:
```java
@Override
public String toString() {
    return "MyObject";
}
```

---

## ⚙️ Custom Annotation

You can define your own annotation too:

```java
@interface MyAnnotation {
    String value();
    int priority() default 1;
}
```

Use it like this:

```java
@MyAnnotation(value = "Test case", priority = 3)
public void run() {}
```

---

## 🎯 Real World: Framework Annotations

| Annotation      | Where it's used | Purpose |
|------------------|-----------------|---------|
| `@Controller`, `@Service`, `@Repository` | Spring | Dependency Injection |
| `@RequestMapping`, `@GetMapping` | Spring MVC | Web routing |
| `@Entity`, `@Table`, `@Column` | Hibernate/JPA | ORM mappings |
| `@Test` | JUnit | Unit testing |
| `@Autowired` | Spring | Inject dependencies |

These drive almost everything in **enterprise Spring applications**.

---

## 🛠 Custom Annotation + Reflection Use-Case (Advanced)

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@interface LogExecutionTime {}

public class Demo {
    @LogExecutionTime
    public void serve() {
        System.out.println("Serving...");
    }
}
```

You can later scan methods using **Reflection API** and add custom behavior.

---

## 🧠 Summary

| Feature   | Enum                           | Annotation |
|-----------|--------------------------------|------------|
| Purpose   | Type-safe constant grouping     | Metadata, behavior-driven development |
| Usage     | API status, state, config       | Framework config, lifecycle, testing |
| Extends   | Can have methods, fields        | Cannot have behavior; just metadata |
| Real-World | HTTP codes, Roles, Days        | Spring, JPA, Logging, JUnit |

---

## 🏠 Homework

### 1. Create an `enum` named `Role` with constants: `ADMIN`, `USER`, `GUEST`.  
- Add a method `getPermissionLevel()` that returns `int` values for each role.

### 2. Create a custom annotation `@Info` with fields: `author` and `version`.  
- Apply it on a class and print its values using reflection.

### 3. Use built-in annotations:
- Use `@Override` on a method
- Use `@Deprecated` on an old method and call it

---

Would you like to move onto **Object Class Methods** (`equals`, `hashCode`, `toString`), or go into **Wrapper Classes**, **Generics**, or even **Lambdas & Streams** next?