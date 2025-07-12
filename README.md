# 📘 Session 11: Authentication, Error Handling & Logging in Spring Boot

> **Project Context:** BookMart – Implement robust backend features like JWT-based authentication, global exception handling, and production-level logging for real-world reliability and security.

---

## 🎯 1. Main Project (PBL Context)

In BookMart, we are now entering the **secured API** phase.  
Buyers and Sellers must **log in** to access features like creating listings or placing orders.  
We also want **clear error responses** and **logged activity** for debugging and audit.

This session sets up:
- JWT-based login system
- Global exception handler
- Centralized logging

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Secure all critical APIs with JWT-based authentication and authorization, implement meaningful error handling with global exception classes, and add proper logging using SLF4J.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Understand Authentication vs Authorization
- ✅ Implement login endpoint to generate JWT
- ✅ Secure REST endpoints using JWT and filters
- ✅ Build global exception handler with `@ControllerAdvice`
- ✅ Log important actions (login, errors, requests)

---

## 🧠 4. Scenario-Based Framing

> A Seller logs in using email/password.  
> On successful login, the backend returns a **JWT token**.  
> On any error (e.g., wrong credentials), a **proper error message** is sent with status code.  
> All activities (login attempts, errors) are **logged** in the console/log file.

---

## 🗺️ 5. Mini Visual Roadmap

```text
POST /api/auth/login → LoginController
   ↓
Validates user → Generate JWT → Return Token
   ↓
Other APIs require Authorization header: Bearer <token>
   ↓
Security Filter validates JWT and grants access
   ↓
If token invalid → send error → log it
````

---

## 📚 6. Conceptual Explanation

### 🔐 Authentication vs Authorization

| Term           | Description                           |
| -------------- | ------------------------------------- |
| Authentication | Validating user credentials (login)   |
| Authorization  | Allowing access to specific resources |

### 📜 JWT (JSON Web Token) Basics

* Stateless authentication method
* Token has 3 parts: Header, Payload, Signature
* Sent with each request via HTTP Header

### ⚠️ Global Error Handling

Use `@ControllerAdvice` + `@ExceptionHandler` to catch and return consistent error responses.

### 🧾 Logging (SLF4J / Logback)

* Use `@Slf4j` or `LoggerFactory.getLogger()` for logs
* Log important events (logins, errors, request traces)

---

## 💻 7. Hands-On Implementation

### 📦 Project Structure Additions

```text
com.bookmart
├── config
│   └── JwtFilter.java
│   └── SecurityConfig.java
├── controller
│   └── AuthController.java
├── dto
│   └── LoginRequestDto.java
│   └── JwtResponseDto.java
├── exception
│   └── GlobalExceptionHandler.java
│   └── InvalidCredentialsException.java
├── util
│   └── JwtUtil.java
```

---

### ✅ AuthController.java

```java
@RestController
@RequestMapping("/api/auth")
public class AuthController {

    @Autowired
    private AuthenticationManager authManager;

    @Autowired
    private JwtUtil jwtUtil;

    @PostMapping("/login")
    public ResponseEntity<JwtResponseDto> login(@RequestBody LoginRequestDto request) {
        try {
            Authentication auth = authManager.authenticate(
                new UsernamePasswordAuthenticationToken(request.getEmail(), request.getPassword())
            );

            String token = jwtUtil.generateToken(request.getEmail());
            return ResponseEntity.ok(new JwtResponseDto(token));

        } catch (AuthenticationException ex) {
            throw new InvalidCredentialsException("Invalid email or password");
        }
    }
}
```

---

### ✅ JwtUtil.java (simplified version)

```java
@Component
public class JwtUtil {

    private final String SECRET = "bookmart-secret";

    public String generateToken(String email) {
        return Jwts.builder()
            .setSubject(email)
            .setIssuedAt(new Date())
            .setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60)) // 1 hour
            .signWith(SignatureAlgorithm.HS512, SECRET)
            .compact();
    }

    public String extractEmail(String token) {
        return Jwts.parser().setSigningKey(SECRET)
            .parseClaimsJws(token)
            .getBody()
            .getSubject();
    }

    public boolean validateToken(String token) {
        try {
            Jwts.parser().setSigningKey(SECRET).parseClaimsJws(token);
            return true;
        } catch (JwtException e) {
            return false;
        }
    }
}
```

---

### ✅ GlobalExceptionHandler.java

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(InvalidCredentialsException.class)
    public ResponseEntity<String> handleInvalidCredentials(InvalidCredentialsException ex) {
        return ResponseEntity.status(HttpStatus.UNAUTHORIZED).body(ex.getMessage());
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleGeneric(Exception ex) {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("Something went wrong");
    }
}
```

---

### ✅ Logging with SLF4J

```java
@Slf4j
@RestController
public class AuthController {
    // Inside login()
    log.info("Login attempt for user: {}", request.getEmail());
}
```

---

## 📤 8. Output-Based Assessment

| ✅ Task                              | 💬 Expected Outcome              |
| ----------------------------------- | -------------------------------- |
| Login returns JWT                   | `/api/auth/login` returns token  |
| Token required for protected routes | Unauthorized without it          |
| Proper error handling in place      | Meaningful status + message      |
| Logs appear in console              | Login attempts, errors           |
| GitHub push done                    | `feature/jwt-auth-error-logging` |

---

## 🎯 9. Interview Preparation

### Q1. What is JWT? Why is it used?

> A secure way to transmit identity between client and server in a stateless manner.

### Q2. Difference between Authentication and Authorization?

> Auth**entication** = Who you are;
> Auth**orization** = What you can access.

### Q3. What is `@ControllerAdvice`?

> A Spring annotation used to handle exceptions globally.

### Q4. How do you log data in Spring Boot?

> Using `Logger` or `@Slf4j`, with various log levels (`info`, `warn`, `error`).

---

## 🔄 10. Connection to the Next Problem Statement

Now that we’ve secured our application and handled logging/error flow, next we will:

* **Build a full-featured CRUD module** (Book or Order)
* Integrate with DB using Spring Data JPA
* Use **DTO mapping**, pagination, and filtering

---

## ✅ Next Topic:

### Session 12 → Full CRUD Operations with Spring Data JPA, Pagination, and DTO Integration in BookMart

