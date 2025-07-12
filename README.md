# 📘 Session 1: Introduction to Backend Development with Java & Spring Boot

> **Project Context:** BookMart – An online bookstore platform supporting Buyer & Seller roles.

---

## 🎯 1. Main Project (PBL Context)

We are beginning backend development for **BookMart**, an online bookstore that enables sellers to manage books and buyers to search and purchase them.  
This session lays the foundation by introducing backend architecture using **Java + Spring Boot**.

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Set up the base backend and understand what backend development entails. Explore request processing, layered design (Controller → Service → Repository), and the backend's role in full-stack apps.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will be able to:

- ✅ Define backend development and its purpose
- ✅ Explain the role of backend in web & enterprise apps
- ✅ Identify key backend layers (Controller, Service, Repository)
- ✅ Describe how Java + Spring Boot are used to build scalable backends
- ✅ Test a simple REST API endpoint using Spring Boot

---

## 🧠 4. Scenario-Based Framing

> Imagine a user logs into BookMart and searches for “Data Structures in Java”.

Who processes this request?  
Where does the data come from?  
How is the result sent back?

➡️ This entire journey is managed by the **backend**, which:
- Receives the request (API)
- Fetches book data from a database
- Sends the result back as JSON

This is what backend development is all about!

---

## 🗺️ 5. Mini Visual Roadmap

```text
Frontend (React)
   ↓ API Call
Controller (Spring Boot)
   ↓
Service Layer
   ↓
Repository → DB (MySQL/Postgres)
````

* Controller handles the HTTP request
* Service layer applies business rules
* Repository layer accesses data

---

## 📚 6. Conceptual Explanation

### 🧱 What is Backend Development?

Backend refers to the **server-side logic** of a web application:

* Manages business rules and logic
* Stores and retrieves data
* Validates input, applies security
* Sends responses to frontend clients

### ☕ Why Java for Backend?

* Platform-independent via JVM
* Strong OOP principles
* Ecosystem: Spring Boot, Hibernate, Security, etc.
* Trusted in enterprise-grade systems

### 🌐 Spring Boot Sample Flow

```java
@RestController
@RequestMapping("/api/books")
public class BookController {

    @GetMapping
    public String getBooks() {
        return "Welcome to BookMart - Here are your books!";
    }
}
```

This:

* Exposes an HTTP GET API
* Can be tested at: `http://localhost:8080/api/books`

---

## 💻 7. Hands-On Implementation (BookMart Setup)

### ✅ Step-by-Step Setup

1. Visit [https://start.spring.io](https://start.spring.io)
2. Generate project with:

   * Language: Java
   * Spring Boot: 3.x
   * Dependencies: Spring Web
3. Create the following structure:

```
com.bookmart
├── controller → BookController.java
├── service → BookService.java (interface)
├── service → BookServiceImpl.java
├── repository → BookRepository.java
└── BookMartApplication.java
```

### ✅ Test Controller

```java
@RestController
@RequestMapping("/api/test")
public class BookController {
    
    @GetMapping
    public String home() {
        return "BookMart backend is up!";
    }
}
```

Test URL: [http://localhost:8080/api/test](http://localhost:8080/api/test)

---

## 📤 8. Output-Based Assessment

| ✅ Task                        | 💬 Expected Outcome                 |
| ----------------------------- | ----------------------------------- |
| Project compiles and runs     | Spring Boot app boots successfully  |
| `/api/test` endpoint works    | Returns test response               |
| Folder structure follows MVC  | Controller, Service, Repository     |
| Student explains backend flow | Request → Controller → Service → DB |

📌 **Push to GitHub**:
Branch: `feature/backend-intro`
Folder: `/src/main/java/com/bookmart/`

---

## 🎯 9. Interview Preparation

### Q1. What is backend development?

> Server-side logic that manages requests, processes data, and sends structured responses to frontend clients.

### Q2. Why Java is good for backend?

> It’s platform-independent, scalable, object-oriented, and supported by frameworks like Spring Boot.

### Q3. What is the role of the controller?

> Accepts HTTP requests and routes them to services.

### Q4. What happens when a frontend calls an API?

> The request is handled by the controller → passed to service → may hit repository → response sent back.

---

## 🔄 10. Connection to the Next Problem Statement

Now that we’ve set up our base backend structure, it’s time to understand **how Spring Boot manages objects internally using IoC (Inversion of Control), Beans, and Dependency Injection**.

---

## ✅ Next Topic:

### Session 2 → Spring Core: IoC, Beans, and Loose Coupling in BookMart
