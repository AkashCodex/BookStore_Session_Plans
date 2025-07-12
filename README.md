# 📘 Session 15: ORM Fundamentals & Entity Modeling with Spring Data JPA

> **Project Context:** BookMart – We now need to **persist our data**. To store books, users, and orders in a database efficiently, we must understand **ORM** (Object-Relational Mapping) and use **Spring Data JPA + Hibernate**.

---

## 🎯 1. Main Project (PBL Context)

BookMart allows Sellers to manage books and Buyers to browse and purchase them.  
Now, we’ll **store** and **retrieve** this data from a **relational database** using:
- **Entity classes**
- **Spring Data JPA repositories**
- **Hibernate** as the ORM provider

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Introduce the concept of ORM, differentiate between JDBC, JPA, and Hibernate, and configure the Spring Boot project to use Spring Data JPA with Hibernate and MySQL/PostgreSQL.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Understand what ORM is and why it is used
- ✅ Differentiate between JDBC, JPA, and Hibernate
- ✅ Define Entity classes in Spring Boot
- ✅ Setup Spring Data JPA & Hibernate with Spring Boot
- ✅ Configure database settings in `application.properties`
- ✅ Create repository interfaces for database access

---

## 🧠 4. Scenario-Based Framing

> Previously we used mock data (Java Lists). Now we want to **persist books** in a real database.
> 
> A seller creates a new book listing via POST `/api/books`. The book is:
- Converted to a Book entity
- Saved using `BookRepository`
- Mapped via Hibernate ORM
- Stored in the database automatically!

---

## 🗺️ 5. Mini Visual Roadmap

```text
📦 DTO → Book Entity
  ↓
📂 Spring Data JPA Repo (BookRepository)
  ↓
📚 Hibernate ORM maps Entity → Table
  ↓
🗃️ MySQL/Postgres stores the data
````

---

## 📚 6. Conceptual Explanation

### 🔧 What is ORM?

> **Object-Relational Mapping** is a technique that maps Java objects to database tables, making DB operations easier and more maintainable.

### 🔍 JDBC vs JPA vs Hibernate

| Feature      | JDBC      | JPA (Java API)        | Hibernate (JPA Implementation) |
| ------------ | --------- | --------------------- | ------------------------------ |
| Level        | Low-level | Specification/API     | Implementation of JPA          |
| Manual SQL   | Yes       | No                    | No                             |
| Auto Mapping | No        | Yes (via annotations) | Yes                            |
| Boilerplate  | More      | Less                  | Minimal                        |

### 🧰 Why Spring Data JPA?

* Auto-implements common DB operations (CRUD, paging, sorting)
* Uses interfaces instead of boilerplate queries
* Integrates Hibernate under the hood

---

## 💻 7. Hands-On Implementation

### ✅ Step 1: Add Dependencies in `pom.xml`

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <dependency>
        <groupId>mysql</groupId> <!-- Or postgresql -->
        <artifactId>mysql-connector-j</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

---

### ✅ Step 2: Configure `application.properties`

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/bookmart
spring.datasource.username=root
spring.datasource.password=root123

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

spring.jpa.database-platform=org.hibernate.dialect.MySQL8Dialect
```

> 💡 Use `ddl-auto=update` for dev mode, `validate` or `none` for prod.

---

### ✅ Step 3: Create Entity Class

```java
@Entity
@Table(name = "books")
public class Book {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    private String title;
    private String author;
    private Double price;
    private String category;

    // Constructors, Getters, Setters
}
```

---

### ✅ Step 4: Create Repository Interface

```java
@Repository
public interface BookRepository extends JpaRepository<Book, Integer> {
    List<Book> findByCategory(String category);
}
```

---

### ✅ Step 5: Use in Service

```java
@Service
public class BookServiceImpl implements BookService {

    @Autowired
    private BookRepository bookRepository;

    @Override
    public Book addBook(Book book) {
        return bookRepository.save(book);
    }
}
```

---

## 📤 8. Output-Based Assessment

| ✅ Task                          | 💬 Expected Outcome              |
| ------------------------------- | -------------------------------- |
| Project connects to DB          | Console shows Hibernate SQL logs |
| Entity mapped to table          | `books` table auto-created       |
| BookRepository saves/reads data | Book added and fetched           |
| GitHub push complete            | `feature/jpa-entity-modeling`    |

---

## 🎯 9. Interview Preparation

### Q1. What is ORM?

> It maps Java objects to database tables, enabling DB operations through object manipulation.

### Q2. Difference between JPA and Hibernate?

> JPA is the API/spec; Hibernate is a concrete implementation.

### Q3. How does Spring Boot auto-configure JPA?

> Via `spring-boot-starter-data-jpa`, it scans for `@Entity`, configures Hibernate, and creates schema.

### Q4. What does `@Entity` do?

> Marks a POJO as a database entity that should be mapped to a table.

---

## 🔄 10. Connection to the Next Problem Statement

Now that our entities and database setup are ready, the next step is to:

* Build complete **CRUD APIs**
* Add **Pagination**, **Sorting**, and **Filtering** for Book listings

---

## ✅ Next Topic:

### Session 16 → CRUD + Pagination, Sorting, and Filtering using Spring Data JPA in BookMart

