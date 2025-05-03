---
title: "Adavance Exception Handling.md"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 16
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
Let’s go deeper and professional with **multi-catch**, **try-with-resources**, and **best practices** — these are must-knows if you're aiming to become a top-tier Java coach or preparing students for real-world and enterprise-grade coding.

---

## 1️⃣ Multi-Catch (`catch (Exception1 | Exception2 e)`)

### ✅ Purpose:
Handle **multiple exception types** with a **single catch block**, introduced in Java 7.

### ✅ Benefits:
- Cleaner code (no duplication)
- Easier maintenance

### 📦 Example:

```java
public class MultiCatchExample {
    public static void main(String[] args) {
        try {
            String s = null;
            System.out.println(s.length()); // NullPointerException
            int x = 5 / 0;                  // ArithmeticException
        } catch (NullPointerException | ArithmeticException e) {
            System.out.println("Caught an exception: " + e.getClass().getSimpleName());
        }
    }
}
```

⚠️ **Note:** The exception types must **not be parent-child** (like `IOException` and `FileNotFoundException`) because it causes ambiguity.

---

## 2️⃣ Try-With-Resources (Automatic Resource Management)

### ✅ Purpose:
Auto-close resources like files, sockets, DB connections without needing `finally`.

Introduced in **Java 7** and enhanced in **Java 9**.

### ✅ Works with classes that implement `AutoCloseable` or `Closeable`.

### 📦 Example:

```java
import java.io.*;

public class TryWithResources {
    public static void main(String[] args) {
        try (BufferedReader br = new BufferedReader(new FileReader("test.txt"))) {
            System.out.println(br.readLine());
        } catch (IOException e) {
            System.out.println("IO Error: " + e.getMessage());
        }
    }
}
```

### ✅ Java 9+ Enhancement

You can use an already declared variable (that is final or effectively final):

```java
BufferedReader br = new BufferedReader(new FileReader("test.txt"));
try (br) {
    System.out.println(br.readLine());
}
```

---

## 3️⃣ Enterprise-Level Exception Handling: Best Practices

Here’s what separates average developers from professionals:

---

### ✅ A. Don’t Swallow Exceptions

Bad:
```java
try {
    // risky code
} catch (Exception e) {
    // swallowed silently!
}
```

Good:
```java
catch (Exception e) {
    logger.error("Exception occurred", e);
    throw e; // or wrap and throw
}
```

---

### ✅ B. Wrap Checked Exceptions (if appropriate)

Use custom exceptions to **hide implementation details**.

```java
try {
    // risky code
} catch (IOException e) {
    throw new DataAccessException("Failed to load data", e);
}
```

---

### ✅ C. Always Log Before Rethrowing

```java
catch (SQLException e) {
    logger.error("DB failure on loadUser()", e);
    throw e;
}
```

---

### ✅ D. Prefer Specific Exceptions

```java
catch (NumberFormatException e) {
    // Better than generic catch
}
```

---

### ✅ E. Use Custom Business Exceptions

Make your code meaningful to the domain.

```java
class InsufficientBalanceException extends RuntimeException {
    public InsufficientBalanceException(String message) {
        super(message);
    }
}
```

---

### ✅ F. Use Exception Hierarchy in Custom Exceptions

```java
class ApplicationException extends Exception {}
class ServiceException extends ApplicationException {}
class DataAccessException extends ApplicationException {}
```

This makes it easy to handle at different layers (UI, Service, DAO).

---

### ✅ G. Avoid Overusing Checked Exceptions

Too many checked exceptions make APIs hard to use. Favor `RuntimeException` for programming errors.

---

## 🧠 Why This Matters

✅ In real-world apps:

- You often need to **log**, **rethrow**, or **convert** exceptions
- You deal with **IO, DB, and APIs** — exception handling is everywhere
- Clean exception handling reflects **clean system architecture**

---

## 🏠 Homework for Students (Advanced)

1. Create a file-reader method that uses **try-with-resources** to read and print a file line-by-line.

2. Write a program with a method `parseInt(String s)` that throws `NumberFormatException`, and use **multi-catch** to handle `NumberFormatException` and `NullPointerException`.

3. Create a layered architecture simulation:
   - `DAO`: throws `SQLException`
   - `Service`: catches it and throws `DataAccessException`
   - `Controller`: logs and shows friendly message

4. Create a custom exception hierarchy for a **banking app**:
   - `BankingException`
   - `InsufficientFundsException`
   - `InvalidAccountException`

---

Would you like to now dive into the **Object Class Methods** (`equals()`, `hashCode()`, `toString()`), or something like **Enums**, **Annotations**, or **Wrapper classes vs primitives** next?