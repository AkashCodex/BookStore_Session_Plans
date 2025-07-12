# 📘 Session 12: Role-Based Access & OAuth2 Integration in Spring Boot

> **Project Context:** BookMart – Different features are available to different user roles (e.g., Buyers vs Sellers). We now enforce **role-based restrictions** using annotations and prepare for **Google login (OAuth2)** integration.

---

## 🎯 1. Main Project (PBL Context)

In BookMart:
- **Sellers** can list books, view inventory.
- **Buyers** can place orders, add to cart.
- **Admins** may manage users and books.

We’ll enforce **access control based on roles** and prepare to support login via **Google OAuth2**, a common real-world authentication flow.

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Secure BookMart endpoints by user role using `@PreAuthorize` or `@Secured`, and integrate OAuth2 login capability with Spring Security (Google Sign-In).

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Understand how role-based access works in Spring Security
- ✅ Use annotations like `@PreAuthorize` and `@Secured`
- ✅ Configure security context to assign roles
- ✅ Understand how OAuth2 login flow works
- ✅ Integrate Google Login into Spring Boot app

---

## 🧠 4. Scenario-Based Framing

> A Seller logs in and tries to POST `/api/books` → Allowed  
> A Buyer tries the same → Forbidden (403)  
> An Admin accesses `/api/users` → Allowed

> A new user chooses “Sign in with Google” and is authenticated via OAuth2 and redirected back to BookMart dashboard.

---

## 🗺️ 5. Mini Visual Roadmap

```text
🔒 Role Setup (Buyer, Seller, Admin)
   ↓
🔐 Secure APIs using @PreAuthorize / @Secured
   ↓
🔗 Add OAuth2 Login config (Google)
   ↓
🧪 Test protected routes by simulating roles
````

---

## 📚 6. Conceptual Explanation

### 🧾 What is Role-Based Access Control (RBAC)?

* A method to restrict system access based on user roles.
* Users are assigned one or more roles (BUYER, SELLER, ADMIN)
* Each role has a set of permissions.

### 🔑 Key Annotations

| Annotation                    | Purpose                                    |
| ----------------------------- | ------------------------------------------ |
| `@PreAuthorize`               | Checks roles/permissions before method run |
| `@Secured`                    | Simpler version for checking roles         |
| `@EnableGlobalMethodSecurity` | Enables method-level security              |

### 🔐 OAuth2 Authentication (Google)

* Open standard for token-based authentication
* Spring Security supports it out-of-the-box
* Simplifies login process using social accounts

---

## 💻 7. Hands-On Implementation

### ✅ Step 1: Add Role Enum

```java
public enum Role {
    ROLE_BUYER,
    ROLE_SELLER,
    ROLE_ADMIN
}
```

---

### ✅ Step 2: Annotate Controller Methods

```java
@RestController
@RequestMapping("/api/books")
public class BookController {

    @PreAuthorize("hasRole('SELLER')")
    @PostMapping
    public ResponseEntity<String> createBook(@RequestBody BookDto bookDto) {
        return ResponseEntity.ok("Book created successfully");
    }

    @PreAuthorize("hasAnyRole('SELLER', 'BUYER')")
    @GetMapping
    public ResponseEntity<List<BookDto>> getAllBooks() {
        return ResponseEntity.ok(new ArrayList<>());
    }
}
```

---

### ✅ Step 3: Enable Method Security

In `SecurityConfig.java`:

```java
@EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true)
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    // existing config...
}
```

---

### ✅ Step 4: Add OAuth2 Google Login

In `application.properties`:

```properties
spring.security.oauth2.client.registration.google.client-id=your-client-id
spring.security.oauth2.client.registration.google.client-secret=your-client-secret
spring.security.oauth2.client.registration.google.redirect-uri={baseUrl}/login/oauth2/code/{registrationId}
spring.security.oauth2.client.registration.google.scope=openid,profile,email
```

Add to `SecurityConfig.java`:

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        .authorizeRequests()
            .antMatchers("/api/books").hasRole("SELLER")
            .anyRequest().authenticated()
        .and()
        .oauth2Login(); // Enables Google login
}
```

Google will handle:

* Login page
* Token handling
* User profile access

---

## 📤 8. Output-Based Assessment

| ✅ Task                                             | 💬 Expected Outcome                 |
| -------------------------------------------------- | ----------------------------------- |
| `@PreAuthorize` restricts access                   | Seller-only access to book creation |
| Buyer can't access Seller routes                   | 403 Forbidden                       |
| Google login via `/login/oauth2/code/google` works | Redirect + authentication           |
| GitHub push complete                               | `feature/role-oauth2-integration`   |

---

## 🎯 9. Interview Preparation

### Q1. What’s the difference between `@PreAuthorize` and `@Secured`?

> `@PreAuthorize` uses SpEL and is more flexible; `@Secured` checks only roles.

### Q2. What is OAuth2?

> A secure protocol allowing third-party login using Google, GitHub, etc.

### Q3. How does Spring Security integrate with Google?

> Using `spring.security.oauth2.client.registration.*` configs and `oauth2Login()`.

### Q4. How to restrict an endpoint to multiple roles?

> `@PreAuthorize("hasAnyRole('ADMIN', 'SELLER')")`

---

## 🔄 10. Connection to the Next Problem Statement

Now that we've secured routes by role and added Google login, our next goal is to build **full CRUD with pagination, filtering, and sorting**, using **Spring Data JPA** and DTOs.

---

## ✅ Next Topic:

### Session 13 → Full CRUD + Pagination, Sorting & Filtering using Spring Data JPA in BookMart

