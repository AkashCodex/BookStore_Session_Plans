# 📘 Session 2: Handling Requests, Business Logic & Data Layer in Spring Boot

> **Project Context:** BookMart – Building the core backend workflow: controller → service → repository with DB integration, validation, and error handling.

---

## 🎯 1. Main Project (PBL Context)

In this session, we’ll build core backend functionality for **BookMart** using the three-tier architecture:
- Controller to handle HTTP requests,
- Service for business logic,
- Repository for database interaction using **Spring Data JPA**.

We’ll also explore **request validation**, **authentication concepts**, and **exception handling**.

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Implement a working backend workflow for managing books using RESTful principles. Understand how to receive client requests, process them via business logic, interact with the database, and handle validation and exceptions gracefully.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Understand the flow of request → controller → service → repository
- ✅ Use Spring Data JPA to interact with a relational database
- ✅ Implement basic validation for incoming data
- ✅ Handle common exceptions using `@ControllerAdvice`
- ✅ Understand Client-Server Architecture in Spring Boot

---

## 🧠 4. Scenario-Based Framing

> A seller wants to add a new book to BookMart.

- Frontend sends a POST request with book data.
- Backend must validate this data.
- Store the book in the database.
- Return success or error messages.

This is the **heart of backend processing**: receiving, validating, processing, persisting, and responding.

---

## 🗺️ 5. Mini Visual Roadmap

```text
Client (Frontend)
   ↓
[Controller]  → Validates request
   ↓
[Service Layer] → Applies business rules
   ↓
[Repository] → Saves to DB (via Spring Data JPA)
   ↓
Returns response or error
````

Additional layers:

* ✅ Validation using `@Valid` and `@NotBlank`
* ✅ Exception Handling using `@ControllerAdvice`
* ✅ Status codes via `ResponseEntity`

---

## 📚 6. Conceptual Explanation

### 🌐 HTTP Request Lifecycle

1. **Controller**: Maps endpoints (GET, POST, etc.)
2. **Service**: Contains the logic to process the request
3. **Repository**: Interfaces with the DB using Spring Data JPA

### 🧠 Spring Data JPA

* Interface-based DB access
* Uses method naming convention to auto-generate queries

```java
@Repository
public interface BookRepository extends JpaRepository<Book, Integer> {
    Optional<Book> findByTitle(String title);
}
```

### 🛡️ Validation Example

```java
public class BookDto {
    @NotBlank(message = "Title cannot be empty")
    private String title;

    @Min(value = 1, message = "Price must be positive")
    private double price;
}
```

Use `@Valid` in Controller:

```java
@PostMapping("/books")
public ResponseEntity<String> addBook(@Valid @RequestBody BookDto bookDto) {
    ...
}
```

### ❗ Exception Handling Example

```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<String> handleValidationErrors(MethodArgumentNotValidException ex) {
        return ResponseEntity.badRequest().body("Validation Failed: " +
            ex.getBindingResult().getFieldError().getDefaultMessage());
    }
}
```

---

## 💻 7. Hands-On Implementation (BookMart Feature)

### 📦 Packages:

```
com.bookmart
├── controller → BookController.java
├── dto → BookDto.java
├── entity → Book.java
├── repository → BookRepository.java
├── service → BookService.java / BookServiceImpl.java
└── exception → GlobalExceptionHandler.java
```

### ✅ Tasks:

1. Create `Book` Entity

```java
@Entity
public class Book {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    private String title;
    private double price;
}
```

2. Create `BookDto`

```java
public class BookDto {
    @NotBlank(message = "Title required")
    private String title;

    @Min(1)
    private double price;
}
```

3. Create `BookRepository`:

```java
@Repository
public interface BookRepository extends JpaRepository<Book, Integer> {
}
```

4. Implement `BookServiceImpl`:

```java
@Service
public class BookServiceImpl implements BookService {
    @Autowired
    private BookRepository bookRepository;

    @Override
    public Book saveBook(Book book) {
        return bookRepository.save(book);
    }
}
```

5. BookController (POST + GET):

```java
@RestController
@RequestMapping("/api/books")
public class BookController {

    @Autowired
    private BookService bookService;

    @PostMapping
    public ResponseEntity<Book> createBook(@Valid @RequestBody BookDto bookDto) {
        Book book = new Book();
        book.setTitle(bookDto.getTitle());
        book.setPrice(bookDto.getPrice());
        return new ResponseEntity<>(bookService.saveBook(book), HttpStatus.CREATED);
    }

    @GetMapping
    public ResponseEntity<List<Book>> getBooks() {
        return new ResponseEntity<>(bookService.getAllBooks(), HttpStatus.OK);
    }
}
```

6. Add `GlobalExceptionHandler` class

---

## 📤 8. Output-Based Assessment

| ✅ Task                              | 💬 Expected Outcome         |
| ----------------------------------- | --------------------------- |
| REST endpoints `/api/books` created | Handles GET & POST          |
| Validation using `@Valid`           | Returns errors on bad input |
| DB interaction using JPA            | Books persist in DB         |
| Global exception handler works      | Meaningful error messages   |
| Folder/package structure clean      | Modular & layered design    |
| Pushed to GitHub                    | `feature/book-crud`         |

---

## 🎯 9. Interview Preparation

### Q1. What is the role of the Controller?

> It maps HTTP requests to handler methods and returns the response.

### Q2. What is Spring Data JPA?

> It provides easy CRUD operations using interface-based repositories with minimal boilerplate.

### Q3. How does Spring handle validation?

> Using annotations like `@Valid`, `@NotBlank`, `@Min` and exception handling via `@ControllerAdvice`.

### Q4. What is Client-Server architecture?

> The frontend (client) sends requests to the backend (server) which processes and sends back a response.

---

## 🔄 10. Connection to Next Problem Statement

In the next session, we’ll deep-dive into **Spring Core concepts** that power this entire architecture behind the scenes — specifically **IoC (Inversion of Control)**, **Beans**, **Component Scanning**, and **Loose Coupling** using interfaces and `@Autowired`.

---

## ✅ Next Topic:

### Session 3 → Spring Core: IoC, Beans, Autowiring, and Loose Coupling in BookMart

