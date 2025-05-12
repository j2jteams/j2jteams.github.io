---
title: "Hibernate CRUD Operations"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 304
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

# **🚀 Step-by-Step Guide: CRUD Operations in Hibernate with H2**  

Now that we have set up **Hibernate with H2**, let's implement **CRUD (Create, Read, Update, Delete) operations** for our **Student Management Application**.  

---

## **🔹 Step 1: Create `StudentDAO` (Data Access Object)**
We will use the **DAO pattern** to manage database operations.

📌 **Create `StudentDAO.java`**
```java
package com.example.dao;

import com.example.model.Student;
import com.example.util.HibernateUtil;
import org.hibernate.Session;
import org.hibernate.Transaction;
import java.util.List;

public class StudentDAO {

    // CREATE - Add new student
    public void saveStudent(Student student) {
        Transaction transaction = null;
        try (Session session = HibernateUtil.getSessionFactory().openSession()) {
            transaction = session.beginTransaction();
            session.save(student);
            transaction.commit();
            System.out.println("Student saved: " + student);
        } catch (Exception e) {
            if (transaction != null) transaction.rollback();
            e.printStackTrace();
        }
    }

    // READ - Get a student by ID
    public Student getStudentById(int id) {
        try (Session session = HibernateUtil.getSessionFactory().openSession()) {
            return session.get(Student.class, id);
        }
    }

    // READ - Get all students
    public List<Student> getAllStudents() {
        try (Session session = HibernateUtil.getSessionFactory().openSession()) {
            return session.createQuery("from Student", Student.class).list();
        }
    }

    // UPDATE - Modify student details
    public void updateStudent(Student student) {
        Transaction transaction = null;
        try (Session session = HibernateUtil.getSessionFactory().openSession()) {
            transaction = session.beginTransaction();
            session.update(student);
            transaction.commit();
            System.out.println("Student updated: " + student);
        } catch (Exception e) {
            if (transaction != null) transaction.rollback();
            e.printStackTrace();
        }
    }

    // DELETE - Remove student by ID
    public void deleteStudent(int id) {
        Transaction transaction = null;
        try (Session session = HibernateUtil.getSessionFactory().openSession()) {
            transaction = session.beginTransaction();
            Student student = session.get(Student.class, id);
            if (student != null) {
                session.delete(student);
                System.out.println("Deleted student: " + student);
            }
            transaction.commit();
        } catch (Exception e) {
            if (transaction != null) transaction.rollback();
            e.printStackTrace();
        }
    }
}
```

✅ **What this DAO does:**  
✔ `saveStudent()` → Adds a new student to the database.  
✔ `getStudentById()` → Fetches a student by ID.  
✔ `getAllStudents()` → Retrieves all students from the database.  
✔ `updateStudent()` → Modifies an existing student's details.  
✔ `deleteStudent()` → Removes a student by ID.  

---

## **🔹 Step 2: Test CRUD Operations in `Main.java`**
📌 **Modify `Main.java` to test CRUD operations**
```java
package com.example;

import com.example.dao.StudentDAO;
import com.example.model.Student;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        StudentDAO studentDAO = new StudentDAO();

        // CREATE - Add new students
        Student student1 = new Student("Alice", "Brown", "alice.brown@example.com");
        Student student2 = new Student("Bob", "Smith", "bob.smith@example.com");
        studentDAO.saveStudent(student1);
        studentDAO.saveStudent(student2);

        // READ - Fetch all students
        List<Student> students = studentDAO.getAllStudents();
        System.out.println("All Students: " + students);

        // READ - Get a student by ID
        Student fetchedStudent = studentDAO.getStudentById(1);
        System.out.println("Fetched Student: " + fetchedStudent);

        // UPDATE - Modify a student's details
        fetchedStudent.setEmail("alice.updated@example.com");
        studentDAO.updateStudent(fetchedStudent);

        // READ - Verify update
        Student updatedStudent = studentDAO.getStudentById(1);
        System.out.println("Updated Student: " + updatedStudent);

        // DELETE - Remove a student
        studentDAO.deleteStudent(2);

        // READ - Verify deletion
        List<Student> studentsAfterDelete = studentDAO.getAllStudents();
        System.out.println("Students after deletion: " + studentsAfterDelete);
    }
}
```

✅ **Expected Output**
```
Hibernate: insert into students (email, first_name, last_name) values (?, ?, ?)
Hibernate: insert into students (email, first_name, last_name) values (?, ?, ?)
Student saved: Student{id=1, firstName='Alice', lastName='Brown', email='alice.brown@example.com'}
Student saved: Student{id=2, firstName='Bob', lastName='Smith', email='bob.smith@example.com'}
All Students: [Student{id=1, firstName='Alice', lastName='Brown', email='alice.brown@example.com'}, 
              Student{id=2, firstName='Bob', lastName='Smith', email='bob.smith@example.com'}]
Fetched Student: Student{id=1, firstName='Alice', lastName='Brown', email='alice.brown@example.com'}
Hibernate: update students set email=?, first_name=?, last_name=? where id=?
Student updated: Student{id=1, firstName='Alice', lastName='Brown', email='alice.updated@example.com'}
Updated Student: Student{id=1, firstName='Alice', lastName='Brown', email='alice.updated@example.com'}
Deleted student: Student{id=2, firstName='Bob', lastName='Smith', email='bob.smith@example.com'}
Students after deletion: [Student{id=1, firstName='Alice', lastName='Brown', email='alice.updated@example.com'}]
```

---

# **✅ Summary**
### **🎯 What We Implemented**
✔ **Configured Hibernate with H2 in-memory database**  
✔ **Created a `StudentDAO` for CRUD operations**  
✔ **Implemented `save`, `get`, `update`, and `delete` operations**  
✔ **Tested CRUD operations step by step**  

### **🚀 Next Steps**
Would you like to move forward with **Hibernate Queries (HQL, Criteria API, and Native SQL)?** 😃