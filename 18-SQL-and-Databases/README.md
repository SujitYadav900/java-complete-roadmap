# 18 — SQL & Databases

> **Experience level: 🟣 Advanced (2 – 4 years)**

Every real application stores data. SQL is the universal language for relational databases — MySQL, PostgreSQL, Oracle, SQLite — and knowing it well is a prerequisite for JDBC, JPA/Hibernate, and every backend job. This module gives you the fundamentals: relational modeling, CRUD, queries, transactions, and schema design.

---

## 📂 Files in This Module

| File | What You'll Learn |
|------|-------------------|
| [Relational-Databases.md](Relational-Databases.md) | Tables, keys, and why relational design matters |
| [SQL-CRUD.md](SQL-CRUD.md) | INSERT, SELECT, UPDATE, DELETE |
| [SQL-Queries.md](SQL-Queries.md) | WHERE, JOIN, GROUP BY, ORDER BY, aggregates |
| [SQL-Advanced.md](SQL-Advanced.md) | Constraints, indexes, transactions, ACID |
| [Database-Design.md](Database-Design.md) | ERD, relationships, normalization |
| [MySQL-Getting-Started.md](MySQL-Getting-Started.md) | Install MySQL, connect, run SQL |
| [Exercises.md](Exercises.md) | Practice tasks for this module |

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:

1. ✅ Create tables with primary and foreign keys.
2. ✅ Write CRUD statements confidently.
3. ✅ Join multiple tables and aggregate data.
4. ✅ Use indexes and transactions appropriately.
5. ✅ Design a normalized schema for a small app.

---

## 🧭 Module Map

```mermaid
flowchart LR
    A[Relational DBs] --> B[SQL CRUD]
    B --> C[SQL Queries]
    C --> D[SQL Advanced]
    D --> E[Database Design]
    E --> F[MySQL Getting Started]
    F --> G[Exercises]
```

---

## 📌 Quick Preview

```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(150) UNIQUE
);

INSERT INTO users (name, email) VALUES ('Alice', 'alice@example.com');
SELECT id, name FROM users WHERE email = 'alice@example.com';
UPDATE users SET name = 'Alice Cooper' WHERE id = 1;
DELETE FROM users WHERE id = 1;
```

---

## Next Module

➡️ [19 — JDBC](../19-JDBC/README.md) — connect Java to databases.