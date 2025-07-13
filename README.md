## 🎯 1. Main Project (PBL Context)

In BookMart:
- If a user provides an invalid ID or unauthorized action → we must show a meaningful **error message**
- If an internal error occurs (e.g., DB fails) → we should **log** it but not expose internal stack trace to the frontend

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Uncaught exceptions break the API or return generic 500 errors. We want to **handle errors centrally**, log them, and send **structured, user-friendly JSON responses**.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Use `@ControllerAdvice` and `@ExceptionHandler`
- ✅ Return custom JSON error responses with message, status, timestamp
- ✅ Use SLF4J (`LoggerFactory`) for structured logging
- ✅ Wrap risky service logic in try-catch and throw custom exceptions

---

## 🧠 4. Scenario-Based Framing

> Imagine:
- A buyer requests a book ID that doesn’t exist → we must return `404` with a clear message: "Book not found"
- A DB error occurs → log the stack trace, return a friendly message: "Internal server error. Please try again."

---

## 🗺️ 5. Mini Visual Roadmap

```text
🔍 Service Layer → try/catch → throw CustomException
↓
🎯 @ControllerAdvice catches exception
↓
🛡️ Structured JSON returned → errorCode, message, timestamp
↓
📝 Logger logs full stack trace internally
````

---

## 📚 6. Conceptual Explanation

### 📌 Why Use Centralized Error Handling?

* Avoids duplicating try-catch in controllers
* Ensures consistent response structure
* Separates concerns (handling vs business logic)
* Helps in auditing and debugging via logging

---

### 📦 Error Response Format (Standardized JSON)

```json
{
  "timestamp": "2025-07-12T12:34:56",
  "errorCode": "BOOK_NOT_FOUND",
  "message": "Book with ID 42 not found"
}
```

---

## 💻 7. Hands-On Implementation

---

### ✅ Step 1: Create Custom Exception Class

```java
public class BookNotFoundException extends RuntimeException {
    public BookNotFoundException(String message) {
        super(message);
    }
}
```

---

### ✅ Step 2: Create Global Exception Handler

```java
@ControllerAdvice
public class GlobalExceptionHandler {

    private static final Logger logger = LoggerFactory.getLogger(GlobalExceptionHandler.class);

    @ExceptionHandler(BookNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleBookNotFound(BookNotFoundException ex) {
        logger.warn("Book not found: {}", ex.getMessage());
        ErrorResponse response = new ErrorResponse("BOOK_NOT_FOUND", ex.getMessage());
        return new ResponseEntity<>(response, HttpStatus.NOT_FOUND);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGeneric(Exception ex) {
        logger.error("Internal error occurred", ex);
        ErrorResponse response = new ErrorResponse("INTERNAL_ERROR", "Something went wrong. Try again later.");
        return new ResponseEntity<>(response, HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

---

### ✅ Step 3: Define Error Response DTO

```java
public class ErrorResponse {
    private String errorCode;
    private String message;
    private LocalDateTime timestamp = LocalDateTime.now();

    public ErrorResponse(String errorCode, String message) {
        this.errorCode = errorCode;
        this.message = message;
    }

    // Getters and setters
}
```

---

### ✅ Step 4: Update Service Layer with try-catch

```java
@Service
public class BookServiceImpl implements BookService {

    private static final Logger logger = LoggerFactory.getLogger(BookServiceImpl.class);

    @Override
    public Book getBookById(int id) {
        try {
            return repository.findById(id)
                .orElseThrow(() -> new BookNotFoundException("Book with ID " + id + " not found"));
        } catch (BookNotFoundException ex) {
            logger.warn("Book lookup failed: {}", ex.getMessage());
            throw ex;
        } catch (Exception e) {
            logger.error("Unexpected error in getBookById", e);
            throw new RuntimeException("Database error");
        }
    }
}
```

---

## 📤 8. Output-Based Assessment

| ✅ Task                           | 💬 Expected Outcome                           |
| -------------------------------- | --------------------------------------------- |
| Custom exceptions created        | Extends `RuntimeException`                    |
| @ControllerAdvice handles errors | Errors return JSON, not HTML                  |
| SLF4J logs stack traces          | Logs to console/file with structured output   |
| 404 for missing book ID          | With message: `Book with ID X not found`      |
| GitHub push                      | `feature/central-error-logging` branch pushed |

---

## 🎯 9. Interview Preparation

### Q1. What is `@ControllerAdvice` used for?

> It handles exceptions globally across all controllers, avoiding duplicated try-catch blocks.

### Q2. How do you structure custom error responses?

> By creating a DTO with fields like errorCode, message, and timestamp.

### Q3. What is SLF4J and why is it used?

> A logging abstraction that allows you to log messages across frameworks using a consistent API.

### Q4. Why use try-catch in service and not just controller?

> Service layers deal with logic where exceptions may occur (e.g., DB access) and are the right place to catch and rethrow domain-specific errors.

---

## 🔄 10. Connection to the Next Problem Statement

Now that we’ve set up centralized error handling and logging...

Next session will focus on:

* Writing **unit and integration tests** for error scenarios
* Mocking services to simulate exceptions
* Verifying correct HTTP status & error JSON

---

## ✅ Next Topic:

### Session 26 → Testing Error Handling using JUnit + MockMvc in Spring Boot

