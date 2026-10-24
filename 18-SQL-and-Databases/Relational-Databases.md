# Relational Databases

## 1. Introduction

A **relational database** stores data in **tables** of rows and columns, linked together by **keys**. It is the most widely used data-storage model and is managed through SQL (Structured Query Language).

## 2. Why This Topic Is Important

- Almost every business system runs on a relational database.
- Concepts here (tables, keys, relationships) reappear in JDBC, ORMs, and schema design.
- Interviewers test relational fundamentals constantly.

## 3. Real-World Analogy

A database is like a **well-organized library**. Each table is a shelf section; each row is a book; each column is a fixed field on the book (title, author, year). The **catalog number** (primary key) uniquely identifies each book, and **reference cards** (foreign keys) point from one section to related books elsewhere.

## 4. Theory

### Tables, rows, columns

- **Table** — a collection of related records (e.g., `users`).
- **Row (record/tuple)** — one entry (one user).
- **Column (field/attribute)** — a property with a fixed data type.

### Keys

| Key | Purpose |
|-----|---------|
| **Primary key (PK)** | Uniquely identifies each row; non-null |
| **Foreign key (FK)** | A column referencing a PK in another table |
| **Unique key** | Ensures a value never repeats (e.g., email) |
| **Composite key** | A PK made of multiple columns |

### Relationships

- **One-to-one** — each A has at most one B (user ↔ passport).
- **One-to-many** — one A has many B (user → many orders).
- **Many-to-many** — many A relate to many B (students ↔ courses), modeled with a join table.

## 5. Syntax

```sql
CREATE TABLE users (
    id   INT PRIMARY KEY,
    name VARCHAR(100)
);
```

## 6. Flow Explanation

1. Data is written into tables with constraints enforced (types, NOT NULL, uniqueness).
2. Rows are linked through foreign keys.
3. Queries (SELECT with JOINs) combine related tables on the fly.
4. The database engine uses indexes to find rows fast.

## 7. Memory Representation

Conceptual layout of `users` and `orders`:

```
users            orders
id | name        id | user_id | total
 1 | Alice        1 |    1    | 49.99
 2 | Bob          2 |    1    | 19.99
```

`orders.user_id` → `users.id` is the foreign-key relationship.

## 8. Simple Example

```sql
CREATE TABLE employees (
    id   INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    dept VARCHAR(50)
);

INSERT INTO employees (name, dept) VALUES ('Aisha', 'Engineering');
SELECT * FROM employees;
```

## 9. Intermediate Example

```sql
CREATE TABLE departments (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    dept_id INT,
    FOREIGN KEY (dept_id) REFERENCES departments(id)
);
```

## 10. Advanced Example

```sql
-- Many-to-many via a join table
CREATE TABLE students (id INT PRIMARY KEY, name VARCHAR(100));
CREATE TABLE courses   (id INT PRIMARY KEY, title VARCHAR(100));

CREATE TABLE enrollments (
    student_id INT,
    course_id  INT,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES students(id),
    FOREIGN KEY (course_id)  REFERENCES courses(id)
);
```

## 11. Output

`SELECT * FROM employees;`

```
id | name  | dept
 1 | Aisha | Engineering
 2 | Raj   | Sales
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| No primary key | Duplicate rows, slow queries | Always define a PK |
| String columns for IDs | Slow, error-prone joins | Use integer/serial keys |
| Deleting a parent row | Orphaned children / FK errors | Use FK constraints + `ON DELETE` rules |
| Duplicating data | Update anomalies | Normalize |

## 13. Best Practices

- Every table needs a primary key.
- Use foreign keys to enforce referential integrity.
- Store one fact per column with the right data type.
- Let the database enforce constraints; don't trust the app alone.
- Name tables plural (`users`) and keys `id`/`table_id` consistently.

## 14. Interview Questions

**Q: What is a primary key?**
A column (or columns) that uniquely identifies each row and cannot be null.

**Q: What is a foreign key?**
A column referencing the primary key of another table, enforcing a relationship.

**Q: One-to-many vs many-to-many?**
One-to-many: a user has many orders (FK on orders). Many-to-many: students ↔ courses, requiring a join table.

## 15. Practice Exercises

1. Create a `products` table with an auto-increment primary key.
2. Create a `categories` table and add a foreign key from products to categories.
3. Add NOT NULL and UNIQUE constraints and try violating them.

## 16. Coding Challenges

1. Model a mini e-commerce schema: `users`, `products`, `orders`, `order_items` with proper keys and relationships.
2. Explain (in writing) the referential integrity rules for deleting a user who has orders.

## 17. Summary

- Relational databases store data in tables linked by keys.
- PK identifies rows; FK links tables; unique keys prevent duplicates.
- Relationships: one-to-one, one-to-many, many-to-many (join table).
- Constraints protect data integrity.
- Next, write data with **SQL-CRUD.md**.