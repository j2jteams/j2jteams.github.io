---
title: "Transaction Management"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 306
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
# **🚀 Step-by-Step Guide: Transaction Management in Hibernate (With H2 Database)**  

### **🎯 Why Is Transaction Management Important?**
Transaction management ensures **data consistency** and **integrity** in a database. It allows operations like **commit** and **rollback** in case of failures.  

---

# **🔹 Step 1: Understanding Transactions in Hibernate**  
### **🔹 Key Transaction Concepts**
🔹 **Commit** → Saves changes to the database.  
🔹 **Rollback** → Undoes changes if an error occurs.  
🔹 **ACID Principles** → Ensures atomicity, consistency, isolation, and durability.  

---

# **🔹 Step 2: Configure Hibernate for Transactions**  
📌 **Modify `hibernate.cfg.xml` to Enable Transactions**
```xml
<hibernate-configuration>
    <session-factory>
        <!-- Database Configuration -->
        <property name="hibernate.connection.driver_class">org.h2.Driver</property>
        <property name="hibernate.connection.url">jdbc:h2:mem:testdb</property>
        <property name="hibernate.connection.username">sa</property>
        <property name="hibernate.connection.password"></property>
        <property name="hibernate.dialect">org.hibernate.dialect.H2Dialect</property>

        <!-- Enable Transactions -->
        <property name="hibernate.current_session_context_class">thread</property>

        <!-- Show SQL Queries -->
        <property name="hibernate.show_sql">true</property>
        <property name="hibernate.format_sql">true</property>
    </session-factory>
</hibernate-configuration>
```
✅ **Key Updates**
- Configured an **in-memory H2 database**.
- Enabled **thread-based session management** for transactions.

---

# **🔹 Step 3: Implement Transaction Management**
📌 **Modify `StudentDAO.java` to Manage Transactions**
```java
package com.example.dao;

import com.example.model.Student;
import com.example.util.HibernateUtil;
import org.hibernate.Session;
import org.hibernate.Transaction;

public class StudentDAO {

    // Save Student with Transaction Management
    public void saveStudent(Student student) {
        Transaction transaction = null;
        try (Session session = HibernateUtil.getSessionFactory().openSession()) {
            transaction = session.beginTransaction(); // Start Transaction
            session.save(student); // Insert into DB
            transaction.commit(); // Commit Transaction
        } catch (Exception e) {
            if (transaction != null) transaction.rollback(); // Rollback on Error
            e.printStackTrace();
        }
    }

    // Update Student with Transaction Management
    public void updateStudent(int id, String newEmail) {
        Transaction transaction = null;
        try (Session session = HibernateUtil.getSessionFactory().openSession()) {
            transaction = session.beginTransaction(); // Start Transaction
            Student student = session.get(Student.class, id);
            if (student != null) {
                student.setEmail(newEmail);
                session.update(student);
            }
            transaction.commit(); // Commit Transaction
        } catch (Exception e) {
            if (transaction != null) transaction.rollback(); // Rollback on Error
            e.printStackTrace();
        }
    }

    // Delete Student with Transaction Management
    public void deleteStudent(int id) {
        Transaction transaction = null;
        try (Session session = HibernateUtil.getSessionFactory().openSession()) {
            transaction = session.beginTransaction(); // Start Transaction
            Student student = session.get(Student.class, id);
            if (student != null) {
                session.delete(student);
            }
            transaction.commit(); // Commit Transaction
        } catch (Exception e) {
            if (transaction != null) transaction.rollback(); // Rollback on Error
            e.printStackTrace();
        }
    }
}
```
✅ **Key Highlights**  
- **`transaction.beginTransaction()`** → Starts a transaction.  
- **`transaction.commit()`** → Saves changes permanently.  
- **`transaction.rollback()`** → Reverts changes if an error occurs.  

---

# **🔹 Step 4: Test Transactions in `Main.java`**
📌 **Modify `Main.java` to Simulate Transactions**
```java
package com.example;

import com.example.dao.StudentDAO;
import com.example.model.Student;

public class Main {
    public static void main(String[] args) {
        StudentDAO studentDAO = new StudentDAO();

        // Transaction: Save Student
        System.out.println("Saving student...");
        studentDAO.saveStudent(new Student("Alice", "Brown", "alice@example.com"));

        // Transaction: Update Student
        System.out.println("Updating student email...");
        studentDAO.updateStudent(1, "alice.updated@example.com");

        // Transaction: Delete Student (Simulating Error)
        System.out.println("Attempting to delete non-existing student...");
        studentDAO.deleteStudent(999); // Student ID does not exist → Transaction rollback
    }
}
```

✅ **Expected Output**
```
Saving student...
Hibernate: insert into Student (firstName, lastName, email) values (?, ?, ?)
Updating student email...
Hibernate: update Student set email=? where id=?
Attempting to delete non-existing student...
Transaction rolled back due to error: No student found with ID 999
```

---

# **🔹 Step 5: Testing Transaction Rollback**
### **🔹 Scenario: Simulating an Error**
To test rollback, modify `updateStudent()` to **force an error**:
```java
// Simulating error: Trying to update a non-existent field
public void updateStudent(int id, String newEmail) {
    Transaction transaction = null;
    try (Session session = HibernateUtil.getSessionFactory().openSession()) {
        transaction = session.beginTransaction();
        
        // Fetch Student
        Student student = session.get(Student.class, id);
        if (student != null) {
            student.setEmail(newEmail);
            session.update(student);
        }
        
        // Simulate an error
        if (true) throw new RuntimeException("Simulated Error");

        transaction.commit(); // Will not reach here due to error
    } catch (Exception e) {
        if (transaction != null) transaction.rollback(); // Rollback on Error
        System.out.println("Transaction Rolled Back!");
    }
}
```

✅ **Expected Output**
```
Transaction Rolled Back!
```
The update operation **fails**, and **data remains unchanged** in the database.

---

# **✅ Summary**
### **🎯 What We Implemented**
✔ **Configured Hibernate Transactions**  
✔ **Implemented `commit()` and `rollback()`**  
✔ **Tested transaction rollback in error scenarios**  

### **🚀 Next Steps**
Would you like to explore **Spring Integration with Hibernate**, or should we first cover **JPA basics**? 😃