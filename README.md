## 🎯 1. Main Project (PBL Context)

In BookMart, it's critical to follow a clean project structure to ensure the backend is **scalable**, **maintainable**, and **collaborative**.  
In this session, you'll learn to **organize your backend code**, set configuration via `application.properties`, and run your first REST API using **Maven** or **Gradle**.

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Set up a full Spring Boot project for BookMart with a clean folder structure and configuration. Build and run a working REST API endpoint from scratch.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Understand standard folder structure in a Spring Boot project
- ✅ Separate controller, service, model, and repository layers
- ✅ Use `application.properties` to configure port and DB (optional)
- ✅ Run the Spring Boot server via Maven or Gradle
- ✅ Build and test a simple REST API endpoint (e.g., `/api/hello`)

---

## 🧠 4. Scenario-Based Framing

> Imagine you're building BookMart with 3 other developers.  
> One person handles the controller, another the service, another data access.

To avoid confusion, the project must have:
- Clearly defined folders
- Proper config files
- A way to run the server locally for development and testing

---

## 🗺️ 5. Mini Visual Roadmap

```text
BookMart/
├── controller/     → Web endpoints (API layer)
├── service/        → Business logic
├── model/          → Entities (Book, User)
├── repository/     → JPA Repositories (DB access)
├── dto/            → Data Transfer Objects
├── config/         → Custom app configurations
├── resources/
│   └── application.properties
└── BookMartApplication.java (main)
````

---

## 📚 6. Conceptual Explanation

### 🧱 Standard Spring Boot Layered Architecture

| Layer        | Responsibility                       |
| ------------ | ------------------------------------ |
| Controller   | Handles HTTP requests                |
| Service      | Contains business logic              |
| Repository   | Interfaces with the database         |
| Model/Entity | Maps to DB tables                    |
| DTO          | API input/output data representation |

### ⚙️ application.properties Configuration

```properties
server.port=8080
spring.application.name=BookMart
```

Can be extended with DB configs later.

### 🚀 Run the App (Maven or Gradle)

**Using Maven:**

```bash
./mvnw spring-boot:run
```

**Using Gradle:**

```bash
./gradlew bootRun
```

Or directly via IntelliJ or VS Code "Run" button on `BookMartApplication.java`.

---

## 💻 7. Hands-On Implementation (BookMart Setup)

### ✅ Create Initial Project

1. Go to: [https://start.spring.io](https://start.spring.io)
2. Choose:

   * Project: Maven or Gradle
   * Language: Java
   * Spring Boot: 3.x
   * Dependencies: Spring Web

### ✅ Folder Setup

Create these packages under `src/main/java/com/bookmart/`:

```text
controller/
service/
repository/
model/
dto/
```

### ✅ Sample Controller

```java
@RestController
@RequestMapping("/api")
public class HelloController {

    @GetMapping("/hello")
    public String sayHello() {
        return "Welcome to BookMart Backend!";
    }
}
```

### ✅ Run & Test

* Run the app: `mvn spring-boot:run`
* Visit: [http://localhost:8080/api/hello](http://localhost:8080/api/hello)
* Expected: `"Welcome to BookMart Backend!"`

---

## 📤 8. Output-Based Assessment

| ✅ Task                            | 💬 Expected Outcome               |
| --------------------------------- | --------------------------------- |
| Folder structure cleanly created  | Follows MVC layering              |
| application.properties configured | Port & name added                 |
| Project builds & runs             | No errors during boot             |
| REST endpoint accessible          | `/api/hello` returns success      |
| GitHub push done                  | `feature/project-setup-structure` |

---

## 🎯 9. Interview Preparation

### Q1. What is the standard folder structure in Spring Boot?

> controller, service, repository, model/entity, config, dto – promoting separation of concerns.

### Q2. What is `application.properties` used for?

> For configuration: server port, DB URL, credentials, custom flags.

### Q3. How do you run a Spring Boot application?

> Via Maven (`./mvnw spring-boot:run`), Gradle, or from the IDE using the main class.

### Q4. What’s the role of the main class with `@SpringBootApplication`?

> It bootstraps the application by scanning components and auto-configuring dependencies.

---

## 🔄 10. Connection to the Next Problem Statement

Now that your project is cleanly structured and running, it’s time to handle **DTO mapping**.
You’ll learn to convert between frontend-friendly DTOs and backend entities using manual methods or tools like **ModelMapper**.

---

## ✅ Next Topic:

### Session 7 → DTO Mapping & ModelMapper Integration in BookMart

