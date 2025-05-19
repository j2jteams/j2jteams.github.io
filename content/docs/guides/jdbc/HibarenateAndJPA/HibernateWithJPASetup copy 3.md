---
title: "Using @Named Queries"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 404
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
Great! Now, let’s explore **`@NamedQuery`** in Spring Data JPA, which allows defining **predefined queries** directly in the entity class.

---

# **🎯 Learning Objectives**  
✅ Learn how to define **named queries** inside the `@Entity` class  
✅ Use **JPQL** to create reusable queries  
✅ Execute named queries in `StudentRepository`  

---

## **🔹 Step 1: Define Named Queries in `Student` Entity**  

We define **`@NamedQuery`** above the entity class using **JPQL (Java Persistence Query Language)**.

```java
package com.example.model;

import jakarta.persistence.*;

@Entity
@Table(name = "students")
@NamedQueries({
    @NamedQuery(
        name = "Student.findByEmail",
        query = "SELECT s FROM Student s WHERE s.email = :email"
    ),
    @NamedQuery(
        name = "Student.findByLastName",
        query = "SELECT s FROM Student s WHERE s.lastName = :lastName"
    ),
    @NamedQuery(
        name = "Student.updateEmailById",
        query = "UPDATE Student s SET s.email = :email WHERE s.id = :id"
    )
})
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    private String firstName;
    private String lastName;
    private String email;

    // Constructors, Getters, and Setters
    public Student() {}

    public Student(String firstName, String lastName, String email) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.email = email;
    }

    // Getters and Setters omitted for brevity
}
```
✅ **Why?**  
- `@NamedQueries` groups multiple named queries together.  
- `@NamedQuery(name = "...", query = "...")` defines a reusable JPQL query.  
- Queries can **filter, update, or delete** data efficiently.  

---

## **🔹 Step 2: Use Named Queries in `StudentRepository`**  

Now, we reference **named queries** in the repository interface.

```java
package com.example.repository;

import com.example.model.Student;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
public interface StudentRepository extends JpaRepository<Student, Integer> {

    // Using NamedQuery defined in the Student entity
    @Query(name = "Student.findByEmail")
    Student findByEmail(@Param("email") String email);

    @Query(name = "Student.findByLastName")
    List<Student> findByLastName(@Param("lastName") String lastName);
}
```
✅ **Why?**  
- **JPQL query is already defined in the entity**, so we only reference it here.  
- **`@Query(name = "...")`** fetches the query from `@NamedQuery`.  

---

## **🔹 Step 3: Test in `Main.java`**
```java
package com.example;

import com.example.model.Student;
import com.example.service.StudentService;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
public class Main {
    public static void main(String[] args) {
        SpringApplication.run(Main.class, args);
    }

    @Bean
    public CommandLineRunner demo(StudentService studentService) {
        return args -> {
            // Save some students
            studentService.saveStudent(new Student("Alice", "Smith", "alice@example.com"));
            studentService.saveStudent(new Student("Bob", "Smith", "bob@example.com"));

            // Find student by email
            Student student = studentService.getStudentByEmail("alice@example.com");
            System.out.println("Found: " + student.getFirstName() + " " + student.getLastName());

            // Find students by last name
            System.out.println("Students with last name 'Smith': " + studentService.getStudentsByLastName("Smith"));
        };
    }
}
```
✅ **Why?**  
- Demonstrates how `@NamedQuery` is used in a real-world scenario.  
- Fetches a student by email and lists students with the same last name.  

---

# **📌 Key Takeaways**
| **Feature**           | **Explanation** |
|------------------------|----------------|
| `@NamedQuery`         | Defines a reusable query inside an entity class |
| `@NamedQueries`       | Groups multiple named queries |
| `@Query(name = "...")` | References the named query in Spring Data JPA |
| **Advantage**         | Better organization and query reuse |

---

# **🚀 Next Steps**
✅ Modify **named queries** to support **pagination & sorting**  
✅ Implement an **update query** using `@Modifying` and `@NamedQuery`  

👉 **Would you like to see an example of modifying named queries for updates?** 😊