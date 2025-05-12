---
title: "Just JDBC"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 301
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
By implementing the Student Management System using **JDBC**, you will see the manual effort required in handling connections, queries, and transactions, which makes you realize Hibernate's advantages.  

---

# **🚀 Step-by-Step Guide: Implementing Student Management with JDBC (H2 Database)**  

### **🎯 What Will Students Learn?**  
✅ How to connect to an **H2 database** using JDBC  
✅ How to perform **CRUD operations** manually  
✅ Understand the limitations of JDBC before transitioning to Hibernate  

---

## **🔹 Step 1: Set Up JDBC Configuration**  

📌 **Add H2 Database Dependency (For Maven Projects)**  
```xml
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
```

📌 **Configure Database Connection in `db.properties`**  
```properties
jdbc.url=jdbc:h2:mem:testdb
jdbc.username=sa
jdbc.password=
jdbc.driverClassName=org.h2.Driver
```

---

## **🔹 Step 2: Create `JDBCUtil` for Database Connection**  
```java
package com.example.util;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.Properties;
import java.io.InputStream;

public class JDBCUtil {
    private static String url;
    private static String username;
    private static String password;

    static {
        try (InputStream input = JDBCUtil.class.getClassLoader().getResourceAsStream("db.properties")) {
            Properties prop = new Properties();
            prop.load(input);
            url = prop.getProperty("jdbc.url");
            username = prop.getProperty("jdbc.username");
            password = prop.getProperty("jdbc.password");
            Class.forName(prop.getProperty("jdbc.driverClassName"));
        } catch (Exception e) {
            e.printStackTrace();
            throw new RuntimeException("Failed to load database properties");
        }
    }

    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(url, username, password);
    }
}
```
✅ **Why?** Manages database connection setup, making it reusable across the application.

---

## **🔹 Step 3: Create `Student` Model Class**  
```java
package com.example.model;

public class Student {
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
✅ **Why?** Represents the `Student` entity for our system.

---

## **🔹 Step 4: Create `StudentDAO` for CRUD Operations**
```java
package com.example.dao;

import com.example.model.Student;
import com.example.util.JDBCUtil;

import java.sql.*;
import java.util.ArrayList;
import java.util.List;

public class StudentDAO {

    // Save Student
    public void saveStudent(Student student) {
        String sql = "INSERT INTO Student (first_name, last_name, email) VALUES (?, ?, ?)";
        try (Connection conn = JDBCUtil.getConnection();
             PreparedStatement stmt = conn.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS)) {
            stmt.setString(1, student.getFirstName());
            stmt.setString(2, student.getLastName());
            stmt.setString(3, student.getEmail());
            stmt.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    // Fetch All Students
    public List<Student> getAllStudents() {
        List<Student> students = new ArrayList<>();
        String sql = "SELECT * FROM Student";
        try (Connection conn = JDBCUtil.getConnection();
             PreparedStatement stmt = conn.prepareStatement(sql);
             ResultSet rs = stmt.executeQuery()) {
            while (rs.next()) {
                Student student = new Student();
                student.setId(rs.getInt("id"));
                student.setFirstName(rs.getString("first_name"));
                student.setLastName(rs.getString("last_name"));
                student.setEmail(rs.getString("email"));
                students.add(student);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return students;
    }

    // Update Student
    public void updateStudent(int id, String newEmail) {
        String sql = "UPDATE Student SET email = ? WHERE id = ?";
        try (Connection conn = JDBCUtil.getConnection();
             PreparedStatement stmt = conn.prepareStatement(sql)) {
            stmt.setString(1, newEmail);
            stmt.setInt(2, id);
            stmt.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    // Delete Student
    public void deleteStudent(int id) {
        String sql = "DELETE FROM Student WHERE id = ?";
        try (Connection conn = JDBCUtil.getConnection();
             PreparedStatement stmt = conn.prepareStatement(sql)) {
            stmt.setInt(1, id);
            stmt.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```
✅ **Why?** Manages **CRUD operations manually** using raw SQL.

---

## **🔹 Step 5: Create `Main.java` to Test JDBC Implementation**
```java
package com.example;

import com.example.dao.StudentDAO;
import com.example.model.Student;

import java.util.List;

public class Main {
    public static void main(String[] args) {
        StudentDAO studentDAO = new StudentDAO();

        // Save Student
        System.out.println("Saving Student...");
        studentDAO.saveStudent(new Student("John", "Doe", "john.doe@example.com"));

        // Fetch All Students
        System.out.println("Fetching Students...");
        List<Student> students = studentDAO.getAllStudents();
        students.forEach(s -> System.out.println(s.getId() + " " + s.getFirstName() + " " + s.getEmail()));

        // Update Student
        System.out.println("Updating Student Email...");
        studentDAO.updateStudent(1, "john.updated@example.com");

        // Delete Student
        System.out.println("Deleting Student...");
        studentDAO.deleteStudent(1);
    }
}
```

✅ **Expected Output**
```
Saving Student...
Fetching Students...
1 John john.doe@example.com
Updating Student Email...
Deleting Student...
```

---

# **📌 Why Is This Approach Limiting?**
| JDBC Issues | Hibernate/JPA Solution |
|------------|-----------------------|
| **Boilerplate Code**: Requires manually handling **connections, statements, and result sets**. | Hibernate **eliminates boilerplate code** using `Session` and `EntityManager`. |
| **No Caching**: Every query runs directly on the DB, increasing latency. | Hibernate has **first-level and second-level caching** for better performance. |
| **No Automatic Mapping**: Need to manually set object properties from `ResultSet`. | Hibernate **automatically maps** objects using annotations. |
| **Difficult Transactions**: Requires manual commit/rollback handling. | Hibernate **automates transactions** with `@Transactional`. |
| **Vendor-Specific Code**: SQL queries may not be portable across databases. | JPA supports **database-independent queries** using JPQL. |

---

# **🚀 Next Step: Transition to JPA**
Now that we've seen the limitations of JDBC, let's **switch to JPA**, where the framework handles transactions, mappings, and queries automatically.  

👉 **Are you ready to move forward with JPA?** 😃