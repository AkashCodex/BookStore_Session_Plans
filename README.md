## 🎯 1. Main Project (PBL Context)

In BookMart:
- Admins may log in using username/password
- We use `HttpSession` to track their login
- Store their user info in session for future requests
- Provide an endpoint to log out (invalidate session)

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Implement a traditional session-based login system using `HttpSession`, where a user logs in, is tracked across requests, and can log out by destroying the session.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Understand what session-based login is
- ✅ Use Spring Boot's `HttpSession` to store user info
- ✅ Authenticate users and track login across requests
- ✅ Logout by invalidating the session
- ✅ Understand tradeoffs vs stateless (JWT) login

---

## 🧠 4. Scenario-Based Framing

> Imagine an internal **admin dashboard** for BookMart:
- Admin logs in using username & password
- Backend stores their info in the server-side session
- Subsequent requests are automatically authenticated
- Logging out removes the session on server side

---

## 🗺️ 5. Mini Visual Roadmap

```text
User sends login POST → 🛡️ Credentials validated
↓
🧾 User object stored in HttpSession
↓
Future requests → backend checks session for login
↓
User logs out → session invalidated, login cleared
````

---

## 📚 6. Conceptual Explanation

### 💡 What is a Session-Based Login?

A login mechanism where:

* The backend stores the user info in **server memory** (session)
* The client uses a **session ID cookie** to identify itself
* Backend looks up session ID to retrieve the user

---

### 🔁 How It Works in Spring Boot

| Action              | What Happens                                                  |
| ------------------- | ------------------------------------------------------------- |
| Login               | Spring validates credentials and stores user in `HttpSession` |
| Request with cookie | Spring finds session, retrieves user                          |
| Logout              | `session.invalidate()` removes user session                   |

---

### 🔐 JWT vs Session-Based Auth

| Feature                 | JWT                 | Session-Based                      |
| ----------------------- | ------------------- | ---------------------------------- |
| Stateless               | ✅                   | ❌ (server stores session)          |
| Scalable (multi-server) | ✅ with shared token | ❌ unless using Redis session store |
| Simpler for admin tools | ❌                   | ✅                                  |

---

## 💻 7. Hands-On Implementation

---

### ✅ Step 1: Create Login Request DTO

```java
public class LoginRequest {
    private String username;
    private String password;
}
```

---

### ✅ Step 2: Create Login Controller with Session

```java
@RestController
@RequestMapping("/api/session-auth")
public class SessionAuthController {

    @Autowired
    private UserService userService;

    @PostMapping("/login")
    public ResponseEntity<String> login(@RequestBody LoginRequest request, HttpSession session) {
        Optional<User> user = userService.authenticate(request.getUsername(), request.getPassword());
        if (user.isPresent()) {
            session.setAttribute("user", user.get());
            return ResponseEntity.ok("Login successful");
        } else {
            return ResponseEntity.status(HttpStatus.UNAUTHORIZED).body("Invalid credentials");
        }
    }

    @GetMapping("/me")
    public ResponseEntity<?> currentUser(HttpSession session) {
        User user = (User) session.getAttribute("user");
        if (user != null) {
            return ResponseEntity.ok(user);
        }
        return ResponseEntity.status(HttpStatus.UNAUTHORIZED).body("Not logged in");
    }

    @PostMapping("/logout")
    public ResponseEntity<String> logout(HttpSession session) {
        session.invalidate();
        return ResponseEntity.ok("Logged out successfully");
    }
}
```

---

### ✅ Step 3: Service Authentication Logic (Example)

```java
@Service
public class UserService {
    private static List<User> users = List.of(
        new User("admin", "admin123", "ADMIN"),
        new User("buyer", "buyer123", "BUYER")
    );

    public Optional<User> authenticate(String username, String password) {
        return users.stream()
            .filter(u -> u.getUsername().equals(username) && u.getPassword().equals(password))
            .findFirst();
    }
}
```

---

## 📤 8. Output-Based Assessment

| ✅ Task               | 💬 Expected Outcome                             |
| -------------------- | ----------------------------------------------- |
| User logs in         | Session created, `user` stored in session       |
| Calls `/me` endpoint | Returns logged-in user info                     |
| Calls `/logout`      | Session destroyed, user logged out              |
| Invalid credentials  | Returns `401 Unauthorized`                      |
| GitHub push          | `feature/session-authentication` branch created |

---

## 🎯 9. Interview Preparation

### Q1. How is session-based login different from JWT?

> In session-based login, the server stores the session and tracks users. JWT is stateless and doesn’t require server storage.

### Q2. What is `HttpSession`?

> A Java interface that represents a user's session between requests. Used to store login data server-side.

### Q3. How do you invalidate a session in Spring?

> Call `session.invalidate()` to destroy all data and logout the user.

### Q4. Where is session data stored?

> By default, in server memory (RAM), but can be stored in Redis or database for clustering.

---

## 🔄 10. Connection to the Next Problem Statement

We’ve covered session login.
Now we’ll explore **testing authenticated sessions** using **MockMvc + JUnit**, and how to write tests for `HttpSession`-backed login flows.

---

## ✅ Next Topic:

### Session 25 → Testing Session-Based Auth APIs with MockMvc & JUnit

