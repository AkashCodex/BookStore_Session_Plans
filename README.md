## 🎯 1. Main Project (PBL Context)

In BookMart:
- DB queries can slow down if not optimized
- API load can spike during sales or promotions
- Admins need metrics to observe system behavior

In this session:
- We'll **optimize queries with indexing**
- **Monitor APIs with Actuator**
- **Load test APIs using Postman/JMeter**
- **Introduce Prometheus + Grafana** for enterprise-grade monitoring

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> APIs are getting slower. We have no visibility into backend performance under load.  
> Solution: Use **indexing, monitoring, and load testing** tools to diagnose and fix performance bottlenecks.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Add indexes to key database fields for performance
- ✅ Enable Spring Boot Actuator endpoints
- ✅ Run API load tests using Postman or JMeter
- ✅ Understand Prometheus + Grafana for production monitoring

---

## 🧠 4. Scenario-Based Framing

> Imagine:
- BookMart’s homepage loads slowly because **category filter** runs an unindexed DB query
- Admin wants to see **live metrics** like memory, CPU, DB status
- You simulate 500 users using **JMeter** and find the bottleneck

This session helps tackle these real-world challenges.

---

## 🗺️ 5. Mini Visual Roadmap

```text
📊 Enable Indexes → ⚙️ Improve DB query speed
📈 Enable Actuator → 🔍 View live app health & metrics
🧪 Load Testing → 📉 Measure real-world performance
🧭 Intro to Prometheus + Grafana → 🧠 Monitoring dashboard
````

---

## 📚 6. Conceptual Explanation

### 🔍 What is Indexing?

* A DB **index** speeds up lookup on a column (like a table of contents)
* Without index: full table scan
* With index: fast binary lookup

✅ Use indexing for frequently queried fields like:

* `title`
* `author`
* `category`
* `book_id`

---

### 📡 What is Spring Boot Actuator?

A production-ready **monitoring toolkit** for Spring Boot apps. It exposes:

* `/actuator/health` → app health
* `/actuator/metrics` → JVM, memory, custom metrics
* `/actuator/beans`, `/actuator/env`, etc.

---

### 🧪 What is Load Testing?

Simulating traffic (e.g., 1000 users in 5 seconds) to test how your API behaves under pressure. Tools:

* **Postman** runner (small-scale)
* **Apache JMeter** (enterprise scale)

---

### 📈 Prometheus + Grafana

* **Prometheus**: collects time-series metrics (via Actuator endpoints)
* **Grafana**: dashboards to visualize performance

---

## 💻 7. Hands-On Implementation

---

### ✅ Step 1: Add Indexing in DB

If using JPA annotations:

```java
@Entity
@Table(name = "books", indexes = {
    @Index(name = "idx_title", columnList = "title"),
    @Index(name = "idx_author", columnList = "author")
})
public class Book {
    // fields
}
```

📌 For existing SQL DB:

```sql
CREATE INDEX idx_title ON books(title);
CREATE INDEX idx_author ON books(author);
```

---

### ✅ Step 2: Add Actuator to Project

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

**application.properties**

```properties
management.endpoints.web.exposure.include=*
management.endpoint.health.show-details=always
```

Then access:

* `http://localhost:8080/actuator/health`
* `http://localhost:8080/actuator/metrics`
* `http://localhost:8080/actuator/httptrace`

---

### ✅ Step 3: Load Test with Postman or JMeter

**Option A: Postman**

* Create collection → Add `GET /api/books`
* Use Runner → Set iterations = 100

**Option B: JMeter**

* Create Thread Group → 100 threads, 10 loops
* Add HTTP Request Sampler
* Observe response time, failure rate

---

### ✅ Step 4: Setup Prometheus + Grafana (Optional)

Docker Compose sample:

```yaml
version: "3"
services:
  prometheus:
    image: prom/prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"

  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
```

Use Spring Boot's `micrometer` metrics:

```xml
<dependency>
  <groupId>io.micrometer</groupId>
  <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

---

## 📤 8. Output-Based Assessment

| ✅ Task                        | 💬 Expected Outcome                            |
| ----------------------------- | ---------------------------------------------- |
| Indexed DB columns            | Faster query on `/api/books?title=Java`        |
| Actuator exposed              | Accessible metrics via `/actuator/metrics`     |
| Load tested API               | Response times under 500ms for 100+ calls      |
| Prometheus running (optional) | Metrics visible on port 9090                   |
| Grafana dashboard (optional)  | Beautiful API monitoring panel                 |
| GitHub push                   | `feature/performance-monitoring` branch pushed |

---

## 🎯 9. Interview Preparation

### Q1. What is the purpose of DB indexing?

> To improve the speed of queries by reducing the need for full table scans.

### Q2. What does Spring Boot Actuator provide?

> Health, metrics, thread dumps, bean info, and other runtime diagnostics.

### Q3. What’s the difference between Postman and JMeter?

> Postman is good for small load tests; JMeter is better for high-scale simulation.

### Q4. What’s the role of Prometheus and Grafana?

> Prometheus collects metrics; Grafana visualizes them using dashboards.

---

## 🔄 10. Connection to the Next Problem Statement

Now that we’ve optimized and monitored performance…

Next session focuses on:

* **Security hardening**
* HTTP headers, HTTPS setup, and secure practices in Spring Boot

---

## ✅ Next Topic:

### Session 29 → Spring Boot Security Best Practices (HTTPS, CORS, Headers)

