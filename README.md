# 📘 Session 18: Custom Queries, Validation & Schema Design Best Practices

> **Project Context:** BookMart – As our system grows, we need more control over queries, validation of user inputs, and thoughtful schema design for performance and clarity.

---

## 🎯 1. Main Project (PBL Context)

We now support basic CRUD using `JpaRepository`.  
In this session, we:

- Use `@Query` to write **JPQL or native SQL queries**
- Enforce **input validation** at the DTO layer
- Apply **database schema best practices** for long-term maintainability and scalability

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Developers must often go beyond auto-implemented repository methods. We’ll implement advanced queries with `@Query`, secure input validation using annotations, and follow schema standards to ensure performance and consistency.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Write custom JPQL and native SQL queries using `@Query`
- ✅ Understand the difference between JPQL and native SQL
- ✅ Use validation annotations like `@NotNull`, `@Size`, `@Pattern`
- ✅ Apply `@Valid` at the controller level
- ✅ Recognize schema design best practices (naming, indexing, normalization)

---

## 🧠 4. Scenario-Based Framing

> Imagine BookMart allows search by title patterns or price ranges:
- `findBooksByTitleContaining(String keyword)`
- `getTop5BooksByPriceDesc()`

We also want to validate incoming data:
- Title should not be empty
- Price must be positive
- ISBN must match a regex pattern

Without this, we risk saving **incomplete or incorrect** data.

---

## 🗺️ 5. Mini Visual Roadmap

```text
📦 Controller
  ↓ @Valid + BookDto (with @Size, @NotNull)
🧠 BookService
  ↓ BookRepository → @Query (JPQL/SQL)
🗃️ DB: schema follows naming/index/normalization rules
````

---

## 📚 6. Conceptual Explanation

### 🧩 Custom Queries with `@Query`

* **JPQL (Java Persistence Query Language)**: Entity-based (not table-based)
* **Native SQL**: Raw SQL, for performance or DB-specific syntax

```java
@Query("SELECT b FROM Book b WHERE b.title LIKE %:keyword%")
List<Book> searchByTitleKeyword(@Param("keyword") String keyword);

@Query(value = "SELECT * FROM books WHERE price > :price", nativeQuery = true)
List<Book> findBooksCostlierThan(@Param("price") double price);
```

---

### 🔐 Input Validation

| Annotation        | Purpose                                  |
| ----------------- | ---------------------------------------- |
| `@NotNull`        | Field cannot be null                     |
| `@Size(min, max)` | Validates string/collection size         |
| `@Min`, `@Max`    | Numeric range                            |
| `@Pattern`        | Regex validation                         |
| `@Email`          | Must be a valid email format             |
| `@Valid`          | Triggers validation for nested/DTO input |

---

### 🧱 Schema Design Best Practices

| Concept                | Best Practice Example                       |
| ---------------------- | ------------------------------------------- |
| Naming Conventions     | snake\_case for tables/columns (`book_id`)  |
| Indexing               | Add indexes on `title`, `category`, `price` |
| Primary Key Convention | Always use surrogate key (`id` AUTO\_GEN)   |
| Normalization          | Separate entities (Book, User, Order)       |
| Denormalization (when) | Use for read-heavy & fast retrieval needs   |

---

## 💻 7. Hands-On Implementation

### ✅ Step 1: Custom JPQL Query in BookRepository

```java
@Query("SELECT b FROM Book b WHERE b.category = :cat AND b.price < :price")
List<Book> findBooksByCategoryAndPriceLessThan(@Param("cat") String category, @Param("price") double price);
```

---

### ✅ Step 2: Native SQL Query

```java
@Query(value = "SELECT * FROM books ORDER BY price DESC LIMIT 5", nativeQuery = true)
List<Book> getTop5ExpensiveBooks();
```

---

### ✅ Step 3: Validation in DTO

```java
public class BookDto {

    private Integer id;

    @NotNull(message = "Title cannot be null")
    @Size(min = 3, message = "Title must be at least 3 characters")
    private String title;

    @Min(value = 0, message = "Price must be positive")
    private double price;

    @NotBlank
    private String category;

    @Pattern(regexp = "\\d{3}-\\d{10}", message = "ISBN format is invalid")
    private String isbn;

    private Integer ownerId;
}
```

---

### ✅ Step 4: Use `@Valid` in Controller

```java
@PostMapping
public ResponseEntity<BookDto> createBook(@RequestBody @Valid BookDto dto) {
    Book book = bookService.save(dto);
    return new ResponseEntity<>(convertToDto(book), HttpStatus.CREATED);
}
```

---

### ✅ Step 5: Add Indexes (PostgreSQL example)

```sql
CREATE INDEX idx_books_title ON books(title);
CREATE INDEX idx_books_category_price ON books(category, price);
```

---

## 📤 8. Output-Based Assessment

| ✅ Task                             | 💬 Expected Outcome                      |
| ---------------------------------- | ---------------------------------------- |
| Custom query returns expected data | Filtered results using JPQL/SQL          |
| Invalid input rejected             | Returns 400 with field-level errors      |
| Index improves query speed         | Explain plan shows index usage           |
| GitHub push complete               | `feature/custom-query-validation-schema` |

---

## 🎯 9. Interview Preparation

### Q1. What is the difference between JPQL and native SQL?

> JPQL uses entity names/fields; SQL uses raw table and column names.

### Q2. What is `@Valid` used for?

> It triggers validation logic for a method parameter (usually a DTO).

### Q3. Why is indexing important in DB schema?

> It speeds up SELECT queries by avoiding full table scans.

### Q4. When should we denormalize?

> When performance is critical and data redundancy is acceptable (e.g., read-optimized systems).

---

## 🔄 10. Connection to the Next Problem Statement

We’ve now:

* Mastered Spring Data's auto and custom query capabilities
* Validated data entry using annotations
* Applied schema best practices

Next, we build:

* **Pagination**, **Sorting**, and **Filtering** APIs with `Pageable`

---

## ✅ Next Topic:

### Session 19 → Implementing Pagination, Sorting & Filtering APIs with Spring Data
