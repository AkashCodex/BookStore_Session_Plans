## 🎯 1. Main Project (PBL Context)

As BookMart scales:
- Buyers search for books by title, category, price, etc.
- Sellers list books, view reports
- Admins may run analytics queries

> This leads to **performance bottlenecks** if queries are not optimized.

In this session:
- You’ll learn how **AI/ML tools** assist in planning SQL performance
- How PostgreSQL generates and analyzes query plans
- How to **benchmark** and **optimize** with the help of tools

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Slow queries hurt user experience. DBAs and developers must **analyze query performance**, identify bottlenecks, and **add proper indexes**.  
> AI tools can assist in reading query plans and giving actionable insights.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Understand query execution plans
- ✅ Use PostgreSQL's `EXPLAIN ANALYZE` to inspect performance
- ✅ Explore `pg_stat_statements` to monitor expensive queries
- ✅ Use AI tools like [Explain.dalibo.com](https://explain.dalibo.com/) for plan visualization
- ✅ Learn how AI can suggest indexes and schema changes

---

## 🧠 4. Scenario-Based Framing

> Imagine a BookMart query like:

```sql
SELECT * FROM books WHERE category = 'Tech' AND price > 500;
````

On large datasets, this can **trigger full table scans**.
We want to:

* Know **how long** it takes
* **Visualize** the query plan
* **Optimize** using suggested indexes

AI-based tools can **analyze the plan** and tell us:

* "Use a composite index on `(category, price)`"
* "This step causes a Seq Scan — avoid it"

---

## 🗺️ 5. Mini Visual Roadmap

```text
📦 Application sends SQL to PostgreSQL
↓
🧠 PostgreSQL parses → plans → executes query
↓
📈 EXPLAIN ANALYZE shows performance plan
↓
🤖 Tool (Explain.dalibo / AI engine) visualizes & gives insights
↓
📌 Index suggestions / rewrite query
```

---

## 📚 6. Conceptual Explanation

### ⚙️ What is a Query Execution Plan?

* **A roadmap showing how the database retrieves data**
* Includes steps like `Seq Scan`, `Index Scan`, `Nested Loop`, etc.

### 🧠 AI's Role in Query Planning

* Tools like **Explain.dalibo.com** analyze `EXPLAIN` output
* AI parses the plan and suggests:

  * **Missing indexes**
  * **Join optimizations**
  * **Partitioning suggestions**
  * **Query rewrites**

---

### 🛠 PostgreSQL Tools

| Tool                 | Purpose                                   |
| -------------------- | ----------------------------------------- |
| `EXPLAIN`            | Shows execution plan (cost, row estimate) |
| `EXPLAIN ANALYZE`    | Executes query and shows **actual** steps |
| `pg_stat_statements` | Tracks most expensive queries             |
| `auto_explain`       | Logs slow queries automatically           |

---

## 💻 7. Hands-On Implementation

### ✅ Step 1: Sample Query to Analyze

```sql
EXPLAIN ANALYZE
SELECT * FROM books WHERE category = 'Java' AND price > 500;
```

> Output may show:

```
Seq Scan on books  (cost=0.00..400.00 rows=50 width=80)
```

👎 This means full table scan.
We want an `Index Scan` instead.

---

### ✅ Step 2: Visualize with [Explain.dalibo.com](https://explain.dalibo.com)

1. Copy the `EXPLAIN ANALYZE` output
2. Paste into [https://explain.dalibo.com/](https://explain.dalibo.com/)
3. Tool highlights:

   * Expensive nodes
   * Join issues
   * Missing index recommendations

---

### ✅ Step 3: Add Composite Index

```sql
CREATE INDEX idx_books_category_price ON books(category, price);
```

Re-run query and analyze again — expect `Index Scan`.

---

### ✅ Step 4: Enable `pg_stat_statements` (in PostgreSQL)

PostgreSQL config:

```sql
-- Inside postgresql.conf
shared_preload_libraries = 'pg_stat_statements'
```

Run:

```sql
CREATE EXTENSION pg_stat_statements;

SELECT * FROM pg_stat_statements
ORDER BY total_exec_time DESC
LIMIT 5;
```

> Gives slowest queries in BookMart

---

## 📤 8. Output-Based Assessment

| ✅ Task                       | 💬 Expected Outcome                  |
| ---------------------------- | ------------------------------------ |
| Use `EXPLAIN ANALYZE`        | Shows step-by-step query performance |
| AI tool used (Dalibo)        | Plan visualized with index advice    |
| Index applied                | `Index Scan` replaces `Seq Scan`     |
| pg\_stat\_statements enabled | Top queries tracked with timings     |
| GitHub push                  | `feature/ai-query-optimization`      |

---

## 🎯 9. Interview Preparation

### Q1. What is an execution plan?

> A step-by-step breakdown of how a SQL query is processed by the database engine.

### Q2. What does `Seq Scan` mean in `EXPLAIN`?

> It means the DB is scanning the entire table — often slow on large datasets.

### Q3. What tools can you use to optimize SQL?

> `EXPLAIN ANALYZE`, `pg_stat_statements`, and AI-powered tools like `Explain.dalibo.com`.

### Q4. How can AI help in query planning?

> By interpreting complex query plans, suggesting indexes, and identifying slow joins or scans.

---

## 🔄 10. Connection to the Next Problem Statement

Now that our queries are optimized:

* Let’s write **Unit and Integration Tests** for our APIs
* Learn about **Test-Driven Development (TDD)** in Spring Boot

---

## ✅ Next Topic:

### Session 21 → Unit & Integration Testing (JUnit + Mockito + Spring Boot Test)

