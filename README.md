# 📘 Session 8: REST Annotations & Request-Response Handling in Spring Boot

> **Project Context:** BookMart – Mastering the use of Spring Boot annotations to handle path variables, query parameters, request bodies, and return structured JSON responses.

---

## 🎯 1. Main Project (PBL Context)

In BookMart, both **Buyers and Sellers** perform actions like searching books, creating listings, and retrieving order history.

This session will teach you how to **build expressive and flexible APIs** using Spring annotations. You'll handle multiple types of client input and return well-structured responses.

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Build BookMart REST endpoints using `@RestController`, `@RequestMapping`, `@GetMapping`, and `@PostMapping`, while handling different types of input (path variables, query parameters, request bodies) and responding with clean JSON output.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Use core Spring Web annotations to define endpoints
- ✅ Accept input via path variables (`@PathVariable`)
- ✅ Accept input via query parameters (`@RequestParam`)
- ✅ Accept input via JSON payloads (`@RequestBody`)
- ✅ Return structured responses using `ResponseEntity<>`

---

## 🧠 4. Scenario-Based Framing

> A seller wants to:
> - Create a new book listing (`POST /api/books`)
> - View book details (`GET /api/books/{id}`)
> - Search books by title (`GET /api/books/search?title=java`)

These use all three input types:
- **Path variable** (book ID)
- **Query param** (title)
- **Request body** (book data)

---

## 🗺️ 5. Mini Visual Roadmap

```text
@RequestMapping("/api/books")
   ├── @PostMapping + @RequestBody        → Create new book
   ├── @GetMapping("/{id}") + @PathVariable → Get book by ID
   └── @GetMapping("/search") + @RequestParam → Search by title
````

---

## 📚 6. Conceptual Explanation

### 🔍 Key Spring MVC Annotations

| Annotation        | Purpose                                           |
| ----------------- | ------------------------------------------------- |
| `@RestController` | Marks a class as REST API handler (returns JSON)  |
| `@RequestMapping` | Sets base URI path for the controller class       |
| `@GetMapping`     | Handles HTTP GET requests                         |
| `@PostMapping`    | Handles HTTP POST requests                        |
| `@PathVariable`   | Extracts data from URI path (e.g., `/books/{id}`) |
| `@RequestParam`   | Extracts query string parameters (`?title=xyz`)   |
| `@RequestBody`    | Maps JSON payload to a Java object                |

---

## 💻 7. Hands-On Implementation (BookMart: Full API Interaction)

### ✅ Controller: BookController.java

```java
@RestController
@RequestMapping("/api/books")
public class BookController {

    @Autowired
    private BookService bookService;

    // GET by ID - Path Variable
    @GetMapping("/{id}")
    public ResponseEntity<BookDto> getBookById(@PathVariable int id) {
        BookDto dto = bookService.getBookById(id);
        return ResponseEntity.ok(dto);
    }

    // GET by title - Query Param
    @GetMapping("/search")
    public ResponseEntity<List<BookDto>> searchBooksByTitle(@RequestParam String title) {
        return ResponseEntity.ok(bookService.searchBooksByTitle(title));
    }

    // POST - Create Book - Request Body
    @PostMapping
    public ResponseEntity<BookDto> createBook(@RequestBody BookDto bookDto) {
        BookDto savedBook = bookService.createBook(bookDto);
        return new ResponseEntity<>(savedBook, HttpStatus.CREATED);
    }
}
```

### 🧪 Sample Requests

```http
GET /api/books/10
GET /api/books/search?title=Java
POST /api/books
Content-Type: application/json
{
  "title": "Spring in Action",
  "price": 699
}
```

---

## 📤 8. Output-Based Assessment

| ✅ Task                                        | 💬 Expected Outcome                         |
| --------------------------------------------- | ------------------------------------------- |
| Path variable handled using `@PathVariable`   | `/api/books/10` returns book by ID          |
| Query parameter handled using `@RequestParam` | `/api/books/search?title=java` returns list |
| JSON body handled using `@RequestBody`        | POST creates a new book                     |
| JSON response sent using `ResponseEntity`     | Returns correct status and body             |
| GitHub push complete                          | `feature/request-response-annotations`      |

---

## 🎯 9. Interview Preparation

### Q1. What is the use of `@RestController`?

> It marks a class as a REST API controller and auto-converts return values to JSON.

### Q2. Difference between `@PathVariable` and `@RequestParam`?

> `@PathVariable` extracts from URL path; `@RequestParam` gets query string values.

### Q3. When do you use `@RequestBody`?

> When sending JSON data in the request, like creating or updating a resource.

### Q4. What’s the role of `ResponseEntity<>`?

> It wraps the response body, headers, and status code into a single object.

---

## 🔄 10. Connection to the Next Problem Statement

With a strong foundation in REST annotations and request/response handling, the next step is to learn **DTO Mapping** using **ModelMapper** for clean and consistent frontend-friendly responses.

---

## ✅ Next Topic:

### Session 9 → DTO Mapping, ModelMapper Integration & Clean JSON Design in BookMart

