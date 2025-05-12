---
title: "HQL"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 305
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

# **🚀 Step-by-Step Guide: Hibernate Queries (HQL, Criteria API, Native SQL) in H2**  

Now that we have implemented **CRUD operations** using Hibernate, let's explore different ways to query data:  
1️⃣ **HQL (Hibernate Query Language)** – Object-oriented SQL-like queries.  
2️⃣ **Criteria API** – Programmatic approach to building queries.  
3️⃣ **Native SQL Queries** – Direct SQL execution using Hibernate.  

---

## **🔹 Step 1: Implement HQL Queries**
Hibernate Query Language (**HQL**) allows querying using entity names instead of table names.

📌 **Modify `StudentDAO.java`** to add HQL queries:
```java
package com.example.dao;

import com.example.model.Student;
import com.example.util.HibernateUtil;
import org.hibernate.Session;
import org.hibernate.Transaction;
import java.util.List;

public class StudentDAO {

    // Fetch all students using HQL
    public List<Student> getAllStudentsHQL() {
        try (Session session = HibernateUtil.getSessionFactory().openSession()) {
            return session.createQuery("FROM Student", Student.class).list();
        }
    }

    // Fetch students by first name using HQL
    public List<Student> getStudentsByFirstName(String firstName) {
        try (Session session = HibernateUtil.getSessionFactory().openSession()) {
            return session.createQuery("FROM Student WHERE firstName = :firstName", Student.class)
                          .setParameter("firstName", firstName)
                          .list();
        }
    }

    // Update student email using HQL
    public void updateStudentEmail(int id, String newEmail) {
        Transaction transaction = null;
        try (Session session = HibernateUtil.getSessionFactory().openSession()) {
            transaction = session.beginTransaction();
            session.createQuery("UPDATE Student SET email = :email WHERE id = :id")
                   .setParameter("email", newEmail)
                   .setParameter("id", id)
                   .executeUpdate();
            transaction.commit();
        } catch (Exception e) {
            if (transaction != null) transaction.rollback();
            e.printStackTrace();
        }
    }

    // Delete student using HQL
    public void deleteStudentHQL(int id) {
        Transaction transaction = null;
        try (Session session = HibernateUtil.getSessionFactory().openSession()) {
            transaction = session.beginTransaction();
            session.createQuery("DELETE FROM Student WHERE id = :id")
                   .setParameter("id", id)
                   .executeUpdate();
            transaction.commit();
        } catch (Exception e) {
            if (transaction != null) transaction.rollback();
            e.printStackTrace();
        }
    }
}
```

---

## **🔹 Step 2: Implement Criteria API Queries**
The **Criteria API** provides a type-safe, programmatic approach to querying.

📌 **Modify `StudentDAO.java`** to add Criteria API queries:
```java
import org.hibernate.Criteria;
import org.hibernate.criterion.Restrictions;
import org.hibernate.query.Query;
import java.util.List;

public class StudentDAO {

    // Fetch students by last name using Criteria API
    public List<Student> getStudentsByLastName(String lastName) {
        try (Session session = HibernateUtil.getSessionFactory().openSession()) {
            Criteria criteria = session.createCriteria(Student.class);
            criteria.add(Restrictions.eq("lastName", lastName));
            return criteria.list();
        }
    }
}
```

---

## **🔹 Step 3: Implement Native SQL Queries**
Native SQL queries allow executing **raw SQL** statements within Hibernate.

📌 **Modify `StudentDAO.java`** to add Native SQL queries:
```java
import org.hibernate.query.NativeQuery;

public class StudentDAO {

    // Fetch all students using Native SQL
    public List<Student> getAllStudentsNative() {
        try (Session session = HibernateUtil.getSessionFactory().openSession()) {
            NativeQuery<Student> query = session.createNativeQuery("SELECT * FROM students", Student.class);
            return query.list();
        }
    }
}
```

---

## **🔹 Step 4: Test Queries in `Main.java`**
📌 **Modify `Main.java` to test these queries**
```java
package com.example;

import com.example.dao.StudentDAO;
import com.example.model.Student;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        StudentDAO studentDAO = new StudentDAO();

        // Add students
        studentDAO.saveStudent(new Student("Alice", "Brown", "alice@example.com"));
        studentDAO.saveStudent(new Student("Bob", "Smith", "bob@example.com"));

        // Fetch all students using HQL
        List<Student> studentsHQL = studentDAO.getAllStudentsHQL();
        System.out.println("Students using HQL: " + studentsHQL);

        // Fetch by first name using HQL
        List<Student> aliceList = studentDAO.getStudentsByFirstName("Alice");
        System.out.println("Students named Alice: " + aliceList);

        // Update email using HQL
        studentDAO.updateStudentEmail(1, "alice.new@example.com");

        // Delete student using HQL
        studentDAO.deleteStudentHQL(2);

        // Fetch students by last name using Criteria API
        List<Student> studentsCriteria = studentDAO.getStudentsByLastName("Brown");
        System.out.println("Students with last name Brown: " + studentsCriteria);

        // Fetch students using Native SQL
        List<Student> studentsNative = studentDAO.getAllStudentsNative();
        System.out.println("Students using Native SQL: " + studentsNative);
    }
}
```

✅ **Expected Output**
```
Students using HQL: [Student{id=1, firstName='Alice', lastName='Brown', email='alice@example.com'}, 
                      Student{id=2, firstName='Bob', lastName='Smith', email='bob@example.com'}]
Students named Alice: [Student{id=1, firstName='Alice', lastName='Brown', email='alice@example.com'}]
Students with last name Brown: [Student{id=1, firstName='Alice', lastName='Brown', email='alice.new@example.com'}]
Students using Native SQL: [Student{id=1, firstName='Alice', lastName='Brown', email='alice.new@example.com'}]
```

---

# **✅ Summary**
### **🎯 What We Implemented**
✔ **HQL Queries** (`FROM Student`, `UPDATE`, `DELETE`)  
✔ **Criteria API Queries** (`Restrictions.eq("lastName", lastName)`)  
✔ **Native SQL Queries** (`SELECT * FROM students`)  
✔ **Tested queries with different methods**  

### **🚀 Next Steps**
Would you like to move forward with **Spring Integration with Hibernate**, or do you want to explore **Transaction Management in Hibernate** first? 😃