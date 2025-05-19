---
title: "Exception Handling"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 15
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
**Exception Handling** is a core part of writing robust and production-grade Java applications. It's how Java lets you deal with unexpected events like network failures, file-not-found errors, or division-by-zero — without crashing the program.

---

## 🚧 What is an Exception?

An **exception** is an **event that disrupts the normal flow** of a program. It typically occurs when something goes wrong during execution, like:

- Dividing by zero
- Accessing an invalid index
- Opening a missing file

In Java, **exceptions are objects** that can be caught and handled using the exception handling mechanism.

---

## 📊 Java Exception Hierarchy

```plaintext
Object
 └── Throwable
      ├── Error         (Serious problems: OutOfMemoryError)
      └── Exception     (Things we can handle)
           ├── RuntimeException (Unchecked)
           └── Other Exceptions (Checked)
```

---

## 📂 Types of Exceptions

| Type             | Examples                          | Need to Handle? |
|------------------|-----------------------------------|-----------------|
| **Checked**      | FileNotFoundException, IOException| YES (must catch or declare) |
| **Unchecked**    | NullPointerException, ArithmeticException | NO (optional to catch) |
| **Error**        | OutOfMemoryError, StackOverflowError | NO (fatal, shouldn't handle) |

---

## 🧰 Java Exception Handling Keywords

| Keyword   | Purpose |
|-----------|---------|
| `try`     | Wrap code that might throw an exception |
| `catch`   | Handle specific exception |
| `finally` | Always executes (used for cleanup) |
| `throw`   | Manually throw an exception |
| `throws`  | Declare method may throw an exception |

---

## 🧪 Basic Try-Catch Example

```java
public class Main {
    public static void main(String[] args) {
        try {
            int result = 10 / 0; // ArithmeticException
        } catch (ArithmeticException e) {
            System.out.println("Cannot divide by zero: " + e);
        }
    }
}
```

---

## 🔁 `finally` Block — Always Runs

```java
try {
    int data = 10 / 2;
} catch (Exception e) {
    System.out.println("Exception caught");
} finally {
    System.out.println("Cleanup code in finally block");
}
```

Even if an exception occurs or not — `finally` will run.

---

## 🚀 `throw` and `throws`

### `throw`: Used to throw an exception manually.

```java
throw new IllegalArgumentException("Invalid argument");
```

### `throws`: Declares an exception may occur.

```java
void readFile() throws IOException {
    FileReader fr = new FileReader("file.txt");
}
```

---

## 💡 Custom Exception Class

You can define your own exception by extending `Exception` or `RuntimeException`.

```java
class InvalidAgeException extends Exception {
    public InvalidAgeException(String msg) {
        super(msg);
    }
}

class Test {
    void checkAge(int age) throws InvalidAgeException {
        if (age < 18) throw new InvalidAgeException("Not eligible to vote");
    }
}
```

---

## 🧩 Why Use It?

Exception handling embodies **OOP design**:

- Promotes **separation of concerns** (normal logic vs error handling)
- Encourages **clean, readable code**
- Implements **polymorphism** (catch different exceptions via inheritance)
- Supports **custom behavior** via user-defined exceptions

Without exception handling, you'd be left with scattered `if/else` blocks and hard-to-maintain code.

---

## 🛡 Best Practices

- Catch **specific exceptions** first, then general ones.
- Use `finally` for **resource cleanup** (e.g., closing files, DB connections).
- Don't suppress exceptions silently (`catch (Exception e) {}`).
- Avoid catching `Throwable` unless you're writing a framework.

---

## 🏠 Homework for Students

### 📌 Tasks:

1. Write a method `divide(int a, int b)` that:
   - Throws `ArithmeticException` if `b == 0`
   - Uses `try-catch` to handle it and return safe result

2. Create a method `readFile(String path)` that:
   - Throws `FileNotFoundException`
   - Declare it using `throws`
   - Handle it in the calling method

3. Create a custom exception `InvalidScoreException`
   - If score < 0 or > 100, throw this exception
   - Test it in a grading system

4. Demonstrate `finally` block with a simple division operation

---

Would you like to go deeper into **multi-catch**, **try-with-resources**, and **best practices in enterprise-level exception handling** next? Or should we move into the next core topic like **Object Class Methods** (`equals`, `hashCode`, `toString`)?