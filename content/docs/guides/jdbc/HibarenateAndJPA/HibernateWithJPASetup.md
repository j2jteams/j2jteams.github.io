---
title: "Hibernate with Spring JPA Setup"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 401
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
We'll transition from **Spring with Hibernate** to **Spring Data JPA** for the Student Management System. This will help you understand d how JPA simplifies ORM by eliminating the need for direct `SessionFactory` or `Session` handling.

---

# **🚀 Step-by-Step Guide: Implementing Student Management with Spring Data JPA (H2 Database)**  

## **🎯 Learning Objectives**  
✅ Understand how **Spring Data JPA** simplifies ORM  
✅ Configure **Spring Data JPA with H2 Database**  
✅ Perform **CRUD operations** using **Spring Data JPA**  
✅ Explore **Spring Boot advantages over manual Hibernate setup**  

---

## **🔹 Step 1: Add Dependencies (Maven)**  
We'll use **Spring Data JPA**, **H2 Database**, and **Spring Boot Starter** dependencies.  
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
    <version>2.7.10</version>
</dependency>

<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
```
✅ **Why?**  
- `spring-boot-starter-data-jpa`: Includes Hibernate and JPA features.  
- `h2`: In-memory database for easy testing.  

---

## **🔹 Step 2: Configure `application.properties`**  
Create this file inside `src/main/resources/` for database configuration.  
```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=update
spring.h2.console.enabled=true
```
✅ **Why?**  
- Enables **H2 in-memory database** with an interactive console.  
- `spring.jpa.hibernate.ddl-auto=update`: Auto-creates tables based on entities.  

---

## **🔹 Step 3: Define `Student` Entity**
Now, we define our `Student` entity using **JPA annotations**.  
```java
package com.example.model;

import javax.persistence.*;

@Entity
@Table(name = "students")
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    @Column(name = "first_name")
    private String firstName;

    @Column(name = "last_name")
    private String lastName;

    @Column(name = "email")
    private String email;

    public Student() {}

    public Student(String firstName, String lastName, String email) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.email = email;
    }

    public int getId() { return id; }
    public void setId(int id) { this.id = id; }
    public String getFirstName() { return firstName; }
    public void setFirstName(String firstName) { this.firstName = firstName; }
    public String getLastName() { return lastName; }
    public void setLastName(String lastName) { this.lastName = lastName; }
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}
```
✅ **Why?**  
- Uses **`@Entity`** to mark this as a JPA entity.  
- **`@Id` & `@GeneratedValue`** auto-generate primary key.  

---

## **🔹 Step 4: Create `StudentRepository`**
Spring Data JPA allows us to create repositories without writing **boilerplate DAO code**.  
```java
package com.example.repository;

import com.example.model.Student;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface StudentRepository extends JpaRepository<Student, Integer> {
}
```
✅ **Why?**  
- `JpaRepository<Student, Integer>` provides **built-in CRUD operations**.  
- **No need for `SessionFactory`, `EntityManager`, or SQL Queries`!**  

---

## **🔹 Step 5: Create `StudentService` for Business Logic**
We'll use **`@Service`** to create a service layer.  
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

    public Student saveStudent(Student student) {
        return studentRepository.save(student);
    }

    public List<Student> getAllStudents() {
        return studentRepository.findAll();
    }

    public Student updateStudent(int id, String newEmail) {
        Student student = studentRepository.findById(id).orElse(null);
        if (student != null) {
            student.setEmail(newEmail);
            return studentRepository.save(student);
        }
        return null;
    }

    public void deleteStudent(int id) {
        studentRepository.deleteById(id);
    }
}
```
✅ **Why?**  
- Uses **Spring's `@Service` layer** to handle business logic.  
- **Delegates** database tasks to `StudentRepository`.  

---

## **🔹 Step 6: Create `Main.java` to Test JPA Operations**
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
            // Save Student
            System.out.println("Saving Student...");
            studentService.saveStudent(new Student("Alice", "Brown", "alice@example.com"));

            // Fetch All Students
            System.out.println("Fetching Students...");
            List<Student> students = studentService.getAllStudents();
            students.forEach(s -> System.out.println(s.getId() + " " + s.getFirstName() + " " + s.getEmail()));

            // Update Student
            System.out.println("Updating Student Email...");
            studentService.updateStudent(1, "alice.updated@example.com");

            // Delete Student
            System.out.println("Deleting Student...");
            studentService.deleteStudent(1);
        };
    }
}
```
✅ **Why?**  
- Uses `@SpringBootApplication` for auto-configuration.  
- Runs queries inside `CommandLineRunner` for testing.  

---

# **📌 Key Takeaways: Why JPA is Better than Hibernate**
| **Hibernate (ORM)** | **Spring Data JPA** |
|--------------------|--------------------|
| **Requires `SessionFactory`** | Uses `JpaRepository` (No need for `SessionFactory`) |
| **Manual Transactions** | Transactions handled automatically |
| **HQL Queries Required** | Uses method names like `findById()` |
| **More Configuration Needed** | Auto-configures with `spring.jpa.hibernate.ddl-auto=update` |

---

# **🚀 Next Steps: Expand Functionality**
Now that we have **Spring Data JPA working**, we can expand by:  
✅ Adding **Custom Queries** (`@Query`)  
✅ Implementing **Spring REST API**  
✅ Using **Spring Security for authentication**  

👉 **Are you ready to move forward?** 😊