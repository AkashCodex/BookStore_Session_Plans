# 📘 Session 16: PostgreSQL Integration, Entity Relationships & DTO Mapping

> **Project Context:** BookMart – With growing entities like Users, Books, and Orders, we must now define **relational mappings** using JPA, connect to a **PostgreSQL** database, and expose DTOs instead of raw entities to the frontend.

---

## 🎯 1. Main Project (PBL Context)

We now have:
- `Book` Entity (owned by a Seller)
- `User` Entity (Buyer or Seller)
- `Order` Entity (placed by Buyer for Book)

In this session:
- We define relationships like `User → Books`, `Order → User`
- Persist data in a **PostgreSQL** DB
- Expose clean API responses via DTOs

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Connect Spring Boot to PostgreSQL, create relational entity mappings using annotations (`@OneToMany`, `@ManyToOne`, etc.), and use DTOs to decouple persistence models from API responses.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Setup PostgreSQL connection in Spring Boot
- ✅ Use `@Entity`, `@Table`, `@Id`, and `@GeneratedValue`
- ✅ Define relationships (`@OneToMany`, `@ManyToOne`, `@ManyToMany`)
- ✅ Understand `mappedBy`, `cascade`, and `fetch`
- ✅ Use DTOs to return safe & clean data to frontend
- ✅ Map Entities ↔ DTO using ModelMapper or manual logic

---

## 🧠 4. Scenario-Based Framing

> A Seller (User) lists multiple books  
> → One-to-Many: One User → Many Books  
>
> A Buyer (User) places many orders  
> → One-to-Many: One User → Many Orders  
>
> Each Order belongs to one Book  
> → Many-to-One: Many Orders → One Book

We also want to expose only clean data — **not full entity graphs** — using **DTOs**.

---

## 🗺️ 5. Mini Visual Roadmap

```text
📦 User Entity
  └── 1:N Books
  └── 1:N Orders
📦 Book Entity
  └── N:1 User
  └── 1:N Orders
📦 Order Entity
  └── N:1 User
  └── N:1 Book
````

---

## 📚 6. Conceptual Explanation

### 🧩 Key JPA Annotations

| Annotation        | Use Case                                |
| ----------------- | --------------------------------------- |
| `@Entity`         | Marks a class as a JPA entity           |
| `@Table`          | Defines the DB table name               |
| `@Id`             | Primary key field                       |
| `@GeneratedValue` | Auto-generates IDs                      |
| `@OneToMany`      | One parent to many children             |
| `@ManyToOne`      | Many children to one parent             |
| `@ManyToMany`     | Bidirectional many-to-many relationship |
| `@OneToOne`       | One-to-one relationship                 |
| `mappedBy`        | Inverse ownership in bidirectional maps |
| `cascade`         | Propagates ops like persist/delete      |
| `fetch`           | Lazy or Eager loading                   |

---

### 🧾 DTO vs Entity

| Entity                          | DTO                       |
| ------------------------------- | ------------------------- |
| Tied to database schema         | Tailored for API response |
| Includes all fields + relations | Includes selected fields  |
| Should not be directly exposed  | Safe to expose externally |

---

## 💻 7. Hands-On Implementation

### ✅ Step 1: PostgreSQL Configuration in `application.properties`

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/bookmart
spring.datasource.username=postgres
spring.datasource.password=postgres123

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
```

---

### ✅ Step 2: User → Book (One-to-Many)

```java
@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    private String name;
    private String role; // BUYER / SELLER

    @OneToMany(mappedBy = "owner", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    private List<Book> books = new ArrayList<>();
}
```

```java
@Entity
public class Book {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    private String title;
    private double price;

    @ManyToOne
    @JoinColumn(name = "owner_id")
    private User owner;
}
```

---

### ✅ Step 3: Buyer (User) → Order (One-to-Many)

### ✅ Book → Order (One-to-Many)

```java
@Entity
public class Order {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    private LocalDate orderDate;

    @ManyToOne
    private User buyer;

    @ManyToOne
    private Book book;
}
```

---

### ✅ Step 4: Create DTOs

```java
public class BookDto {
    private Integer id;
    private String title;
    private double price;
    private Integer ownerId;
}
```

```java
public class OrderDto {
    private Integer id;
    private Integer buyerId;
    private Integer bookId;
    private LocalDate orderDate;
}
```

---

### ✅ Step 5: Use ModelMapper or Manual Mapper

**Option A: Manual**

```java
public BookDto convertToDto(Book book) {
    BookDto dto = new BookDto();
    dto.setId(book.getId());
    dto.setTitle(book.getTitle());
    dto.setPrice(book.getPrice());
    dto.setOwnerId(book.getOwner().getId());
    return dto;
}
```

**Option B: ModelMapper (add dependency)**

```xml
<dependency>
    <groupId>org.modelmapper</groupId>
    <artifactId>modelmapper</artifactId>
    <version>3.1.1</version>
</dependency>
```

```java
@Configuration
public class MapperConfig {
    @Bean
    public ModelMapper modelMapper() {
        return new ModelMapper();
    }
}
```

```java
@Autowired
private ModelMapper mapper;

BookDto dto = mapper.map(book, BookDto.class);
```

---

## 📤 8. Output-Based Assessment

| ✅ Task                       | 💬 Expected Outcome                       |
| ---------------------------- | ----------------------------------------- |
| PostgreSQL connected         | DB starts with Hibernate-generated schema |
| Entity relationships working | Users → Books, Buyers → Orders            |
| DTOs returned from APIs      | Only clean response data (no recursion)   |
| GitHub push complete         | `feature/jpa-relations-dto-mapping`       |

---

## 🎯 9. Interview Preparation

### Q1. What is the purpose of `@OneToMany` and `mappedBy`?

> `@OneToMany` sets up a child collection, and `mappedBy` points to the owning field in the child entity.

### Q2. What are cascade types?

> They determine how related entities behave during operations like save/delete.

### Q3. Why use DTOs instead of exposing Entities?

> To control the response structure, hide sensitive fields, and avoid lazy loading issues.

### Q4. What’s the difference between `EAGER` and `LAZY` fetch types?

> `EAGER` loads relations immediately, `LAZY` defers until accessed.

---

## 🔄 10. Connection to the Next Problem Statement

We now have a working relational database model with PostgreSQL and JPA.
Next, we’ll:

* Build complete **CRUD APIs**
* Implement **Pagination**, **Sorting**, and **Filtering**

---

## ✅ Next Topic:

### Session 17 → Complete CRUD + Pagination, Sorting, and Filtering (Books & Orders)

