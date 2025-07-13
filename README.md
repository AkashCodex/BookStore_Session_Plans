## 🎯 1. Main Project (PBL Context)

We’ve implemented:
- Service logic (e.g., fetching books, placing orders)
- Controllers with API endpoints
- Error handling & logging

Now, we want to:
- Test services in isolation using **Mockito**
- Test controllers using **MockMvc**
- Validate expected inputs/outputs

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> As our codebase grows, we must write **automated unit tests** for service and controller layers using **JUnit 5**, **Mockito**, and **Spring Boot testing tools**.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Understand the purpose of unit testing
- ✅ Use `@SpringBootTest`, `@WebMvcTest`, and `@MockBean`
- ✅ Write tests for services using mocks
- ✅ Write controller tests using MockMvc
- ✅ Run tests using JUnit 5

---

## 🧠 4. Scenario-Based Framing

> Imagine:
- You refactor `BookServiceImpl` → break something by accident
- Your `BookController` returns 500 instead of 200
- Tests help you catch bugs **before deploying**

Unit testing ensures:
- ✅ Predictable behavior
- ✅ Safe code changes
- ✅ Documented expectations

---

## 🗺️ 5. Mini Visual Roadmap

```text
🧠 Understand: What is unit testing?
🧪 Write: Service layer tests using Mockito
🧪 Write: Controller tests using MockMvc
🧪 Run: Tests using IntelliJ or Maven
✅ Validate: Inputs, outputs, error scenarios
````

---

## 📚 6. Conceptual Explanation

### 🧪 What is Unit Testing?

* Testing a **single unit** of code (method/class) in isolation
* No database or external dependency — all mocked
* Verifies expected inputs produce expected outputs

---

### 🧰 Spring Boot Test Annotations

| Annotation        | Purpose                                         |
| ----------------- | ----------------------------------------------- |
| `@SpringBootTest` | Loads full Spring context (integration testing) |
| `@WebMvcTest`     | Loads controller + web layer only               |
| `@MockBean`       | Injects a mock into the Spring context          |
| `@DataJpaTest`    | Tests repository layer with in-memory DB        |

---

## 💻 7. Hands-On Implementation

---

### ✅ Step 1: Add Dependencies

```xml
<!-- JUnit 5 is included in Spring Boot -->
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-core</artifactId>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

---

### ✅ Step 2: Write Service Test with Mockito

```java
@SpringBootTest
class BookServiceTest {

    @MockBean
    private BookRepository repository;

    @Autowired
    private BookService bookService;

    @Test
    void testGetAllBooksReturnsList() {
        List<Book> mockBooks = List.of(new Book(1, "DSA Handbook", "TechPress"));
        Mockito.when(repository.findAll()).thenReturn(mockBooks);

        List<Book> books = bookService.getAllBooks();
        Assertions.assertEquals(1, books.size());
        Assertions.assertEquals("DSA Handbook", books.get(0).getTitle());
    }
}
```

---

### ✅ Step 3: Write Controller Test with MockMvc

```java
@WebMvcTest(BookController.class)
class BookControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private BookService bookService;

    @Test
    void testGetAllBooksEndpoint() throws Exception {
        List<Book> mockBooks = List.of(new Book(1, "DSA Handbook", "TechPress"));
        Mockito.when(bookService.getAllBooks()).thenReturn(mockBooks);

        mockMvc.perform(get("/api/books"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$[0].title").value("DSA Handbook"));
    }
}
```

---

## 📤 8. Output-Based Assessment

| ✅ Task                            | 💬 Expected Outcome                     |
| --------------------------------- | --------------------------------------- |
| BookService test runs             | Validates mock repository + logic       |
| Controller test runs with MockMvc | Validates JSON response and status code |
| Failed test for bad logic         | Identifies bug via red test             |
| Use `@MockBean` for dependencies  | Service injected into controller test   |
| GitHub push                       | `feature/unit-testing` branch pushed    |

---

## 🎯 9. Interview Preparation

### Q1. What is the difference between `@SpringBootTest` and `@WebMvcTest`?

> `@SpringBootTest` loads the full context. `@WebMvcTest` loads only controller + web layer.

### Q2. What is Mockito used for?

> To mock external dependencies like repositories or services and isolate the class under test.

### Q3. How does `MockMvc` help in controller testing?

> It simulates HTTP requests without starting the server, allowing you to test routing, headers, response body, etc.

### Q4. What are some best practices in unit testing?

> Write small, focused tests; name them clearly; use mocks to isolate units; cover both success and failure paths.

---

## 🔄 10. Connection to the Next Problem Statement

Now that we can write and run tests...

Next session will explore:

* **Integration tests** for DB, security, and controller flows
* Using in-memory H2 DB
* Testing authenticated endpoints (JWT/session)

---

## ✅ Next Topic:

### Session 27 → Integration Testing with Spring Boot, DB, and Authentication

