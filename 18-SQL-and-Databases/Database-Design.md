# Database Design

## 1. Introduction

Database design is deciding **what tables to create and how they relate** before you write SQL. A good design is easy to query, hard to corrupt, and stable as the app grows. The core tools are **ERDs** (entity-relationship diagrams) and **normalization**.

## 2. Why This Topic Is Important

- Bad schema design causes slow queries, duplicate data, and bugs that surface years later.
- Design questions ("model a bookstore") are common in interviews.
- ORMs like JPA map almost directly to your tables, so design quality matters even more.

## 3. Real-World Analogy

Designing a database is like **architecting a building**: you decide rooms (tables), doors (keys), and hallways (relationships) before any furniture arrives. Rebuilding walls later is expensive — so the plan matters.

## 4. Theory

### Steps of design

1. **Identify entities** — nouns: Customer, Order, Product.
2. **Identify relationships** — verbs: a customer *places* orders; an order *contains* products.
3. **Define attributes** — properties of each entity.
4. **Choose keys** — PK for each, FK for relationships.
5. **Normalize** — remove duplication.

### Normalization (1NF → 3NF)

| Normal form | Rule |
|-------------|------|
| **1NF** | Atomic values; no repeating groups |
| **2NF** | No partial dependency on part of a composite key |
| **3NF** | No transitive dependency (non-key depends on non-key) |

Practically: **one fact per column, nothing derived stored, no duplicated data**.

## 5. Syntax

```sql
-- Final schema from an ERD for an order system
CREATE TABLE customers (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(150) UNIQUE
);

CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT NOT NULL,
    total DECIMAL(10,2) NOT NULL DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);

CREATE TABLE order_items (
    order_id INT,
    product_id INT,
    quantity INT NOT NULL,
    price DECIMAL(10,2) NOT NULL,
    PRIMARY KEY (order_id, product_id),
    FOREIGN KEY (order_id)   REFERENCES orders(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);
```

## 6. Flow Explanation

1. Draw the ERD: entities as boxes, relationships as lines (1:N, M:N).
2. Convert each entity to a table; each attribute to a column.
3. Put FK columns on the "many" side of 1:N relationships.
4. For M:N, add a join table with two FKs.
5. Normalize to remove duplication; denormalize later only for specific performance reasons.

## 7. Memory Representation

```
customers 1 ──── N orders 1 ──── N order_items N ──── 1 products
    id          customer_id         order_id            id
    name        total               product_id          name
    email                            quantity            price
                                     price
```

## 8. Simple Example

Design a `users` + `posts` blog:

```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) NOT NULL UNIQUE
);

CREATE TABLE posts (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    title VARCHAR(200) NOT NULL,
    body TEXT,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

## 9. Intermediate Example

Add comments (users → posts → comments, two FKs on comments):

```sql
CREATE TABLE comments (
    id INT PRIMARY KEY AUTO_INCREMENT,
    post_id INT NOT NULL,
    user_id INT NOT NULL,
    body TEXT NOT NULL,
    FOREIGN KEY (post_id) REFERENCES posts(id),
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

## 10. Advanced Example

Model a **library with authors and books** (many-to-many):

```sql
CREATE TABLE authors  (id INT PRIMARY KEY AUTO_INCREMENT, name VARCHAR(100));
CREATE TABLE books    (id INT PRIMARY KEY AUTO_INCREMENT, title VARCHAR(200));

CREATE TABLE book_authors (
    book_id INT,
    author_id INT,
    PRIMARY KEY (book_id, author_id),
    FOREIGN KEY (book_id)   REFERENCES books(id),
    FOREIGN KEY (author_id) REFERENCES authors(id)
);
```

## 11. Output

Verify your design with queries that answer real questions:

```sql
SELECT u.username, COUNT(p.id) AS post_count
FROM users u LEFT JOIN posts p ON p.user_id = u.id
GROUP BY u.id, u.username;
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Storing comma-separated lists in a column | Unqueryable, violates 1NF | Join table |
| Storing derived values (age, total) | Inconsistency | Compute or update transactionally |
| No join table for M:N | Lost relationships | Add one |
| Oversized VARCHAR for everything | Waste/limits | Use correct types |

## 13. Best Practices

- Draw the ERD before writing DDL.
- Use surrogate integer PKs (`id`) and add UNIQUE on natural keys.
- Enforce FKs for every relationship.
- Store money as `DECIMAL`, never `FLOAT`.
- Keep timestamps with a consistent timezone (UTC).

## 14. Interview Questions

**Q: What is normalization and why do it?**
Structuring tables to eliminate redundancy and dependency anomalies, so updates stay consistent and queries stay simple.

**Q: How do you model a many-to-many relationship?**
With a join table containing foreign keys to both tables.

**Q: When would you denormalize?**
For read-heavy reporting where joins are too slow — accept controlled redundancy with application-maintained consistency.

## 15. Practice Exercises

1. Draw an ERD for a simple `users → orders → products` system.
2. Convert it to SQL DDL with proper keys.
3. Write a query that joins all three tables.

## 16. Coding Challenges

1. Design a schema for a **movie rental store** (customers, movies, rentals, and the fact that a movie can be rented many times by many customers).
2. Refactor a flawed design: `orders(customer_name, customer_email, item1, item2, item3)` into 3NF.

## 17. Summary

- Design from entities/relationships to ERDs to tables.
- 1NF–3NF remove duplication and anomalies.
- 1:N → FK on the many side; M:N → join table.
- Use correct types and enforce constraints.
- Next, get MySQL running in **MySQL-Getting-Started.md**.