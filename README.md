# 📘 Session 7: Request Routing & API Integration in Spring Boot

> **Project Context:** BookMart – Designing smart and maintainable routes in your backend while preparing for future integration with internal/external APIs.

---

## 🎯 1. Main Project (PBL Context)

In this session, we focus on how **Spring Boot handles routing**, how requests are mapped to endpoints, and how responses are structured and sent back to the client.  
We'll also explore the **API integration pattern** to lay the foundation for integrating with external services like AI recommendation APIs or payment gateways in the future.

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Design a robust and clean routing mechanism in Spring Boot that handles different request types, path variables, query parameters, and integrates with internal/external services in a modular way.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Understand how routing works in Spring Boot
- ✅ Use `@PathVariable`, `@RequestParam`, and `@RequestBody` to handle client input
- ✅ Send consistent JSON responses
- ✅ Route to internal services (like a recommendation engine)
- ✅ Create a service method simulating integration with an AI or external API

---

## 🧠 4. Scenario-Based Framing

> A buyer visits `/api/books/recommendations?genre=fiction`  
> The backend should:
> - Route the request correctly
> - Extract query parameters
> - Call an internal service (simulated AI engine)
> - Return a list of recommended books as JSON

> In the future, this pattern could help integrate real AI APIs (ChatGPT, Gemini, Bard, etc.).

---

## 🗺️ 5. Mini Visual Roadmap

```text
Request → /api/books/recommendations?genre=fiction
   ↓
@GetMapping → Controller → Service Method
   ↓
Returns mock AI-recommended books (via custom logic or stub)
   ↓
ResponseEntity<List<BookDto>> → JSON response
````

---

## 📚 6. Conceptual Explanation

### 🔀 Routing in Spring Boot

| Annotation        | Use Case                       |
| ----------------- | ------------------------------ |
| `@RequestMapping` | Class-level route prefix       |
| `@GetMapping`     | Handle GET requests            |
| `@PostMapping`    | Handle POST requests           |
| `@PathVariable`   | Capture data from URI          |
| `@RequestParam`   | Capture query params (?genre=) |
| `@RequestBody`    | Accept JSON in request body    |

### 💡 Response Handling

* Always return meaningful status codes
* Use `ResponseEntity<T>` to control headers, body, and status

---

## 💻 7. Hands-On Implementation (Routing + AI Recommendation Stub)

### 📦 Folder Setup

Use your existing structure:

```
com.bookmart
├── controller
│   └── BookController.java
├── service
│   └── BookService.java
│   └── BookServiceImpl.java
├── dto
│   └── BookDto.java
```

### ✅ Add Routing + API Simulation

#### 📍 Controller (BookController.java)

```java
@GetMapping("/recommendations")
public ResponseEntity<List<BookDto>> getRecommendations(@RequestParam String genre) {
    List<BookDto> recommendations = bookService.recommendBooksByGenre(genre);
    return ResponseEntity.ok(recommendations);
}
```

#### 🧠 Service Layer

```java
@Override
public List<BookDto> recommendBooksByGenre(String genre) {
    // Mock AI-based recommendation logic
    List<BookDto> list = new ArrayList<>();

    if ("fiction".equalsIgnoreCase(genre)) {
        list.add(new BookDto("The Alchemist", 499));
        list.add(new BookDto("Life of Pi", 399));
    } else if ("tech".equalsIgnoreCase(genre)) {
        list.add(new BookDto("Clean Code", 599));
        list.add(new BookDto("Spring in Action", 699));
    }

    return list;
}
```

#### 🧪 Sample Request

```
GET http://localhost:8080/api/books/recommendations?genre=tech
```

#### 📤 Sample JSON Response

```json
[
  {
    "title": "Clean Code",
    "price": 599
  },
  {
    "title": "Spring in Action",
    "price": 699
  }
]
```

---

## 📤 8. Output-Based Assessment

| ✅ Task                                    | 💬 Expected Outcome                |
| ----------------------------------------- | ---------------------------------- |
| Custom routing with `@RequestParam` works | `/recommendations?genre=tech`      |
| Proper routing structure in controller    | Layered and RESTful                |
| AI service stub returns mock data         | List of BookDto                    |
| JSON response formatted cleanly           | With status 200 OK                 |
| GitHub push done                          | `feature/book-routing-integration` |

---

## 🎯 9. Interview Preparation

### Q1. What is the use of `@RequestParam`?

> It binds a query parameter from the URL to a method argument.

### Q2. What’s the difference between `@PathVariable` and `@RequestParam`?

> `@PathVariable` extracts from URI path, `@RequestParam` from query string.

### Q3. How do you return JSON from a Spring Boot controller?

> Use `@RestController`, and return objects or lists via `ResponseEntity`.

### Q4. How would you integrate an external API?

> Create a `RestTemplate` or use `WebClient`, call the external API in service layer, and return data via controller.

---

## 🔄 10. Connection to the Next Problem Statement

Now that we have routed and simulated a response from a backend AI service, the next logical step is to implement **DTO Mapping** for all major endpoints to ensure frontend-friendly data.
We'll also explore using libraries like **ModelMapper** to automate this mapping.

---

## ✅ Next Topic:

### Session 8 → DTO Mapping, ModelMapper Integration & Cleaner JSON in BookMart

