---
title: "Using @Modifying"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 403
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Great! Now, let's explore **modifying queries** using `@Modifying` in Spring Data JPA for updating and deleting records.

---

# **🎯 Learning Objectives**  
✅ Learn how to use `@Modifying` for **update and delete operations**  
✅ Understand when to use **`@Transactional`** with modifying queries  
✅ Implement modifying queries in `StudentRepository`  

---

## **🔹 Step 1: Update `StudentRepository` with Modifying Queries**  
Spring Data JPA allows modifying queries using `@Modifying`, usually combined with `@Transactional`.

```java
package com.example.repository;

import com.example.model.Student;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Modifying;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;
import org.springframework.stereotype.Repository;
import org.springframework.transaction.annotation.Transactional;

@Repository
public interface StudentRepository extends JpaRepository<Student, Integer> {

    // Update student's email by ID
    @Modifying
    @Transactional
    @Query("UPDATE Student s SET s.email = :email WHERE s.id = :id")
    int updateEmailById(@Param("id") int id, @Param("email") String email);

    // Delete a student by ID
    @Modifying
    @Transactional
    @Query("DELETE FROM Student s WHERE s.id = :id")
    void deleteStudentById(@Param("id") int id);

    // Delete students by last name
    @Modifying
    @Transactional
    @Query("DELETE FROM Student s WHERE s.lastName = :lastName")
    int deleteStudentsByLastName(@Param("lastName") String lastName);
}
```
✅ **Why?**  
- `@Modifying` tells Spring that the query modifies the database.  
- `@Transactional` ensures the operation is wrapped in a transaction.  
- The `int` return type gives the number of rows affected.

---

## **🔹 Step 2: Update `StudentService` to Call Modifying Queries**
```java
package com.example.service;

import com.example.repository.StudentRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class StudentService {

    @Autowired
    private StudentRepository studentRepository;

    @Transactional
    public int updateStudentEmail(int id, String email) {
        return studentRepository.updateEmailById(id, email);
    }

    @Transactional
    public void removeStudentById(int id) {
        studentRepository.deleteStudentById(id);
    }

    @Transactional
    public int removeStudentsByLastName(String lastName) {
        return studentRepository.deleteStudentsByLastName(lastName);
    }
}
```
✅ **Why?**  
- Methods wrap modifying queries for cleaner service logic.  
- Transactional methods ensure consistency when modifying data.

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
            Student student = new Student("John", "Doe", "john.doe@example.com");
            studentService.saveStudent(student);

            // Update Email
            int updated = studentService.updateStudentEmail(student.getId(), "new.email@example.com");
            System.out.println("Updated " + updated + " student(s) email");

            // Delete by ID
            studentService.removeStudentById(student.getId());
            System.out.println("Student removed by ID");

            // Delete by Last Name
            int deleted = studentService.removeStudentsByLastName("Doe");
            System.out.println("Deleted " + deleted + " student(s) with last name Doe");
        };
    }
}
```
✅ **Why?**  
- Saves a student, updates the email, and deletes students dynamically.  
- Confirms that modifying queries work as expected.

---

# **📌 Key Takeaways**
| **Feature**            | **Explanation** |
|------------------------|----------------|
| `@Modifying`          | Marks queries that modify data |
| `@Transactional`      | Ensures updates/deletes run in a transaction |
| Return Type           | Returns affected rows count (for updates/deletes) |
| `@Query`              | Supports JPQL and Native SQL |

---

# **🚀 Next Steps**
✅ Extend modifying queries to update multiple fields  
✅ Add logging to check before-and-after changes  
✅ Implement **Spring REST API** to trigger modifying queries  

👉 **Would you like to explore modifying multiple fields next?** 😊