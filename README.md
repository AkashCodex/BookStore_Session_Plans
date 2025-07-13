## 🎯 1. Main Project (PBL Context)

BookMart supports two roles:
- `ROLE_BUYER`
- `ROLE_SELLER`

We need:
- Secure login (returns JWT)
- All protected APIs to require valid tokens
- Restrict specific endpoints by role using `@PreAuthorize`

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> We need to implement **stateless, secure authentication** and restrict access to certain endpoints by user role.

We’ll do this using **JWT tokens**.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Understand what JWT is and why it's useful
- ✅ Generate JWT on login
- ✅ Add JWT to `Authorization` header
- ✅ Validate the token in every request using a filter
- ✅ Use `@PreAuthorize` to restrict endpoints by role

---

## 🧠 4. Scenario-Based Framing

> Imagine:
- A **buyer** tries to access a **seller-only** endpoint
- Or a **user sends a fake token**

We must:
- Authenticate only **valid tokens**
- Authorize only **authorized roles**

JWT enables:
- Stateless auth (no session storage)
- Easy client-server separation

---

## 🗺️ 5. Mini Visual Roadmap

```text
Login → 🎫 JWT Token generated
↓
Client stores it in localStorage or memory
↓
All requests → send JWT in Authorization header
↓
Backend filters request → validates JWT
↓
✅ Access granted based on role (@PreAuthorize)
````

---

## 📚 6. Conceptual Explanation

### 🔐 What is JWT (JSON Web Token)?

* Compact, stateless way to **authenticate and authorize**
* Encodes **claims** (username, roles, expiry, etc.)
* Signed with a **secret key**

```json
Header: { alg: "HS256", typ: "JWT" }
Payload: { username: "john", role: "ROLE_SELLER" }
Signature: HMACSHA256(base64url(header) + "." + base64url(payload), secret)
```

---

### 🔁 Why Use JWT?

| Traditional Session   | JWT                          |
| --------------------- | ---------------------------- |
| Server stores session | No server storage needed     |
| Stateful              | Stateless                    |
| Scaling is hard       | Easy to scale                |
| Cookie-based          | Header-based (Authorization) |

---

### 🔑 JWT Workflow

1. User logs in → Server validates credentials
2. Server returns JWT token
3. Client includes JWT in every request (header)
4. Backend:

   * Verifies signature
   * Validates expiration
   * Loads roles → uses `@PreAuthorize` for access control

---

## 💻 7. Hands-On Implementation

### ✅ Step 1: Add JWT Dependencies

```xml
<!-- JWT library -->
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.11.5</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.11.5</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId>
    <version>0.11.5</version>
    <scope>runtime</scope>
</dependency>
```

---

### ✅ Step 2: Generate JWT on Login

```java
public String generateToken(UserDetails userDetails) {
    return Jwts.builder()
        .setSubject(userDetails.getUsername())
        .claim("roles", userDetails.getAuthorities())
        .setIssuedAt(new Date(System.currentTimeMillis()))
        .setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60 * 10)) // 10 hours
        .signWith(secretKey, SignatureAlgorithm.HS256)
        .compact();
}
```

---

### ✅ Step 3: JWT Filter

Create a filter to:

* Intercept all requests
* Check the `Authorization` header
* Validate token
* Populate Spring Security context

```java
public class JwtAuthFilter extends OncePerRequestFilter {
    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain chain) {
        String authHeader = request.getHeader("Authorization");
        if (authHeader != null && authHeader.startsWith("Bearer ")) {
            String token = authHeader.substring(7);
            // Validate & set context
        }
        chain.doFilter(request, response);
    }
}
```

---

### ✅ Step 4: Secure Endpoints by Role

```java
@PreAuthorize("hasRole('SELLER')")
@PostMapping("/books")
public ResponseEntity<?> addBook(...) { ... }

@PreAuthorize("hasRole('BUYER')")
@GetMapping("/books/search")
public ResponseEntity<?> searchBooks(...) { ... }
```

---

### ✅ Step 5: Add Token in Request Header (Postman / Frontend)

```http
GET /api/books
Authorization: Bearer <jwt_token>
```

---

## 📤 8. Output-Based Assessment

| ✅ Task                        | 💬 Expected Outcome                          |
| ----------------------------- | -------------------------------------------- |
| Login returns valid JWT       | Token encodes username and role              |
| Protected APIs require JWT    | Unauthorized if token missing/invalid        |
| @PreAuthorize restricts roles | Only `ROLE_SELLER` can access seller APIs    |
| Token validation works        | Backend checks signature, expiration, claims |
| GitHub push                   | `feature/jwt-auth-role-based` branch created |

---

## 🎯 9. Interview Preparation

### Q1. What is JWT and why use it?

> A stateless, compact, secure token used for authentication and authorization.

### Q2. How do you validate JWT in Spring Boot?

> Use a custom filter to extract, validate, and load user roles into the context.

### Q3. What’s the benefit of `@PreAuthorize`?

> Declarative role-based access control at method level using SpEL expressions.

### Q4. How do you send JWT from the frontend?

> Add it in the `Authorization` header using `Bearer <token>` format.

---

## 🔄 10. Connection to the Next Problem Statement

Now our system is secure with JWT and roles.

Next session, we’ll write **integration tests** to verify security, error handling, and successful role-based access control with **JUnit + Spring Security Test**.

---

## ✅ Next Topic:

### Session 23 → Testing Secure APIs with JUnit, Spring Security & MockMvc

