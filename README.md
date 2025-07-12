# 📘 Session 17: Spring Data Repositories with JPA in BookMart

> **Project Context:** BookMart – Now that we've defined our entities and mapped them to a PostgreSQL database, we will use **Spring Data JPA repositories** to access and manipulate data using **predefined and custom query methods**.

---

## 🎯 1. Main Project (PBL Context)

BookMart includes entities like `User`, `Book`, and `Order`.  
These need robust backend CRUD functionality. Instead of writing SQL or boilerplate DAO code, Spring Data provides **out-of-the-box repository interfaces**.

Today we:
- Use `JpaRepository` and `CrudRepository`
- Implement data access logic for Books, Users, and Orders
- Leverage query method naming conventions like `findByCategory()`

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Implement persistent storage for all entities using Spring Data JPA repositories and build essential CRUD operations using auto-implemented query methods like `findById`, `save`, `deleteById`, and `existsById`.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Understand what `CrudRepository`, `JpaRepository` offer
- ✅ Create interfaces for `BookRepository`, `UserRepository`, `OrderRepository`
- ✅ Use built-in methods like `save()`, `deleteById()`, `findAll()`, `existsById()`
- ✅ Write custom query methods using naming conventions
- ✅ Use repository methods inside service classes cleanly

---

## 🧠 4. Scenario-Based Framing

> In BookMart, a Seller wants to:
- List all their books → `findByOwnerId(Integer id)`
- Delete a book → `deleteById(Integer bookId)`
- Check if a book exists before updating → `existsById(Integer id)`

With Spring Data JPA, all of this can be done with **no SQL** or **manual DAO**.

---

## 🗺️ 5. Mini Visual Roadmap

```text
📦 BookService
  ↓
🧠 BookRepository extends JpaRepository
  ↓
🔄 Auto-query methods → DB via Hibernate
  ↓
📚 PostgreSQL stores + retrieves entities
````

---

## 📚 6. Conceptual Explanation

### 📘 CrudRepository vs JpaRepository

| Interface        | Features                                |
| ---------------- | --------------------------------------- |
| `CrudRepository` | Basic CRUD methods (`save`, `findById`) |
| `JpaRepository`  | CRUD + Pagination + Sorting + Flush     |

> Use `JpaRepository` in most Spring Boot apps.

---

### 🛠️ Common Auto-Implemented Methods

| Method           | Purpose                |
| ---------------- | ---------------------- |
| `findById(id)`   | Fetch one entity by ID |
| `findAll()`      | Get all records        |
| `save(entity)`   | Insert or update       |
| `deleteById(id)` | Delete by ID           |
| `existsById(id)` | Check if record exists |
| `count()`        | Total record count     |

---

## 💻 7. Hands-On Implementation

### ✅ Step 1: Define BookRepository

```java
@Repository
public interface BookRepository extends JpaRepository<Book, Integer> {

    List<Book> findByCategory(String category);

    List<Book> findByOwnerId(Integer ownerId);

    boolean existsByTitle(String title);
}
```

---

### ✅ Step 2: Define UserRepository

```java
@Repository
public interface UserRepository extends JpaRepository<User, Integer> {

    Optional<User> findByName(String name);

    boolean existsByName(String name);
}
```

---

### ✅ Step 3: Define OrderRepository

```java
@Repository
public interface OrderRepository extends JpaRepository<Order, Integer> {

    List<Order> findByBuyerId(Integer buyerId);

    List<Order> findByBookId(Integer bookId);
}
```

---

### ✅ Step 4: Use in Service Layer

```java
@Service
public class BookServiceImpl implements BookService {

    @Autowired
    private BookRepository bookRepo;

    @Override
    public List<BookDto> getBooksByCategory(String category) {
        List<Book> books = bookRepo.findByCategory(category);
        return books.stream().map(this::convertToDto).toList();
    }

    @Override
    public boolean bookExists(int id) {
        return bookRepo.existsById(id);
    }

    @Override
    public void deleteBook(int id) {
        bookRepo.deleteById(id);
    }

    private BookDto convertToDto(Book book) {
        return new BookDto(book.getId(), book.getTitle(), book.getPrice(), book.getOwner().getId());
    }
}
```

---

## 📤 8. Output-Based Assessment

| ✅ Task                                | 💬 Expected Outcome                    |
| ------------------------------------- | -------------------------------------- |
| `findById`, `save`, `delete` work     | Book saved, fetched, deleted from DB   |
| Custom methods work (`findByOwnerId`) | Returns only seller's books            |
| Exists check works                    | `existsByTitle("Java")` returns `true` |
| GitHub push complete                  | `feature/spring-data-repositories`     |

---

## 🎯 9. Interview Preparation

### Q1. What is `JpaRepository`?

> A Spring Data interface that provides CRUD, pagination, sorting, and query generation for JPA-based persistence.

### Q2. What happens when you call `findById(id)`?

> Spring auto-generates a SQL query using JPA to fetch the record by primary key.

### Q3. What’s the benefit of extending Spring Data repositories?

> No need to write boilerplate code — methods are generated based on method names.

### Q4. How are custom queries created in Spring Data?

> By defining method names like `findByCategory`, Spring parses the name and builds the query automatically.

---

## 🔄 10. Connection to the Next Problem Statement

With repositories in place, we’re now ready to:

* Build full CRUD controllers
* Implement **Pagination**, **Sorting**, and **Filtering**
* Handle **partial updates** with PATCH

---

## ✅ Next Topic:

### Session 18 → Full CRUD + Pagination, Sorting, and Filtering for Book and Order APIs

