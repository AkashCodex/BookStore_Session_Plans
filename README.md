# 📘 Session 19: Database Migrations with Liquibase/Flyway

> **Project Context:** BookMart – As our application evolves, new features (like orders, reviews, payment tracking) will require schema updates.  
Using **Liquibase** or **Flyway**, we’ll manage schema changes safely, automatically, and in version-controlled steps.

---

## 🎯 1. Main Project (PBL Context)

The BookMart system is expanding.  
We need to:
- Create new tables (e.g., `order`, `payment`)
- Modify columns (e.g., add `isbn` to `book`)
- Maintain history of schema changes
- Ensure all developers/test/staging/prod use the **same DB version**

In this session:
- We integrate **Liquibase or Flyway**
- Use **changelogs** or **SQL scripts**
- Automate **schema versioning and rollbacks**

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> Manual DB changes lead to inconsistencies and production bugs.  
We must integrate a schema migration tool like Liquibase or Flyway to manage DB changes systematically and safely.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Understand schema versioning
- ✅ Configure **Liquibase** or **Flyway** in a Spring Boot app
- ✅ Write changelogs (Liquibase) or versioned SQL files (Flyway)
- ✅ Automate schema creation on app startup
- ✅ Track version history and rollbacks

---

## 🧠 4. Scenario-Based Framing

> Imagine adding an `isbn` field to the `books` table manually.  
> One developer forgets to run the ALTER SQL — boom! Runtime error.

Now imagine:
- A changelog XML/SQL is versioned in Git
- It auto-applies when the app runs
- Everyone stays in sync — safely, automatically

---

## 🗺️ 5. Mini Visual Roadmap

```text
📦 src/main/resources/db/changelog
  └── changelog-1.0.xml (Liquibase)
📂 or
📦 src/main/resources/db/migration
  └── V1__create_books_table.sql (Flyway)
↓
🛠️ Spring Boot starts → tool checks current schema version
↓
✅ Applies pending migrations
↓
🗃️ DB updated with full traceability
````

---

## 📚 6. Conceptual Explanation

### 🔁 What Is Schema Migration?

A **controlled, repeatable way to evolve your database structure** as your application changes.

### 🚀 Why Use Liquibase or Flyway?

| Feature                 | Liquibase            | Flyway                       |
| ----------------------- | -------------------- | ---------------------------- |
| Format Support          | XML, YAML, JSON, SQL | SQL only (simple & powerful) |
| Rollback Support        | Yes                  | Limited                      |
| Diff Support (generate) | Yes                  | No                           |
| ChangeLog Granularity   | Highly structured    | File-based per version       |

> BookMart Recommendation: Start with **Flyway** (simpler) or use **Liquibase** if rollback/version auditing is critical.

---

## 💻 7. Hands-On Implementation

### ✅ Option A: Flyway Setup

#### Step 1: Add Flyway Dependency

```xml
<dependency>
    <groupId>org.flywaydb</groupId>
    <artifactId>flyway-core</artifactId>
</dependency>
```

#### Step 2: Create Migration SQL File

📁 `src/main/resources/db/migration/V1__create_books_table.sql`

```sql
CREATE TABLE books (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    price DOUBLE PRECISION,
    category VARCHAR(100)
);
```

📁 `V2__add_isbn_column.sql`

```sql
ALTER TABLE books ADD COLUMN isbn VARCHAR(20);
```

#### Step 3: Run App

Flyway auto-detects new versions → applies sequentially → tracks in `flyway_schema_history`.

---

### ✅ Option B: Liquibase Setup

#### Step 1: Add Liquibase Dependency

```xml
<dependency>
    <groupId>org.liquibase</groupId>
    <artifactId>liquibase-core</artifactId>
</dependency>
```

#### Step 2: Configure `application.properties`

```properties
spring.liquibase.change-log=classpath:db/changelog/db.changelog-master.xml
```

#### Step 3: Create Master & ChangeLog

📁 `db.changelog-master.xml`

```xml
<databaseChangeLog
    xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
        http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.1.xsd">

    <include file="db/changelog/changelog-1.0.xml"/>
</databaseChangeLog>
```

📁 `changelog-1.0.xml`

```xml
<changeSet id="1" author="bookmart">
    <createTable tableName="books">
        <column name="id" type="int" autoIncrement="true">
            <constraints primaryKey="true"/>
        </column>
        <column name="title" type="varchar(255)"/>
        <column name="price" type="decimal(10,2)"/>
    </createTable>
</changeSet>
```

---

## 📤 8. Output-Based Assessment

| ✅ Task                  | 💬 Expected Outcome                      |
| ----------------------- | ---------------------------------------- |
| Tool configured         | App boot logs show Flyway/Liquibase init |
| Versioned files applied | Tables created automatically             |
| Future DB changes       | Go through new migration version files   |
| GitHub push complete    | `feature/db-migrations-setup` branch     |

---

## 🎯 9. Interview Preparation

### Q1. What is schema migration in Spring Boot?

> It’s a controlled way to apply and version DB structure changes using tools like Liquibase or Flyway.

### Q2. Flyway vs Liquibase – when to use?

> Use Flyway for simple SQL-based migrations.
> Use Liquibase if you need rollback, diffs, or support for multiple file formats.

### Q3. How does Flyway track migrations?

> It creates a `flyway_schema_history` table to track executed versions.

---

## 🔄 10. Connection to the Next Problem Statement

With schema versioning in place, our database is now production-ready.
Next, we’ll explore:

* **Testing Spring Boot APIs**
* Tools: **JUnit + Mockito + TestRestTemplate**

---

## ✅ Next Topic:

### Session 20 → Unit & Integration Testing of Spring Boot APIs
