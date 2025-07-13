## 🎯 1. Main Project (PBL Context)

In BookMart:
- Users often access the same data repeatedly (e.g., books, categories)
- Instead of querying the DB every time, we can **cache** the response
- We’ll use **Redis** as our caching layer with Spring annotations

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Repeated DB queries cause unnecessary load and latency.  
> Solution: Add **Redis caching** to improve performance using Spring Boot’s built-in support.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Understand caching and why Redis is used
- ✅ Add Redis to a Spring Boot project
- ✅ Use annotations like `@Cacheable`, `@CachePut`, and `@CacheEvict`
- ✅ Configure Redis via `application.properties`
- ✅ Test cache hit/miss behavior in BookMart

---

## 🧠 4. Scenario-Based Framing

> Imagine:
- A user visits the **Book Details** page repeatedly.
- Without caching: Every click = DB hit.
- With Redis: The first request stores data in Redis → next calls are fetched instantly.

---

## 🗺️ 5. Mini Visual Roadmap

```text
🧠 What is caching?
🔥 Why Redis?
⚙️ Spring Boot + Redis Setup
🧩 Use @Cacheable, @CacheEvict, @CachePut
🧪 Observe performance + hit/miss behavior
````

---

## 📚 6. Conceptual Explanation

### 💾 What is Caching?

Caching is the technique of **storing data temporarily** so it can be **quickly accessed** on future requests without reprocessing.

---

### 🚀 Why Redis?

| Feature              | Why It Helps                               |
| -------------------- | ------------------------------------------ |
| In-memory            | Super fast reads/writes                    |
| Key-Value store      | Simple structure, ideal for caching        |
| Spring Boot friendly | Easy integration via starter dependency    |
| Scalable             | Works in microservices or distributed apps |

---

### 🏷️ Key Annotations

| Annotation    | Purpose                                                        |
| ------------- | -------------------------------------------------------------- |
| `@Cacheable`  | Reads from cache; if not found, calls method and caches result |
| `@CachePut`   | Updates the cache with new return value                        |
| `@CacheEvict` | Removes entry from the cache                                   |

---

## 💻 7. Hands-On Implementation

---

### ✅ Step 1: Add Redis Dependency

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

---

### ✅ Step 2: Configure Redis in `application.properties`

```properties
spring.cache.type=redis
spring.redis.host=localhost
spring.redis.port=6379
```

📝 **Note**: Redis server must be running locally (use Docker or install manually).

---

### ✅ Step 3: Enable Caching in Your App

```java
@SpringBootApplication
@EnableCaching
public class BookMartApplication {
    public static void main(String[] args) {
        SpringApplication.run(BookMartApplication.class, args);
    }
}
```

---

### ✅ Step 4: Use Caching in Service

```java
@Service
public class BookServiceImpl implements BookService {

    @Autowired
    private BookRepository repository;

    @Override
    @Cacheable(value = "books", key = "#id")
    public Book getBookById(int id) {
        simulateSlowService(); // mimic heavy operation
        return repository.findById(id)
                .orElseThrow(() -> new RuntimeException("Book not found"));
    }

    @Override
    @CachePut(value = "books", key = "#book.id")
    public Book updateBook(Book book) {
        return repository.save(book);
    }

    @Override
    @CacheEvict(value = "books", key = "#id")
    public void deleteBookById(int id) {
        repository.deleteById(id);
    }

    private void simulateSlowService() {
        try { Thread.sleep(2000); } catch (InterruptedException ignored) {}
    }
}
```

---

## 📤 8. Output-Based Assessment

| ✅ Task                           | 💬 Expected Outcome                    |
| -------------------------------- | -------------------------------------- |
| Redis server connected           | Console log: "Connected to Redis..."   |
| First `getBookById(1)` slow      | Data fetched from DB with 2s delay     |
| Second `getBookById(1)` fast     | Cache hit, response returned instantly |
| `updateBook(book)` updates cache | Cache reflects new book data           |
| `deleteBookById(1)` evicts cache | Entry removed from cache               |
| GitHub push                      | `feature/redis-caching` branch pushed  |

---

## 🎯 9. Interview Preparation

### Q1. What is caching and why is it useful?

> Caching stores frequently accessed data to reduce DB hits and improve performance.

### Q2. Why Redis for Spring Boot?

> Redis is fast, in-memory, key-value based, and integrates easily with Spring via `spring-boot-starter-data-redis`.

### Q3. What does `@Cacheable` do?

> Checks cache first → if present, returns cached result; if not, calls the method and stores the result.

### Q4. What is the role of `@CacheEvict`?

> Deletes the entry from the cache (e.g., on delete or update operations).

---

## 🔄 10. Connection to the Next Problem Statement

Now that caching is working…

Next session will focus on:

* Adding **rate limiting** using Redis
* Preventing brute-force login, abuse of APIs, etc.

---

## ✅ Next Topic:

### Session 28 → API Rate Limiting with Redis and Spring Boot Filters

