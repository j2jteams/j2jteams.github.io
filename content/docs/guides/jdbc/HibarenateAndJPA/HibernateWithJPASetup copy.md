---
title: "Custom Queries"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 402
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
Great! Now, let's explore **Custom Queries in Spring Data JPA** for our **Student Management System**.  

---

# **🎯 Learning Objectives**  
✅ Learn how to write **Custom Queries** using `@Query` in Spring Data JPA  
✅ Understand the difference between **JPQL and Native Queries**  
✅ Implement **query methods** with Spring Data JPA  

---

## **🔹 Step 1: Add Custom Queries in `StudentRepository`**
Spring Data JPA allows us to define custom queries using `@Query` annotation.  

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

    // 1. Find students by first name using JPQL
    @Query("SELECT s FROM Student s WHERE s.firstName = :firstName")
    List<Student> findByFirstName(@Param("firstName") String firstName);

    // 2. Find students by email using Native Query
    @Query(value = "SELECT * FROM students WHERE email = :email", nativeQuery = true)
    Student findByEmail(@Param("email") String email);

    // 3. Find students whose first name starts with a given prefix
    @Query("SELECT s FROM Student s WHERE s.firstName LIKE :prefix%")
    List<Student> findByFirstNameStartingWith(@Param("prefix") String prefix);

    // 4. Count students with a specific last name
    @Query("SELECT COUNT(s) FROM Student s WHERE s.lastName = :lastName")
    long countByLastName(@Param("lastName") String lastName);
}
```
✅ **Why?**  
- **JPQL Queries** use entity class names (`Student`), not table names.  
- **Native Queries** interact directly with the database.  
- `@Param("value")` binds method parameters to query placeholders.  

---

## **🔹 Step 2: Update `StudentService` to Use Custom Queries**
```java
package com.example.service;

import com.example.model.Student;
import com.example.repository.StudentRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class StudentService {

    @Autowired
    private StudentRepository studentRepository;

    public List<Student> getStudentsByFirstName(String firstName) {
        return studentRepository.findByFirstName(firstName);
    }

    public Student getStudentByEmail(String email) {
        return studentRepository.findByEmail(email);
    }

    public List<Student> getStudentsByFirstNamePrefix(String prefix) {
        return studentRepository.findByFirstNameStartingWith(prefix);
    }

    public long countStudentsByLastName(String lastName) {
        return studentRepository.countByLastName(lastName);
    }
}
```
✅ **Why?**  
- Service methods wrap repository queries for **better abstraction**.  
- Each method delegates query execution to `StudentRepository`.  

---

## **🔹 Step 3: Test Custom Queries in `Main.java`**
```java
package com.example;

import com.example.model.Student;
import com.example.service.StudentService;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;

import java.util.List;

@SpringBootApplication
public class Main {
    public static void main(String[] args) {
        SpringApplication.run(Main.class, args);
    }

    @Bean
    public CommandLineRunner demo(StudentService studentService) {
        return args -> {
            studentService.saveStudent(new Student("Alice", "Brown", "alice@example.com"));
            studentService.saveStudent(new Student("Bob", "Smith", "bob@example.com"));
            studentService.saveStudent(new Student("Alice", "Taylor", "alice.taylor@example.com"));

            // Find by First Name
            List<Student> alices = studentService.getStudentsByFirstName("Alice");
            System.out.println("Students named Alice: " + alices.size());

            // Find by Email
            Student studentByEmail = studentService.getStudentByEmail("bob@example.com");
            System.out.println("Student found by email: " + studentByEmail.getFirstName());

            // Find by First Name Prefix
            List<Student> prefixResults = studentService.getStudentsByFirstNamePrefix("A");
            System.out.println("Students with names starting with A: " + prefixResults.size());

            // Count by Last Name
            long count = studentService.countStudentsByLastName("Brown");
            System.out.println("Number of students with last name 'Brown': " + count);
        };
    }
}
```
✅ **Why?**  
- Adds test data dynamically.  
- Runs custom queries inside `CommandLineRunner` to verify results.  

---

# **📌 Key Takeaways**
| **JPQL (Java Persistence Query Language)** | **Native Queries** |
|------------------------------------------|------------------|
| Uses entity names & attributes (`Student`) | Uses table names (`students`) |
| Database-independent | Tied to specific databases |
| Supports automatic query parsing | Requires SQL syntax |

---

# **🚀 Next Steps**
✅ Expand queries with `@Modifying` (for updates & deletes)  
✅ Add `@NamedQuery` (predefined queries in `@Entity`)  
✅ Implement **Spring REST API** with custom queries  

👉 **Would you like to explore modifying queries next?** 😊