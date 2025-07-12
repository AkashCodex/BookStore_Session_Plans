# 📘 Session 9: Advanced Request Handling & API Integration (OpenAI Simulation)

> **Project Context:** BookMart – Handle complex request/response scenarios, work with headers and status codes, and simulate real API integration (e.g., OpenAI AI Book Recommender).

---

## 🎯 1. Main Project (PBL Context)

As BookMart evolves, we want to build smarter features — like AI-generated book recommendations, auto-categorization, or smart search suggestions.

To prepare, developers must master handling inputs via `@RequestParam`, `@PathVariable`, and `@RequestBody`, and responding with status codes, headers, and JSON.  
This session also introduces the **integration pattern** used for 3rd-party APIs such as **OpenAI**, **Google Books**, etc.

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Design a robust Spring Boot endpoint that:
> - Accepts inputs via URL (path/query)
> - Accepts a request body (JSON)
> - Returns JSON with custom headers and status codes
> - Simulates integration with an external API (OpenAI-style recommendation logic)

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Use `@RequestParam`, `@PathVariable`, and `@RequestBody` together
- ✅ Return JSON with `@ResponseBody` and `ResponseEntity`
- ✅ Set custom HTTP status codes and headers
- ✅ Structure external API integration logic via `RestTemplate` or mock
- ✅ Build a real-world endpoint for AI-generated recommendations

---

## 🧠 4. Scenario-Based Framing

> A buyer clicks **"Get AI Book Suggestions"** on the frontend.  
> The frontend sends a `POST` request to `/api/ai/recommendations` with:
> - A genre or interest in JSON body
> - Optional user ID in the URL

The backend should:
- Parse the request body
- Simulate calling OpenAI (return fixed response for now)
- Return a structured JSON with status `200 OK` and a custom response header like `X-Recommendation-Engine: BookMart-AI`

---

## 🗺️ 5. Mini Visual Roadmap

```text
POST /api/ai/recommendations/{userId}?lang=en
   ↓
@PathVariable → userId
@RequestParam  → lang
@RequestBody   → genre / preference JSON
   ↓
Controller → Service → AI Stub (simulate external call)
   ↓
Returns ResponseEntity<List<BookDto>> with headers + status
````

---

## 📚 6. Conceptual Explanation

### 📥 Request Handlers

| Annotation       | Purpose                                 |
| ---------------- | --------------------------------------- |
| `@PathVariable`  | Extract from URL                        |
| `@RequestParam`  | Extract from query string               |
| `@RequestBody`   | Parse JSON body to Java object          |
| `@ResponseBody`  | Auto-return Java object as JSON         |
| `ResponseEntity` | Full control over status, headers, body |

### 📤 Response Handling

* **Custom HTTP Status:** `HttpStatus.CREATED`, `HttpStatus.BAD_REQUEST`
* **Custom Headers:** Add metadata like engine used, request ID
* **Body:** List of recommended `BookDto`

---

## 💻 7. Hands-On Implementation (BookMart + AI API Stub)

### 📦 Packages

```
com.bookmart
├── controller
│   └── AIController.java
├── dto
│   └── AIRequestDto.java
│   └── BookDto.java
├── service
│   └── AIService.java
│   └── AIServiceImpl.java
```

### ✅ DTOs

#### AIRequestDto.java

```java
public class AIRequestDto {
    private String genre;
    private String tone;
    // Getters, setters
}
```

#### BookDto.java

```java
public class BookDto {
    private String title;
    private double price;
    // Constructors, getters, setters
}
```

---

### ✅ Controller: AIController.java


@RestController
@RequestMapping("/api/ai")
public class AIController {

    @Autowired
    private AIService aiService;

    @PostMapping("/recommendations/{userId}")
    public ResponseEntity<List<BookDto>> getAIRecommendations(
        @PathVariable int userId,
        @RequestParam(defaultValue = "en") String lang,
        @RequestBody AIRequestDto request
    ) {
        List<BookDto> books = aiService.getRecommendations(userId, lang, request);

        HttpHeaders headers = new HttpHeaders();
        headers.add("X-Recommendation-Engine", "BookMart-AI");

        return ResponseEntity
                .status(HttpStatus.OK)
                .headers(headers)
                .body(books);
    }
}
```

---

### ✅ Service: AIServiceImpl.java


@Service
public class AIServiceImpl implements AIService {

    @Override
    public List<BookDto> getRecommendations(int userId, String lang, AIRequestDto request) {
        // Simulated AI logic
        List<BookDto> books = new ArrayList<>();

        if ("sci-fi".equalsIgnoreCase(request.getGenre())) {
            books.add(new BookDto("Dune", 450));
            books.add(new BookDto("Ender's Game", 350));
        } else {
            books.add(new BookDto("Atomic Habits", 499));
            books.add(new BookDto("Deep Work", 399));
        }

        return books;
    }
}
```

---

## 📤 8. Output-Based Assessment

| ✅ Task                              | 💬 Expected Outcome                              |
| ----------------------------------- | ------------------------------------------------ |
| Endpoint accepts all types of input | `@PathVariable`, `@RequestParam`, `@RequestBody` |
| JSON response is correct            | List of BookDto                                  |
| Custom headers added                | `X-Recommendation-Engine: BookMart-AI`           |
| Status code is 200 OK               | Sent via `ResponseEntity`                        |
| GitHub push complete                | `feature/ai-api-integration`                     |

---

## 🎯 9. Interview Preparation

### Q1. How is `ResponseEntity` different from returning an object?

> `ResponseEntity` lets you set status code, headers, and body manually.

### Q2. How do you handle multiple input types in a Spring controller?

> Use `@PathVariable` for URL values, `@RequestParam` for query strings, and `@RequestBody` for JSON.

### Q3. How do you simulate or integrate with external APIs?

> Use `RestTemplate`, `WebClient`, or stub out logic via service layer.

### Q4. Why use custom headers in API responses?

> For tracing, metadata, caching hints, or to identify the response engine.

---

## 🔄 10. Connection to the Next Problem Statement

Now that we’re capable of handling dynamic input and integrating external logic, we’ll transition to securing APIs with **validation, error handling**, and **custom exception management**.

---

## ✅ Next Topic:

### Session 10 → Validation, Exception Handling, and Error Response Design in Spring Boot

