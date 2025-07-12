# 📘 Session 4: Building RESTful APIs with Spring Boot

> **Project Context:** BookMart – Designing and implementing RESTful APIs for managing book resources using Spring Boot's powerful HTTP method mappings.

---

## 🎯 1. Main Project (PBL Context)

In this session, we’ll learn how to build **RESTful APIs** for BookMart.  
These APIs enable Buyers and Sellers to interact with the system using standard HTTP methods such as **GET**, **POST**, **PUT**, **DELETE**, and **PATCH**.

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Implement CRUD operations for Book resources using RESTful APIs.  
Understand REST principles and how to use Spring Boot’s annotations to build scalable, stateless, and resource-based services.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Define what a RESTful API is
- ✅ Map CRUD operations to HTTP methods
- ✅ Build REST APIs using Spring Boot (`@GetMapping`, `@PostMapping`, etc.)
- ✅ Apply REST design principles (stateless, resource-based, URI naming)
- ✅ Return appropriate HTTP status codes and responses

---

## 🧠 4. Scenario-Based Framing

> A seller wants to:
> - Add a new book to BookMart (POST)
> - View all their books (GET)
> - Update book details (PUT/PATCH)
> - Remove a book (DELETE)

Each of these actions corresponds to an **HTTP method**.  
By designing RESTful endpoints, we ensure a **standardized, predictable, and scalable API** structure.

---

## 🗺️ 5. Mini Visual Roadmap

| Operation         | HTTP Method | Endpoint               |
|------------------|-------------|------------------------|
| Get all books     | GET         | `/api/books`           |
| Get one book      | GET         | `/api/books/{id}`      |
| Create book       | POST        | `/api/books`           |
| Update book       | PUT         | `/api/books/{id}`      |
| Partial update    | PATCH       | `/api/books/{id}`      |
| Delete book       | DELETE      | `/api/books/{id}`      |

---

## 📚 6. Conceptual Explanation

### 📦 What is a RESTful API?

- REST stands for **REpresentational State Transfer**
- RESTful APIs follow standard architectural principles:
  - **Stateless**: No session data stored on the server
  - **Resource-based**: Everything is treated as a resource (books, users, orders)
  - **Standard URIs**: `/api/books`, `/api/users/10`
  - **HTTP methods**: Used to perform actions on resources

### 🌐 HTTP Method Overview

| Method | Purpose              | Use Case Example            |
|--------|----------------------|-----------------------------|
| GET    | Read data            | Get all books               |
| POST   | Create new resource  | Add a new book              |
| PUT    | Update full resource | Replace book with new data  |
| PATCH  | Partial update       | Change only book price      |
| DELETE | Remove resource      | Delete a book by ID         |

---

## 💻 7. Hands-On Implementation (BookMart API)

### 📦 Package Structure:
(Reuse existing)

```

com.bookmart
├── controller → BookController.java
├── dto → BookDto.java
├── entity → Book.java
├── repository → BookRepository.java
├── service → BookService.java / Impl
└── exception → GlobalExceptionHandler.java

````

### ✅ Controller Implementation

```java
@RestController
@RequestMapping("/api/books")
public class BookController {

    @Autowired
    private BookService bookService;

    @GetMapping
    public ResponseEntity<List<Book>> getAllBooks() {
        return ResponseEntity.ok(bookService.getAllBooks());
    }

    @GetMapping("/{id}")
    public ResponseEntity<Book> getBookById(@PathVariable int id) {
        return ResponseEntity.ok(bookService.getBookById(id));
    }

    @PostMapping
    public ResponseEntity<Book> createBook(@Valid @RequestBody BookDto bookDto) {
        return new ResponseEntity<>(bookService.createBook(bookDto), HttpStatus.CREATED);
    }

    @PutMapping("/{id}")
    public ResponseEntity<Book> updateBook(@PathVariable int id, @Valid @RequestBody BookDto bookDto) {
        return ResponseEntity.ok(bookService.updateBook(id, bookDto));
    }

    @PatchMapping("/{id}")
    public ResponseEntity<Book> patchBook(@PathVariable int id, @RequestBody Map<String, Object> updates) {
        return ResponseEntity.ok(bookService.patchBook(id, updates));
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<String> deleteBook(@PathVariable int id) {
        bookService.deleteBook(id);
        return ResponseEntity.ok("Book deleted successfully");
    }
}
````

---

## 📤 8. Output-Based Assessment

| ✅ Task                                 | 💬 Expected Outcome                     |
| -------------------------------------- | --------------------------------------- |
| All HTTP methods implemented           | CRUD operations via API                 |
| URI structure is RESTful               | Follows `/api/books`, `/api/books/{id}` |
| Appropriate response codes used        | `200`, `201`, `404`, `400`              |
| Book data flows from DTO → Entity → DB | Structured & clean                      |
| Pushed to GitHub                       | `feature/book-rest-api`                 |

---

## 🎯 9. Interview Preparation

### Q1. What is a RESTful API?

> A RESTful API follows REST principles using HTTP methods to interact with resources like books or users.

### Q2. What is the difference between PUT and PATCH?

> PUT replaces the whole resource; PATCH updates part of it.

### Q3. Why is REST considered stateless?

> Each request is independent; the server does not store session information.

### Q4. What are best practices for REST endpoint design?

> Use nouns, not verbs in URIs. Example: `/api/books`, not `/api/getBooks`

---

## 🔄 10. Connection to the Next Problem Statement

Now that we’ve built REST APIs for the BookMart backend, the next step is to learn **DTO (Data Transfer Object) mapping**, and how to **transform between Entities and DTOs**, ensuring frontend-friendly responses while hiding internal models.

---

## ✅ Next Topic:

### Session 5 → DTO Mapping, Model-Mapper, and Entity-DTO Conversion in BookMart

