---
title: "Just Hibernate"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 303
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
Here’s a complete **step-by-step guide** for setting up our **Student Management Application** using **Hibernate with H2 (In-Memory Database)** from scratch.  

---

# **🚀 Step-by-Step Guide: Hibernate with H2**
We will set up **Hibernate with an H2 in-memory database** it helps to quickly test and understand Hibernate without needing an external database.

---

## **🔹 Step 1: Add Dependencies in `pom.xml`**
Modify your `pom.xml` to include **Hibernate and H2 database** dependencies.

```xml
<dependencies>
    <!-- Hibernate Core -->
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-core</artifactId>
        <version>6.3.1.Final</version>
    </dependency>

    <!-- H2 Database (In-Memory) -->
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <version>2.1.214</version>
        <scope>runtime</scope>
    </dependency>

    <!-- Hibernate Annotations -->
    <dependency>
        <groupId>org.hibernate.common</groupId>
        <artifactId>hibernate-commons-annotations</artifactId>
        <version>6.0.6.Final</version>
    </dependency>

    <!-- Logging Dependencies -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>2.0.7</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-simple</artifactId>
        <version>2.0.7</version>
    </dependency>
</dependencies>
```

✅ **Why H2?**
- **In-memory database** → No need for MySQL.
- **Resets on every restart** → Perfect for testing.
- **Supports SQL similar to MySQL**.

---

## **🔹 Step 2: Configure Hibernate (`hibernate.cfg.xml`)**
Modify `hibernate.cfg.xml` to use **H2 instead of MySQL**.

```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
    "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
    "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">

<hibernate-configuration>
    <session-factory>
        <!-- H2 Database Connection -->
        <property name="hibernate.connection.driver_class">org.h2.Driver</property>
        <property name="hibernate.connection.url">jdbc:h2:mem:testdb</property>
        <property name="hibernate.connection.username">sa</property>
        <property name="hibernate.connection.password"></property>

        <!-- Hibernate Dialect for H2 -->
        <property name="hibernate.dialect">org.hibernate.dialect.H2Dialect</property>

        <!-- Show SQL Queries in Console -->
        <property name="hibernate.show_sql">true</property>

        <!-- Automatically create tables -->
        <property name="hibernate.hbm2ddl.auto">update</property>

        <!-- Entity Mapping -->
        <mapping class="com.example.model.Student"/>
    </session-factory>
</hibernate-configuration>
```

✅ **Key Changes**
- `jdbc:h2:mem:testdb` → Uses **H2 in-memory database**.
- `hibernate.dialect` → Uses **H2Dialect**.
- `hibernate.hbm2ddl.auto=update` → Automatically creates tables.

---

## **🔹 Step 3: Create `Student` Entity**
📌 **`Student.java` (Entity class)**
```java
package com.example.model;

import jakarta.persistence.*;

@Entity
@Table(name = "students")
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    private String firstName;
    private String lastName;
    private String email;

    public Student() {}

    public Student(String firstName, String lastName, String email) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.email = email;
    }

    // Getters and Setters
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }
    public String getFirstName() { return firstName; }
    public void setFirstName(String firstName) { this.firstName = firstName; }
    public String getLastName() { return lastName; }
    public void setLastName(String lastName) { this.lastName = lastName; }
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }

    @Override
    public String toString() {
        return "Student{id=" + id + ", firstName='" + firstName + "', lastName='" + lastName + "', email='" + email + "'}";
    }
}
```

✅ **Explanation**
- `@Entity` → Marks this as a Hibernate entity.
- `@Table(name = "students")` → Maps the class to `students` table.
- `@Id` → Primary Key.
- `@GeneratedValue(strategy = GenerationType.IDENTITY)` → Auto-increment ID.

---

## **🔹 Step 4: Create Hibernate Utility Class**
📌 **`HibernateUtil.java`**
```java
package com.example.util;

import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class HibernateUtil {

    private static final SessionFactory sessionFactory = buildSessionFactory();

    private static SessionFactory buildSessionFactory() {
        try {
            return new Configuration().configure().buildSessionFactory();
        } catch (Throwable ex) {
            System.err.println("SessionFactory creation failed: " + ex);
            throw new ExceptionInInitializerError(ex);
        }
    }

    public static SessionFactory getSessionFactory() {
        return sessionFactory;
    }

    public static void shutdown() {
        getSessionFactory().close();
    }
}
```

✅ **Why?**
- **Manages Hibernate `SessionFactory`**.
- Provides a **single instance** for managing sessions.

---

## **🔹 Step 5: Verify Connection**
📌 **Create `Main.java`**
```java
package com.example;

import com.example.model.Student;
import com.example.util.HibernateUtil;
import org.hibernate.Session;
import org.hibernate.Transaction;

public class Main {
    public static void main(String[] args) {
        // Open Hibernate Session
        Session session = HibernateUtil.getSessionFactory().openSession();

        // Begin Transaction
        Transaction transaction = session.beginTransaction();

        // Create a Student Object
        Student student = new Student("John", "Doe", "john.doe@example.com");

        // Save the Student Object
        session.save(student);

        // Commit Transaction
        transaction.commit();

        // Close Session
        session.close();

        System.out.println("Student saved successfully!");
    }
}
```

✅ **Expected Output**
```
Hibernate: create table students (id integer not null auto_increment, email varchar(255), first_name varchar(255), last_name varchar(255), primary key (id))
Hibernate: insert into students (email, first_name, last_name) values (?, ?, ?)
Student saved successfully!
```

---