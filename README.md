## 🎯 1. Main Project (PBL Context)

BookMart should handle:
- 📈 More users (buyers + sellers)
- ⚖️ Balanced load across instances
- 🔁 Smooth failover if one instance crashes

In this session, you’ll:
- Spin up multiple instances of your Spring Boot app
- Containerize them using Docker
- Distribute traffic using an Nginx load balancer

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> A single Spring Boot app crashes under load.  
> Solution: Run multiple **Spring Boot containers** and use **Nginx** to route traffic among them.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Understand what horizontal scaling is
- ✅ Run multiple Spring Boot app instances on different ports
- ✅ Use Docker to containerize the app
- ✅ Configure Nginx to balance traffic across instances

---

## 🧠 4. Scenario-Based Framing

> Imagine:
- 500 users hitting `/api/books` at once
- One server can’t handle the traffic and fails
- You spin up **3 containers**, balance traffic using **Nginx**, and keep response time low

---

## 🗺️ 5. Mini Visual Roadmap

```text
🧠 Understand Horizontal Scaling
🐳 Dockerize Spring Boot App
🚀 Run multiple instances (ports 8081, 8082)
⚖️ Nginx routes traffic to containers
🌍 App becomes scalable & resilient
````

---

## 📚 6. Conceptual Explanation

### 📦 What is Horizontal Scaling?

Horizontal scaling = Running **multiple instances** of the same app to divide the load
✅ Each instance runs on a separate port or container
✅ A **load balancer** routes traffic evenly
❌ Vertical scaling = Increasing RAM/CPU on one machine

---

### 🐳 Why Use Docker?

| Feature       | Benefit                               |
| ------------- | ------------------------------------- |
| Portable      | Works on any OS (Linux, Windows, Mac) |
| Lightweight   | Faster than VMs                       |
| Easy to scale | Launch multiple containers in seconds |

---

### ⚖️ Why Load Balancing?

Distributes traffic between instances. Prevents:

* One instance being overloaded
* Downtime during scaling
  Tools: **Nginx, HAProxy, AWS ALB, etc.**

---

## 💻 7. Hands-On Implementation

---

### ✅ Step 1: Run Multiple App Instances

In `application.properties`:

```properties
server.port=8081  # For Instance 1
```

Then copy the project, change port to `8082` in another folder, and run both:

```bash
java -jar bookmart.jar --server.port=8081
java -jar bookmart.jar --server.port=8082
```

---

### ✅ Step 2: Dockerize Your App

Create `Dockerfile` in root of project:

```Dockerfile
FROM openjdk:17-jdk-alpine
COPY target/bookmart.jar bookmart.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "bookmart.jar"]
```

Build and run:

```bash
docker build -t bookmart .
docker run -p 8081:8080 bookmart
docker run -p 8082:8080 bookmart
```

---

### ✅ Step 3: Setup Nginx as Load Balancer

Create `nginx.conf` file:

```nginx
events {}

http {
  upstream bookmart_backend {
    server host.docker.internal:8081;
    server host.docker.internal:8082;
  }

  server {
    listen 80;

    location / {
      proxy_pass http://bookmart_backend;
    }
  }
}
```

Run Nginx:

```bash
docker run -d -p 80:80 -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf nginx
```

Test via:

```
http://localhost/api/books
```

Requests will be load balanced between ports 8081 and 8082 🎯

---

## 📤 8. Output-Based Assessment

| ✅ Task                            | 💬 Expected Outcome                                |
| --------------------------------- | -------------------------------------------------- |
| Spring Boot app runs on 8081,8082 | 2 containers or processes live                     |
| Docker image built successfully   | `bookmart` image available locally                 |
| Nginx balances traffic            | Each request alternates between instances          |
| GitHub push                       | `feature/docker-nginx-loadbalancing` branch pushed |

---

## 🎯 9. Interview Preparation

### Q1. What is horizontal scaling?

> Running multiple instances of an app to divide traffic and improve reliability.

### Q2. Why use Docker with Spring Boot?

> Docker makes your app portable, lightweight, and easy to scale in production.

### Q3. What is a load balancer?

> A component that distributes incoming traffic to multiple backend instances.

### Q4. What does Nginx do in this setup?

> It listens on port 80 and proxies requests to `localhost:8081`, `8082`, etc., in round-robin fashion.

---

## 🔄 10. Connection to the Next Problem Statement

Now that BookMart can scale horizontally...

Next session will focus on:

* **CI/CD basics**
* Automating build & deploy using GitHub Actions


