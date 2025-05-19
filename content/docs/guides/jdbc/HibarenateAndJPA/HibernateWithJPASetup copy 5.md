---
title: "Transaction Propagation"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 406
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
### **🛠 Exploring Transaction Propagation in Spring Data JPA**
Spring transactions allow control over how transactions behave across method calls using **Propagation** settings. These determine whether a method runs within an existing transaction or starts a new one.

---

## **📌 Key Propagation Types**
| Propagation Type  | Behavior |
|-------------------|----------|
| `REQUIRED` (default) | Uses the existing transaction if available; otherwise, creates a new one. |
| `REQUIRES_NEW` | Always creates a new transaction, suspending any existing transaction. |
| `NESTED` | Runs within the existing transaction but allows independent rollback for the nested transaction. |
| `MANDATORY` | Requires an active transaction; fails if none exists. |
| `SUPPORTS` | Runs within a transaction if available, but does not create one if none exists. |
| `NOT_SUPPORTED` | Runs outside of a transaction, suspending any existing transaction. |
| `NEVER` | Ensures no transaction exists; throws an error if called within a transaction. |

---

## **📌 Step-by-Step Example: Exploring Propagation Types**
We'll modify our **Student Management System** to demonstrate these behaviors.

---

### **1️⃣ Create `Student` Entity**
```java
package com.example.model;

import jakarta.persistence.*;

@Entity
@Table(name = "students")
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    private String name;
    private String email;

    public Student() {}

    public Student(String name, String email) {
        this.name = name;
        this.email = email;
    }

    // Getters and Setters
}
```

---

### **2️⃣ Create Repository**
```java
package com.example.repository;

import com.example.model.Student;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface StudentRepository extends JpaRepository<Student, Integer> {
}
```

---

### **3️⃣ Define Service Methods with Different Propagation Types**
#### **Main Service: `StudentService`**
```java
package com.example.service;

import com.example.model.Student;
import com.example.repository.StudentRepository;
import jakarta.transaction.Transactional;
import org.springframework.stereotype.Service;

@Service
public class StudentService {

    private final StudentRepository studentRepository;
    private final RegistrationService registrationService;

    public StudentService(StudentRepository studentRepository, RegistrationService registrationService) {
        this.studentRepository = studentRepository;
        this.registrationService = registrationService;
    }

    @Transactional
    public void registerStudent(String name, String email, boolean fail) {
        System.out.println("Registering Student: " + name);
        studentRepository.save(new Student(name, email));

        // Call another transactional method with different propagation
        registrationService.registerCourse(name, fail);
    }
}
```

---

#### **Supporting Service: `RegistrationService`**
```java
package com.example.service;

import jakarta.transaction.Transactional;
import org.springframework.stereotype.Service;

@Service
public class RegistrationService {

    @Transactional(propagation = jakarta.transaction.Transactional.TxType.REQUIRED)
    public void registerCourse(String name, boolean fail) {
        System.out.println("Registering Course for: " + name);
        
        if (fail) {
            throw new RuntimeException("Course registration failed!");
        }
    }
}
```
---

## **📌 Step 4: Testing Different Propagation Types**
### **Test 1: `REQUIRED` Propagation (Default)**
```java
@Service
public class RegistrationService {

    @Transactional(propagation = jakarta.transaction.Transactional.TxType.REQUIRED)
    public void registerCourse(String name, boolean fail) {
        System.out.println("Registering Course for: " + name);

        if (fail) {
            throw new RuntimeException("Course registration failed!");
        }
    }
}
```
**Behavior:**
- Uses the existing transaction from `StudentService`.
- If `registerCourse()` **fails**, the entire transaction **rolls back** (both student and course are not saved).

---

### **Test 2: `REQUIRES_NEW` (Independent Transaction)**
```java
@Service
public class RegistrationService {

    @Transactional(propagation = jakarta.transaction.Transactional.TxType.REQUIRES_NEW)
    public void registerCourse(String name, boolean fail) {
        System.out.println("Registering Course for: " + name);

        if (fail) {
            throw new RuntimeException("Course registration failed!");
        }
    }
}
```
**Behavior:**
- `registerCourse()` runs **in a separate transaction**.
- If it fails, only the course registration **rolls back**, but the student is still saved.

---

### **Test 3: `NESTED` (Independent Rollback)**
```java
@Service
public class RegistrationService {

    @Transactional(propagation = jakarta.transaction.Transactional.TxType.NESTED)
    public void registerCourse(String name, boolean fail) {
        System.out.println("Registering Course for: " + name);

        if (fail) {
            throw new RuntimeException("Course registration failed!");
        }
    }
}
```
**Behavior:**
- Runs **within the existing transaction**, but allows **partial rollback** of `registerCourse()`.
- If `registerCourse()` fails, **only the nested transaction rolls back**, but the main transaction continues.

---

### **Test 4: `MANDATORY` (Requires Existing Transaction)**
```java
@Service
public class RegistrationService {

    @Transactional(propagation = jakarta.transaction.Transactional.TxType.MANDATORY)
    public void registerCourse(String name, boolean fail) {
        System.out.println("Registering Course for: " + name);
    }
}
```
**Behavior:**
- If `registerCourse()` is called **without an active transaction**, it **throws an exception**.

---

### **Test 5: `SUPPORTS` (Optional Transaction)**
```java
@Service
public class RegistrationService {

    @Transactional(propagation = jakarta.transaction.Transactional.TxType.SUPPORTS)
    public void registerCourse(String name, boolean fail) {
        System.out.println("Registering Course for: " + name);
    }
}
```
**Behavior:**
- Runs within an existing transaction **if available**, otherwise **runs without one**.

---

### **Test 6: `NOT_SUPPORTED` (Forces No Transaction)**
```java
@Service
public class RegistrationService {

    @Transactional(propagation = jakarta.transaction.Transactional.TxType.NOT_SUPPORTED)
    public void registerCourse(String name, boolean fail) {
        System.out.println("Registering Course for: " + name);
    }
}
```
**Behavior:**
- Suspends any active transaction and runs without one.

---

### **Test 7: `NEVER` (Throws Exception if Called in Transaction)**
```java
@Service
public class RegistrationService {

    @Transactional(propagation = jakarta.transaction.Transactional.TxType.NEVER)
    public void registerCourse(String name, boolean fail) {
        System.out.println("Registering Course for: " + name);
    }
}
```
**Behavior:**
- **Fails if called within a transaction**.

---

## **📌 Summary**
| **Propagation Type**  | **Behavior** |
|----------------------|-------------|
| **REQUIRED**  | Uses the existing transaction, or creates a new one if none exists. |
| **REQUIRES_NEW**  | Always creates a new transaction, suspending the existing one. |
| **NESTED**  | Runs inside the existing transaction but allows independent rollback. |
| **MANDATORY**  | Throws an error if no existing transaction is found. |
| **SUPPORTS**  | Uses the existing transaction if available, but runs without one if none exists. |
| **NOT_SUPPORTED**  | Always runs **outside** of a transaction. |
| **NEVER**  | Throws an error if called within a transaction. |

---

## **🚀 Next Steps**
✅ **Try different propagation types** and observe rollback behavior.  
✅ **Explore rollback behavior with checked and unchecked exceptions**.  

👉 Would you like to explore **rollback behavior with checked exceptions next?** 😊