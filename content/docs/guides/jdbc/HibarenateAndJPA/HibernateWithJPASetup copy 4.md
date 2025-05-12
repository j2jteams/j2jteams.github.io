---
title: "Transactionality"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 405
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
### **🛠 Understanding Transactions with Multiple Domain Objects in Spring Data JPA**

To grasp **transaction boundaries, rollback scenarios, and failure handling**, let's build an example using **two related domain objects: `Student` and `CourseRegistration`**.

---

## **🔹 Scenario:**
We will create a service where:  
✅ A student registers for a course.  
✅ Both `Student` and `CourseRegistration` are saved **within the same transaction**.  
✅ If anything goes wrong (like a database constraint violation), the entire transaction **rolls back**.

---

## **📌 Step 1: Define Domain Entities**
### **1️⃣ Student Entity**
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
}
```

---

### **2️⃣ CourseRegistration Entity**
```java
package com.example.model;

import jakarta.persistence.*;

@Entity
@Table(name = "course_registrations")
public class CourseRegistration {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    @ManyToOne
    @JoinColumn(name = "student_id")
    private Student student;

    private String courseName;

    public CourseRegistration() {}

    public CourseRegistration(Student student, String courseName) {
        this.student = student;
        this.courseName = courseName;
    }

    // Getters and Setters
}
```

---

## **📌 Step 2: Create Repositories**
### **1️⃣ StudentRepository**
```java
package com.example.repository;

import com.example.model.Student;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface StudentRepository extends JpaRepository<Student, Integer> {
}
```

### **2️⃣ CourseRegistrationRepository**
```java
package com.example.repository;

import com.example.model.CourseRegistration;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface CourseRegistrationRepository extends JpaRepository<CourseRegistration, Integer> {
}
```

---

## **📌 Step 3: Implement Transactional Service**
```java
package com.example.service;

import com.example.model.CourseRegistration;
import com.example.model.Student;
import com.example.repository.CourseRegistrationRepository;
import com.example.repository.StudentRepository;
import jakarta.transaction.Transactional;
import org.springframework.stereotype.Service;

@Service
public class RegistrationService {

    private final StudentRepository studentRepository;
    private final CourseRegistrationRepository courseRegistrationRepository;

    public RegistrationService(StudentRepository studentRepository, CourseRegistrationRepository courseRegistrationRepository) {
        this.studentRepository = studentRepository;
        this.courseRegistrationRepository = courseRegistrationRepository;
    }

    @Transactional
    public void registerStudentForCourse(String firstName, String lastName, String email, String courseName) {
        Student student = new Student(firstName, lastName, email);
        studentRepository.save(student);

        // Simulating an error (uncomment to test rollback)
        // if (true) throw new RuntimeException("Something went wrong!");

        CourseRegistration registration = new CourseRegistration(student, courseName);
        courseRegistrationRepository.save(registration);
    }
}
```

### **📝 Explanation**
- The method `registerStudentForCourse(...)` is **annotated with `@Transactional`**, ensuring that **both `Student` and `CourseRegistration` are saved within the same transaction**.
- If an **exception occurs**, the transaction **rolls back**, and neither object is saved.

---

## **📌 Step 4: Test Transaction Handling**
```java
package com.example;

import com.example.service.RegistrationService;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
public class Main {
    public static void main(String[] args) {
        SpringApplication.run(Main.class, args);
    }

    @Bean
    public CommandLineRunner demo(RegistrationService registrationService) {
        return args -> {
            try {
                System.out.println("Registering student...");
                registrationService.registerStudentForCourse("John", "Doe", "john.doe@example.com", "Spring Boot");

                // Uncomment to simulate failure and rollback
                // registrationService.registerStudentForCourse("Jane", "Doe", "jane.doe@example.com", "Spring Boot");
                System.out.println("Student successfully registered!");
            } catch (Exception e) {
                System.out.println("Transaction failed: " + e.getMessage());
            }
        };
    }
}
```

---

## **📌 Step 5: Simulating Rollback**
### ✅ **Successful Transaction**
If everything works fine, both `Student` and `CourseRegistration` are saved.

**🛠 Database Records**
| Student | Course Registration |
|---------|---------------------|
| John Doe | Spring Boot |

---

### ❌ **Failed Transaction & Rollback**
If we **uncomment the exception** in `registerStudentForCourse()`, the entire transaction **rolls back**, and **no records are saved**.

**📝 Why?**
- `@Transactional` ensures **atomicity**: either **all** changes succeed or **none** are applied.

---

## **📌 Key Takeaways**
| **Concept**           | **Explanation** |
|------------------------|----------------|
| `@Transactional`       | Ensures all database operations are atomic (all succeed or all fail). |
| **Rollback**          | If an exception occurs, Spring **undoes** all changes automatically. |
| **Propagation**       | Determines how transactions interact when calling methods within other transactions. |
| **Checked vs. Unchecked Exceptions** | By default, **unchecked exceptions** (RuntimeException) trigger a rollback, while **checked exceptions** (Exception) do not. |

---

## **🚀 Next Steps**
✅ **Explore Propagation Types** (e.g., `REQUIRED`, `REQUIRES_NEW`, `NESTED`)  
✅ **Handle Checked Exceptions** in transactions  

👉 **Would you like to explore different transaction propagation behaviors next?** 😊