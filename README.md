# 📘 Session 5: Building REST Endpoints in Spring Boot

> **Project Context:** BookMart – Implementing real-world REST endpoints using Spring annotations to handle HTTP requests, path variables, and request bodies.

---

## 🎯 1. Main Project (PBL Context)

In BookMart, users interact with the backend by hitting **REST endpoints**.  
These endpoints must accept **parameters** (like book IDs), **request bodies** (like new book data), and return **JSON responses**.

This session focuses on how to properly build those REST APIs using Spring Boot annotations like `@RestController`, `@RequestMapping`, `@GetMapping`, `@PostMapping`, `@PathVariable`, and `@RequestBody`.

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Learn to accept and process HTTP requests using Spring Boot by binding path variables and request bodies into controller methods and returning structured JSON responses.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Understand the role of `@RestController` and request mapping annotations
- ✅ Create REST endpoints using `@GetMapping`, `@PostMapping`, `@PutMapping`, etc.
- ✅ Use `@PathVariable` to accept values from URL
- ✅ Use `@RequestBody` to parse JSON request payloads
- ✅ Return JSON responses using `@ResponseBody` (implicitly via `@RestController`)

---

## 🧠 4. Scenario-Based Framing

> A seller on BookMart wants to:
> - Get a specific book by ID (`GET /books/10`)
> - Add a new book with JSON body (`POST /books`)
> - Update book details using its ID and form data (`PUT /books/10`)

The controller layer must:
- Accept parameters from the URL (ID)
- Parse JSON request payloads into Java objects
- Return valid JSON responses

---

## 🗺️ 5. Mini Visual Roadmap

```text
Frontend Request → API → Controller

@GetMapping("/books/{id}")
→ @PathVariable → Fetch book by ID

@PostMapping("/books")
→ @RequestBody → Parse JSON into DTO

@ResponseBody (default with @RestController)
→ Returns data as JSON to frontend
````

---

## 📚 6. Conceptual Explanation

### 📦 Key Spring Annotations

| Annotation        | Purpose                                            |
| ----------------- | -------------------------------------------------- |
| `@RestController` | Combines `@Controller` + `@ResponseBody`           |
| `@RequestMapping` | Sets base path at class level                      |
| `@GetMapping`     | Maps GET requests                                  |
| `@PostMapping`    | Maps POST requests                                 |
| `@PutMapping`     | Maps PUT requests                                  |
| `@DeleteMapping`  | Maps DELETE requests                               |
| `@PathVariable`   | Captures values from the URL path                  |
| `@RequestBody`    | Converts JSON request body into a Java object      |
| `@ResponseBody`   | Sends Java object as JSON response (implicit here) |

---

## 💻 7. Hands-On Implementation (BookMart Endpoints)

### ✅ BookController.java

```java
@RestController
@RequestMapping("/api/books")
public class BookController {

    @Autowired
    private BookService bookService;

    // GET by ID
    @GetMapping("/{id}")
    public ResponseEntity<Book> getBookById(@PathVariable int id) {
        Book book = bookService.getBookById(id);
        return ResponseEntity.ok(book);
    }

    // POST - Create
    @PostMapping
    public ResponseEntity<Book> createBook(@RequestBody BookDto bookDto) {
        Book createdBook = bookService.createBook(bookDto);
        return new ResponseEntity<>(createdBook, HttpStatus.CREATED);
    }

    // PUT - Update
    @PutMapping("/{id}")
    public ResponseEntity<Book> updateBook(@PathVariable int id, @RequestBody BookDto bookDto) {
        Book updatedBook = bookService.updateBook(id, bookDto);
        return ResponseEntity.ok(updatedBook);
    }

    // DELETE - Remove
    @DeleteMapping("/{id}")
    public ResponseEntity<String> deleteBook(@PathVariable int id) {
        bookService.deleteBook(id);
        return ResponseEntity.ok("Book deleted successfully");
    }
}
```

---

## 📤 8. Output-Based Assessment

| ✅ Task                                | 💬 Expected Outcome                            |
| ------------------------------------- | ---------------------------------------------- |
| Endpoints handle URL path variables   | `/api/books/{id}` parsed using `@PathVariable` |
| JSON body mapped using `@RequestBody` | Java object populated from POST/PUT payload    |
| JSON responses returned               | Implicit via `@RestController`                 |
| All CRUD endpoints tested             | With tools like Postman or Curl                |
| GitHub push completed                 | `feature/rest-endpoint-annotations`            |

---

## 🎯 9. Interview Preparation

### Q1. What does `@RestController` do?

> It combines `@Controller` and `@ResponseBody` to return JSON directly.

### Q2. What is the use of `@PathVariable`?

> It binds a variable from the URL path to a method parameter.

### Q3. When is `@RequestBody` used?

> When accepting JSON input from the request body and converting it to a Java object.

### Q4. How are responses returned from REST endpoints?

> As JSON by default when using `@RestController`.

---

## 🔄 10. Connection to the Next Problem Statement

We’ve now built working endpoints and learned how to handle input/output via JSON.
The next session will dive deeper into **Entity-DTO mapping** – converting database models to frontend-friendly response formats using **ModelMapper or manual mapping**.

---

## ✅ Next Topic:

### Session 6 → DTO Mapping & ModelMapper Integration in BookMart

