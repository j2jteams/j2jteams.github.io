---
title: "Generics"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 18
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Let’s dive into **Generics**, one of the most important and sometimes misunderstood features in Java.

Generics bring **type safety**, **code reusability**, and **readability** to Java — especially useful when dealing with **collections**, **APIs**, or building **frameworks**.

---

# 🧬 Java Generics – The Power of Type Safety

---

## 🔍 What Are Generics?

**Generics allow you to write code that works with any type, while maintaining compile-time type checking.**

Instead of writing different versions of the same code for different types (like `Integer`, `String`, etc.), you write it once using **type parameters** like `<T>`.

---

## ✅ Why Use Generics?

| Feature              | Benefit |
|----------------------|---------|
| Type safety          | Errors caught at compile-time (no ClassCastException at runtime) |
| Code reusability     | One method or class for multiple data types |
| Eliminate casting    | Avoid manual type conversion |
| Clean, maintainable  | Improves API and code readability |

✅ **Real-world:** Generics are used *everywhere* — in **Collections**, **Streams**, **custom libraries**, and **frameworks** like Spring and Hibernate.

---

## 📦 Without vs With Generics

### 🔴 Without Generics (Old style):

```java
List list = new ArrayList();
list.add("hello");
String str = (String) list.get(0); // manual cast
```

### ✅ With Generics:

```java
List<String> list = new ArrayList<>();
list.add("hello");
String str = list.get(0); // no cast needed ✅
```

---

## 🎯 Generic Class Example

```java
public class Box<T> {
    private T value;

    public void set(T value) { this.value = value; }
    public T get() { return value; }

    public static void main(String[] args) {
        Box<Integer> intBox = new Box<>();
        intBox.set(100);

        Box<String> strBox = new Box<>();
        strBox.set("Java");

        System.out.println(intBox.get()); // 100
        System.out.println(strBox.get()); // Java
    }
}
```

---

## 🎯 Generic Method Example

```java
public class Printer {
    public static <T> void print(T data) {
        System.out.println(data);
    }

    public static void main(String[] args) {
        print("Hello");
        print(123);
        print(3.14);
    }
}
```

- `<T>` before return type tells the compiler it's a **generic method**.

---

## 📌 Multiple Type Parameters

```java
public class Pair<K, V> {
    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key; this.value = value;
    }

    public K getKey() { return key; }
    public V getValue() { return value; }
}
```

---

## 📌 Bounded Type Parameters

Use `extends` to restrict what type can be used with a generic.

```java
public class MathUtils<T extends Number> {
    public double doubleValue(T num) {
        return num.doubleValue();
    }
}
```

Now you can only use `Integer`, `Double`, `Float`, etc.

---

## 📌 Wildcards (`?`, `extends`, `super`)

| Syntax                   | Meaning |
|--------------------------|---------|
| `<?>`                    | Unknown type |
| `<? extends Number>`     | Accepts `Number` or any subclass |
| `<? super Integer>`      | Accepts `Integer` or any superclass |

### 🔧 Example:

```java
public static void printList(List<?> list) {
    for (Object item : list) {
        System.out.println(item);
    }
}
```

Use wildcards when you're **reading**, not **modifying** the list.

---

## 🧠 Common Interview Q: Why no `List<T>[]`?

Java **doesn’t allow generic array creation** due to **type erasure**. You can’t do:
```java
List<String>[] list = new ArrayList<String>[10]; // Compile error ❌
```

But you can use:
```java
List<?>[] list = new ArrayList<?>[10]; // Warning suppressed
```

---

## 🧠 Summary Table

| Feature                  | Purpose |
|--------------------------|---------|
| `<T>`                    | Declare generic type |
| `<T extends Number>`     | Restrict to specific types |
| `List<?>`                | Accept any list |
| `<? extends T>`          | For reading from structure |
| `<? super T>`            | For writing to structure |
| Generics + Collections   | Type-safe data handling |

---

## 🏠 Homework for Students

### Task 1: Create a generic class `Container<T>` with methods:
- `void add(T item)`
- `T get(int index)`

Test it with both `Integer` and `String` types.

---

### Task 2: Write a method:
```java
<T extends Comparable<T>> T findMax(List<T> list)
```
Which returns the largest element in the list.

---

### Task 3: Use wildcard:
Write a method that takes a `List<?>` and prints all elements, regardless of type.

---

### Task 4: Create a generic `Pair<K, V>` class and print key-value pairs of type:
- `String, Integer`
- `String, String`

---

Would you like to move into **Lambda Expressions & Streams** next, which build beautifully on generics and are core to modern Java (Java 8+)?