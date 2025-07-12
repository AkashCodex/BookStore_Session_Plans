# 📘 Session 13: Centralized Error Handling & Logging in Spring Boot

> **Project Context:** BookMart – As the application grows, consistent and clear error responses become critical. We will implement a centralized error handler and introduce production-grade logging using SLF4J.


## 🎯 1. Main Project (PBL Context)

BookMart now handles user login, book management, and role-based access.  
But how should the system respond to errors like:
- Invalid input?
- Missing book?
- Authentication failure?

This session ensures:
- Consistent error response format (status, message, timestamp)
- Centralized error handler using `@ControllerAdvice`
- Logging every major error/action in production format

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Create a centralized way to handle all exceptions using `@ControllerAdvice`, return meaningful JSON error responses with metadata, and log errors with timestamps and traceability.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Understand why centralized error handling is important
- ✅ Implement `@ControllerAdvice` with `@ExceptionHandler`
- ✅ Return structured error response: timestamp, status, message, path
- ✅ Log key actions and errors using `SLF4J` (`@Slf4j` or `Logger`)
- ✅ Separate client-friendly vs internal error messages

---

## 🧠 4. Scenario-Based Framing

> A Buyer requests a non-existent book ID → API returns:
```json
{
  "timestamp": "2025-07-12T10:22:00",
  "status": 404,
  "error": "Not Found",
  "message": "Book not found with ID: 999",
  "path": "/api/books/999"
}
````

Instead of a stack trace or generic server error, the response is:

* Consistent
* JSON formatted
* Developer-friendly for frontend integration
* Logged for backend audit

---

## 🗺️ 5. Mini Visual Roadmap

```text
📦 Exception Thrown (e.g., BookNotFoundException)
   ↓
🎯 GlobalExceptionHandler (@ControllerAdvice)
   ↓
🧾 Standardized ErrorResponse returned
   ↓
📑 Logged with timestamp, path, error
```

---

## 📚 6. Conceptual Explanation

### 📘 What is `@ControllerAdvice`?

A special component in Spring that allows:

* Centralized handling of all controller-level exceptions
* Reusable logic for all REST controllers

### 📘 What is `@ExceptionHandler`?

Used within `@ControllerAdvice` to define specific methods to handle custom exceptions.

### 📘 Logging Levels (via SLF4J)

| Level   | When to Use                      |
| ------- | -------------------------------- |
| `info`  | Startup, successful ops          |
| `warn`  | Non-critical issues (deprecated) |
| `error` | Exceptions, failures             |

---

## 💻 7. Hands-On Implementation

### ✅ Create ErrorResponse.java

```java
public class ErrorResponse {
    private LocalDateTime timestamp;
    private int status;
    private String error;
    private String message;
    private String path;

    // Constructors, Getters & Setters
}
```

---

### ✅ Custom Exception: BookNotFoundException.java

```java
public class BookNotFoundException extends RuntimeException {
    public BookNotFoundException(String message) {
        super(message);
    }
}
```

---

### ✅ GlobalExceptionHandler.java

```java
@RestControllerAdvice
@Slf4j
public class GlobalExceptionHandler {

    @ExceptionHandler(BookNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleBookNotFound(BookNotFoundException ex, HttpServletRequest request) {
        ErrorResponse err = new ErrorResponse(
            LocalDateTime.now(),
            HttpStatus.NOT_FOUND.value(),
            "Not Found",
            ex.getMessage(),
            request.getRequestURI()
        );

        log.error("BookNotFoundException: {}", ex.getMessage());
        return new ResponseEntity<>(err, HttpStatus.NOT_FOUND);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGeneric(Exception ex, HttpServletRequest request) {
        ErrorResponse err = new ErrorResponse(
            LocalDateTime.now(),
            HttpStatus.INTERNAL_SERVER_ERROR.value(),
            "Internal Server Error",
            "An unexpected error occurred",
            request.getRequestURI()
        );

        log.error("Unhandled Exception: {}", ex.getMessage(), ex);
        return new ResponseEntity<>(err, HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

---

### ✅ Example in BookController.java

```java
@GetMapping("/{id}")
public ResponseEntity<BookDto> getBookById(@PathVariable int id) {
    BookDto book = bookService.getBookById(id)
        .orElseThrow(() -> new BookNotFoundException("Book not found with ID: " + id));
    return ResponseEntity.ok(book);
}
```

---

### ✅ Logging Format in `logback-spring.xml` (optional for advanced setup)

```xml
<pattern>
  %d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n
</pattern>
```

---

## 📤 8. Output-Based Assessment

| ✅ Task                            | 💬 Expected Outcome               |
| --------------------------------- | --------------------------------- |
| Error response structured         | JSON with status, timestamp, path |
| BookNotFoundException returns 404 | With message and URI              |
| All exceptions logged with SLF4J  | Shown in console or logs          |
| GitHub push complete              | `feature/error-handling-logging`  |

---

## 🎯 9. Interview Preparation

### Q1. What is `@ControllerAdvice`?

> A Spring component that handles exceptions globally for all controllers.

### Q2. Why centralize error handling?

> Avoids repetition, ensures consistent response structure, and simplifies debugging.

### Q3. What should an API error response include?

> Timestamp, status, message, request path.

### Q4. How do you log exceptions in Spring Boot?

> Use SLF4J (`@Slf4j`) and log at `error` or `warn` level depending on severity.

---

## 🔄 10. Connection to the Next Problem Statement

With strong error handling and logging in place, we’re ready to build a production-grade backend. Next, we'll move to:

* **Pagination**
* **Sorting**
* **Filtering**
  …all using Spring Data JPA — enabling scalable data access for large book collections.

---

## ✅ Next Topic:

### Session 14 → Full CRUD + Pagination, Sorting & Filtering using Spring Data JPA in BookMart

