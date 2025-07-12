# 📘 Session 14: Structured Logging with SLF4J & Logback in Spring Boot

> **Project Context:** BookMart – Logging is no longer optional in modern backend systems. We need clear, structured, and consistent logs for tracking API behavior, performance, and security — especially across distributed systems.

---

## 🎯 1. Main Project (PBL Context)

In BookMart, we’ve built multiple REST APIs (books, users, orders).  
To trace actions, debug issues, and prepare for production readiness:
- We will implement **structured logging** using `SLF4J` + `Logback`
- Introduce **log levels**, **correlation IDs** (like `X-Request-ID`), and 
- Log every request and response using **Filters or Interceptors**

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Add a logging mechanism that captures:
> - All incoming HTTP requests and outgoing responses
> - Correlation IDs for tracking across logs
> - Log format that is production-friendly (timestamp, level, message)
> - Structured log messages using SLF4J and Logback

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Use `SLF4J` with `@Slf4j` or `LoggerFactory`
- ✅ Set up custom log patterns in `logback-spring.xml`
- ✅ Create a `Filter` to log all HTTP requests/responses
- ✅ Generate correlation/request IDs (UUID) per request
- ✅ Follow log level best practices (`info`, `debug`, `error`)

---

## 🧠 4. Scenario-Based Framing

> A buyer places an order.  
> The API request/response is logged with a **Request ID** like `REQ-123456`, along with:
- Method (POST)
- Path (`/api/orders`)
- Timestamp
- Status code (201)

This makes it easier to:
- Debug issues
- Correlate logs across microservices
- Monitor traffic

---

## 🗺️ 5. Mini Visual Roadmap

```text
📦 Incoming Request
  ↓
🔍 Filter adds Request ID → Logs method, URI, body
  ↓
🧾 Controller handles logic
  ↓
📤 Filter logs response status, body
  ↓
📝 Logged to console/file via SLF4J & Logback
````

---

## 📚 6. Conceptual Explanation

### 🔧 SLF4J (Simple Logging Facade for Java)

* A logging API that allows plugging in any backend (Logback, Log4j, etc.)
* Spring Boot uses **Logback** by default

### 📘 Log Levels (Best Practices)

| Level   | Use for...                             |
| ------- | -------------------------------------- |
| `trace` | Very detailed, for debugging internals |
| `debug` | Detailed dev info                      |
| `info`  | Key app events (startup, login, save)  |
| `warn`  | Something unexpected but recoverable   |
| `error` | Exceptions, failed logic, etc.         |

---

### 📘 Correlation ID

* Unique ID per request
* Helps trace full lifecycle of a request
* Usually passed as a header: `X-Request-ID`

---

## 💻 7. Hands-On Implementation

### ✅ Step 1: Add `RequestLoggingFilter.java`

```java
@Component
@Order(1)
public class RequestLoggingFilter implements Filter {

    private static final Logger log = LoggerFactory.getLogger(RequestLoggingFilter.class);

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {

        HttpServletRequest req = (HttpServletRequest) request;
        HttpServletResponse res = (HttpServletResponse) response;

        String requestId = UUID.randomUUID().toString();
        req.setAttribute("X-Request-ID", requestId);

        log.info("[{}] → {} {}", requestId, req.getMethod(), req.getRequestURI());

        chain.doFilter(request, response);

        log.info("[{}] ← {} {}", requestId, res.getStatus(), req.getRequestURI());
    }
}
```

---

### ✅ Step 2: Use `@Slf4j` in Controllers/Services

```java
@Slf4j
@RestController
@RequestMapping("/api/orders")
public class OrderController {

    @PostMapping
    public ResponseEntity<String> placeOrder(@RequestBody OrderDto order) {
        log.info("Placing new order for userId: {}", order.getUserId());
        return ResponseEntity.status(HttpStatus.CREATED).body("Order Placed");
    }
}
```

---

### ✅ Step 3: Customize `logback-spring.xml`

Place inside `src/main/resources`:

```xml
<configuration>

    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>
                %d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n
            </pattern>
        </encoder>
    </appender>

    <root level="info">
        <appender-ref ref="STDOUT" />
    </root>

</configuration>
```

---

## 📤 8. Output-Based Assessment

| ✅ Task                          | 💬 Expected Outcome                         |
| ------------------------------- | ------------------------------------------- |
| Logs show method + URI + status | `[REQ-1234] → POST /api/orders`             |
| Correlation ID is consistent    | Same request ID for both request & response |
| `@Slf4j` logs controller logic  | Message shown in logs                       |
| Logback format is clean         | Timestamp, level, logger, message           |
| GitHub push complete            | `feature/logging-correlation-id`            |

---

## 🎯 9. Interview Preparation

### Q1. What is SLF4J and why is it used?

> It's a facade for various logging frameworks. Spring Boot uses it with Logback for structured, high-performance logging.

### Q2. What are common logging best practices?

> Use proper levels, avoid logging sensitive data, add correlation IDs, format consistently.

### Q3. How do you log HTTP requests/responses?

> Use Spring filters or interceptors to capture and log details like path, status, headers.

### Q4. What’s the purpose of correlation/request ID?

> To track a request across logs (especially in distributed systems or microservices).

---

## 🔄 10. Connection to the Next Problem Statement

Now that BookMart logs requests/responses with full traceability, we’ll move on to:

* Building full-featured **CRUD operations** with
* **Pagination**, **Sorting**, and **Filtering** using Spring Data JPA.

---

## ✅ Next Topic:

### Session 15 → CRUD + Pagination, Sorting & Filtering using Spring Data JPA (Books & Users)

