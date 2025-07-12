# 📘 Session 3: Client-Server Model & Spring Boot Architecture

> **Project Context:** BookMart – Understanding how frontend clients (browsers) communicate with the backend Spring Boot server and how Spring Boot is architected to serve modern web applications.

---

## 🎯 1. Main Project (PBL Context)

Now that we’ve built basic APIs for BookMart, it’s time to understand the **architecture behind those APIs**.  
This session explains how frontend clients like browsers or Postman communicate with Spring Boot servers using the **Client-Server Model**. It also introduces key components and modules of the Spring Boot ecosystem.

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Understand how client devices send requests to a Spring Boot server, how ports and endpoints work, what happens under the hood in Spring Boot, and how auto-configuration enables a production-ready server with minimal setup.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Explain the client-server model
- ✅ Describe how HTTP requests flow from browsers to backend
- ✅ Understand ports, endpoints, and status codes
- ✅ Explore the high-level Spring Boot architecture
- ✅ Identify and use core annotations like `@RestController`, `@Service`, `@Repository`, and `@Component`
- ✅ Understand the role of Spring Boot modules (Web, Data, Security)
- ✅ Explain how auto-configuration and embedded Tomcat work

---

## 🧠 4. Scenario-Based Framing

> Imagine a user visiting `https://bookmart.com/books`.

- The browser sends an HTTP request (GET) to the backend server.
- Spring Boot receives the request through an embedded **Tomcat** server.
- The **controller** handles the request and sends a response (JSON or error).
- Spring Boot simplifies this setup with **auto-configuration** and **annotations** that create a plug-and-play backend architecture.

---

## 🗺️ 5. Mini Visual Roadmap

```text
Browser/Postman (Client)
   ↓
HTTP Request → http://localhost:8080/api/books
   ↓
Spring Boot (Embedded Tomcat)
   ↓
@RestController (BookController)
   ↓
@Service (BookServiceImpl)
   ↓
@Repository (BookRepository)
   ↓
Database → Response → Client (with HTTP status)
````

---

## 📚 6. Conceptual Explanation

### 🌐 What is the Client-Server Model?

* **Client**: Sends request (browser, Postman, mobile app)
* **Server**: Processes request, executes logic, returns response

Example:
Frontend → `/api/books` → Spring Boot → JSON Response

### 🔌 What is a Port?

* A virtual channel through which clients talk to servers.
* Spring Boot uses `8080` by default:
  `http://localhost:8080/api/books`

You can change it in `application.properties`:

```properties
server.port=9090
```

### 🔄 Endpoints & Response Codes

* **Endpoints**: URIs mapped to controller methods (`/api/books`)
* **Response Codes**:

  * `200 OK`: Success
  * `201 Created`: POST success
  * `400 Bad Request`: Invalid input
  * `404 Not Found`: Resource missing
  * `500 Internal Server Error`: Server crashed

### 🧱 Spring Boot Architecture Overview

* **Auto-configured** by Spring Boot Starter dependencies
* **Embedded Server**: Tomcat, Jetty, or Netty
* **Layered Architecture**:

  * Controller
  * Service
  * Repository
  * Entity
  * DTO

---

## 🔧 7. Spring Boot Core Annotations

| Annotation        | Purpose                                  |
| ----------------- | ---------------------------------------- |
| `@RestController` | Defines HTTP endpoints (GET, POST, etc.) |
| `@Service`        | Marks business logic classes             |
| `@Repository`     | Marks data access layer                  |
| `@Component`      | Generic Spring-managed Bean              |
| `@Autowired`      | Injects a Bean into another Bean         |

---

## 🧰 8. Hands-On Implementation (BookMart - Review & Apply)

### 📦 Package Structure:

```
com.bookmart
├── controller → BookController.java
├── service → BookService.java / BookServiceImpl.java
├── repository → BookRepository.java
├── entity → Book.java
├── dto → BookDto.java
└── BookMartApplication.java
```

### ✅ Tasks:

1. Verify the port setup in `application.properties`

```properties
server.port=8080
```

2. Add multiple endpoints in `BookController`:

```java
@GetMapping("/ping")
public ResponseEntity<String> ping() {
    return new ResponseEntity<>("BookMart backend is active!", HttpStatus.OK);
}
```

3. Return different response codes:

```java
@PostMapping
public ResponseEntity<Book> addBook(@Valid @RequestBody BookDto dto) {
    Book book = new Book();
    book.setTitle(dto.getTitle());
    book.setPrice(dto.getPrice());
    return new ResponseEntity<>(bookService.saveBook(book), HttpStatus.CREATED);
}
```

4. Demonstrate use of `@Service`, `@Repository`, `@Component` in layers

---

## 📤 9. Output-Based Assessment

| ✅ Task                            | 💬 Expected Outcome                 |
| --------------------------------- | ----------------------------------- |
| Client-server model explained     | Clear flow: Client → Server → DB    |
| Working endpoints verified        | `/api/books`, `/api/books/ping`     |
| Proper use of HTTP response codes | `200`, `201`, `400`                 |
| Spring annotations applied        | `@RestController`, `@Service`, etc. |
| Embedded Tomcat recognized        | Server runs on `localhost:8080`     |
| GitHub push                       | `feature/spring-boot-architecture`  |

---

## 🎯 10. Interview Preparation

### Q1. What is the client-server model?

> It's a system where the client requests services and the server provides responses, typically via HTTP.

### Q2. How does a browser communicate with Spring Boot?

> The browser sends an HTTP request to a Spring Boot application running on a specific port (e.g., 8080), which is handled by controllers.

### Q3. What is auto-configuration in Spring Boot?

> Spring Boot automatically configures Beans, controllers, DB connections, and embedded servers based on dependencies in the classpath.

### Q4. What is the role of Tomcat in Spring Boot?

> It acts as an embedded server to process HTTP requests — no need to deploy WAR files externally.

### Q5. What is the use of `@Component`, `@Service`, `@Repository`, `@RestController`?

> These annotations tell Spring to manage these classes as Beans and inject them where needed.

---

## 🔄 11. Connection to the Next Problem Statement

Now that you understand Spring Boot’s structure and the client-server model, we will explore **Spring Core** in depth — including how **IoC, Beans, Dependency Injection**, and **Loose Coupling** power the entire architecture behind the scenes.

---

## ✅ Next Topic:

### Session 4 → Spring Core: IoC, Beans, Autowiring, and Loose Coupling in BookMart

