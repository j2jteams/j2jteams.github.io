---
title: "Hibernate With Spring"
description: "Guides lead a user through a specific task they want to accomplish, often with a sequence of steps."
summary: ""
date: 2023-09-07T16:04:48+02:00
lastmod: 2023-09-07T16:04:48+02:00
draft: false
weight: 308
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
We'll now integrate **Spring with Hibernate** for the Student Management System before transitioning to JPA. This will help how Hibernate automates ORM tasks while still requiring some manual configurations.  

---

# **🚀 Step-by-Step Guide: Implementing Student Management with Spring + Hibernate (H2 Database)**  

## **🎯 Learning Objectives**  
✅ Set up **Spring + Hibernate** integration  
✅ Configure **SessionFactory** using Spring  
✅ Perform **CRUD operations** with Hibernate  
✅ Understand how **Spring handles transactions**  

---

## **🔹 Step 1: Add Dependencies (Maven)**
Add the required dependencies for **Spring, Hibernate, and H2 Database**.  
```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.30</version>
</dependency>

<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-orm</artifactId>
    <version>5.3.30</version>
</dependency>

<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-core</artifactId>
    <version>5.6.15.Final</version>
</dependency>

<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
```

---

## **🔹 Step 2: Configure Hibernate in `hibernate.cfg.xml`**
Create this file inside `src/main/resources` to define database properties.  
```xml
<!DOCTYPE hibernate-configuration PUBLIC  
    "-//Hibernate/Hibernate Configuration DTD 3.0//EN"  
    "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">  

<hibernate-configuration>
    <session-factory>
        <property name="hibernate.connection.driver_class">org.h2.Driver</property>
        <property name="hibernate.connection.url">jdbc:h2:mem:testdb</property>
        <property name="hibernate.connection.username">sa</property>
        <property name="hibernate.connection.password"></property>
        <property name="hibernate.dialect">org.hibernate.dialect.H2Dialect</property>
        <property name="hibernate.hbm2ddl.auto">update</property>
        <property name="show_sql">true</property>
    </session-factory>
</hibernate-configuration>
```
✅ **Why?** Defines **database connection** and **dialect**.

---

## **🔹 Step 3: Create Hibernate Configuration Using Spring**
We'll configure **SessionFactory** and **Transaction Management** in `SpringConfig.java`.  

```java
package com.example.config;

import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.orm.hibernate5.HibernateTransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;

@EnableTransactionManagement
@ComponentScan(basePackages = "com.example")
public class SpringConfig {

    @Bean
    public SessionFactory sessionFactory() {
        return new Configuration().configure().buildSessionFactory();
    }

    @Bean
    public HibernateTransactionManager transactionManager(SessionFactory sessionFactory) {
        return new HibernateTransactionManager(sessionFactory);
    }
}
```
✅ **Why?**  
- **`SessionFactory`**: Initializes Hibernate ORM  
- **`HibernateTransactionManager`**: Handles transactions  

---

## **🔹 Step 4: Define `Student` Entity**
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
✅ **Why?** Uses Hibernate’s **JPA annotations** for ORM.

---

## **🔹 Step 5: Create `StudentDAO` for CRUD Operations**
```java
package com.example.dao;

import com.example.model.Student;
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

@Repository
public class StudentDAO {

    @Autowired
    private SessionFactory sessionFactory;

    @Transactional
    public void saveStudent(Student student) {
        Session session = sessionFactory.getCurrentSession();
        session.save(student);
    }

    @Transactional(readOnly = true)
    public List<Student> getAllStudents() {
        Session session = sessionFactory.getCurrentSession();
        return session.createQuery("FROM Student", Student.class).list();
    }

    @Transactional
    public void updateStudent(int id, String newEmail) {
        Session session = sessionFactory.getCurrentSession();
        Student student = session.get(Student.class, id);
        if (student != null) {
            student.setEmail(newEmail);
            session.update(student);
        }
    }

    @Transactional
    public void deleteStudent(int id) {
        Session session = sessionFactory.getCurrentSession();
        Student student = session.get(Student.class, id);
        if (student != null) {
            session.delete(student);
        }
    }
}
```
✅ **Why?**  
- Uses **`SessionFactory`** for database operations  
- Annotated with **`@Transactional`** for automatic transaction management  
- Uses **HQL** instead of raw SQL  

---

## **🔹 Step 6: Create `Main.java` to Test Hibernate Operations**
```java
package com.example;

import com.example.config.SpringConfig;
import com.example.dao.StudentDAO;
import com.example.model.Student;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import java.util.List;

public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);
        StudentDAO studentDAO = context.getBean(StudentDAO.class);

        // Save Student
        System.out.println("Saving Student...");
        studentDAO.saveStudent(new Student("Alice", "Brown", "alice@example.com"));

        // Fetch All Students
        System.out.println("Fetching Students...");
        List<Student> students = studentDAO.getAllStudents();
        students.forEach(s -> System.out.println(s.getId() + " " + s.getFirstName() + " " + s.getEmail()));

        // Update Student
        System.out.println("Updating Student Email...");
        studentDAO.updateStudent(1, "alice.updated@example.com");

        // Delete Student
        System.out.println("Deleting Student...");
        studentDAO.deleteStudent(1);

        context.close();
    }
}
```
✅ **Expected Output**
```
Saving Student...
Fetching Students...
1 Alice alice@example.com
Updating Student Email...
Deleting Student...
```

---

# **📌 Key Takeaways: Why Hibernate is Better than JDBC**
| **JDBC Limitations** | **Hibernate Solution** |
|----------------------|-----------------------|
| **Manual Connection Handling** | Spring manages connections automatically |
| **Hardcoded SQL Queries** | Uses **HQL** for database-agnostic queries |
| **No Caching** | First-level and second-level caching available |
| **Manual Object Mapping** | Uses **Entity annotations** for automatic mapping |
| **No Built-in Transaction Management** | Spring provides **`@Transactional`** |

---

# **🚀 Next Step: Transition to JPA**
Now that we've explored **Spring with Hibernate**, let's move to **JPA**, where configuration is even simpler and managed by **Spring Data JPA**.

👉 **Are you ready to start with JPA?** 😊