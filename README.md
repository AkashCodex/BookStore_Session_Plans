## 🎯 1. Main Project (PBL Context)

BookMart handles:
- Buyer and seller details
- Transactional data (e.g., book orders, prices)
- Login/authentication tokens

> If this data is leaked or lost, it could severely impact trust and operations.

This session introduces:
- Database **security fundamentals**
- **Access control**, **basic encryption**
- **Backup** and **disaster recovery** planning

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> How do we protect our database against unauthorized access, data leaks, and accidental loss — especially when deployed to production?

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Understand the risks of unsecured databases
- ✅ Apply access control principles (users, roles, privileges)
- ✅ Learn basic concepts of encryption (at rest, in transit)
- ✅ Explore backup & recovery techniques (logical & physical)
- ✅ Know how to secure Spring Boot’s DB configuration

---

## 🧠 4. Scenario-Based Framing

> Imagine BookMart is deployed live.  
> Someone accesses the DB using a leaked password and deletes the `books` table.  
> There’s no backup. The business halts.

We need:
- Proper **DB user management**
- **Secure connections (SSL/TLS)**
- Automated **backups** with clear recovery steps

---

## 🗺️ 5. Mini Visual Roadmap

```text
🧑‍💼 DB Roles & Permissions → Read/Write only when needed
🔐 Sensitive Data → Encrypt at rest + SSL in transit
🗃️ Backups → pg_dump + pg_restore
📦 Secure config → Use secrets manager / vault
````

---

## 📚 6. Conceptual Explanation

### 🔐 Database Access Control

| Practice                     | Description                                 |
| ---------------------------- | ------------------------------------------- |
| Principle of Least Privilege | Users should only have the access they need |
| Role-Based Access            | Separate roles for reader, writer, admin    |
| Avoid `root` access          | Never use DB superuser in production apps   |

---

### 🔐 Encryption

| Type        | Example in BookMart          | Tool / Concept                |
| ----------- | ---------------------------- | ----------------------------- |
| In Transit  | SSL for DB connections       | `sslmode=require` in JDBC URL |
| At Rest     | Encrypted PostgreSQL storage | File system / cloud level     |
| Field-level | Encrypt passwords, tokens    | BCrypt, AES, RSA              |

---

### 🛡️ Spring Boot DB Config Security

* Avoid hardcoding passwords in `application.properties`
* Use:

  * Environment variables
  * `.env` files
  * Spring Cloud Vault or AWS Secrets Manager
* Example:

```properties
spring.datasource.username=${DB_USER}
spring.datasource.password=${DB_PASS}
```

---

### 💾 Backup & Recovery

| Method          | Description                                  |
| --------------- | -------------------------------------------- |
| `pg_dump`       | Backup PostgreSQL DB into a `.sql` file      |
| `pg_restore`    | Restore from a backup                        |
| Scheduled cron  | Automate daily/weekly backups                |
| Cloud snapshots | RDS, GCP, Azure DB snapshots                 |
| Offsite backup  | Store encrypted backups in separate location |

```bash
# Backup
pg_dump -U postgres -d bookmart > backup_bookmart_21.sql

# Restore
psql -U postgres -d bookmart < backup_bookmart_21.sql
```

---

## 💻 7. Hands-On Implementation

### ✅ Step 1: Create a New Read-Only User

```sql
-- Connect as admin
CREATE USER bookmart_reader WITH PASSWORD 'secure123';
GRANT CONNECT ON DATABASE bookmart TO bookmart_reader;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO bookmart_reader;
```

---

### ✅ Step 2: Enable SSL in JDBC (PostgreSQL Example)

```properties
spring.datasource.url=jdbc:postgresql://db.example.com:5432/bookmart?sslmode=require
```

---

### ✅ Step 3: Encrypt Passwords with BCrypt (Spring Security)

```java
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

---

### ✅ Step 4: Perform Backup Locally

```bash
pg_dump -U postgres -d bookmart > backup.sql
```

Upload backup to cloud (AWS S3, Google Cloud Storage, etc.)

---

## 📤 8. Output-Based Assessment

| ✅ Task                    | 💬 Expected Outcome                         |
| ------------------------- | ------------------------------------------- |
| Read-only DB role created | Can connect but not modify DB               |
| JDBC uses SSL             | Secure in-transit communication             |
| Passwords encrypted in DB | Stored using bcrypt hash                    |
| Backup file generated     | File created and optionally uploaded        |
| GitHub push complete      | `feature/db-security-backup` branch created |

---

## 🎯 9. Interview Preparation

### Q1. Why is it dangerous to use the `postgres` user in production?

> It has superuser privileges; misuse could lead to complete data loss or leak.

### Q2. What are the types of encryption in databases?

> Encryption at rest, in transit, and field-level encryption (e.g., passwords).

### Q3. How would you secure DB credentials in Spring Boot?

> Use environment variables or a secrets manager; never commit passwords.

### Q4. What is `pg_dump` and how is it different from full disk backup?

> `pg_dump` is a logical backup (table/data structure), whereas full disk backup copies the entire DB directory.

---

## 🔄 10. Connection to the Next Problem Statement

With our database now secure and resilient:

* We can write **robust integration & unit tests**
* Next session focuses on **JUnit + Mockito** for testing BookMart APIs

---

## ✅ Next Topic:

### Session 22 → API Testing with JUnit & Mockito in Spring Boot

