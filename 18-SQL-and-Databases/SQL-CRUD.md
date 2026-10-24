# SQL CRUD

## 1. Introduction

CRUD stands for **Create, Read, Update, Delete** — the four fundamental operations on data. In SQL these map to `INSERT`, `SELECT`, `UPDATE`, and `DELETE`. Master these and you can interact with any relational database.

## 2. Why This Topic Is Important

- CRUD is the backbone of every application's data layer.
- JDBC, ORMs, and REST APIs all ultimately execute these four statements.
- Interview coding questions on SQL are almost always CRUD-based.

## 3. Real-World Analogy

Think of a **library's record system**: INSERT is registering a new book, SELECT is looking one up, UPDATE is correcting a misprinted author, and DELETE is removing a discarded book.

## 4. Theory

| Operation | Keyword | What it does |
|-----------|---------|--------------|
| Create | `INSERT` | Adds rows to a table |
| Read | `SELECT` | Retrieves rows |
| Update | `UPDATE` | Modifies existing rows |
| Delete | `DELETE` | Removes rows |

### Important rules

- `UPDATE` and `DELETE` need a `WHERE` — without it they affect **every** row.
- Use `SELECT *` sparingly; name the columns you need.
- Always filter with the primary key when targeting one row.

## 5. Syntax

```sql
INSERT INTO users (name, email) VALUES ('Alice', 'alice@example.com');
SELECT name, email FROM users WHERE id = 1;
UPDATE users SET email = 'new@example.com' WHERE id = 1;
DELETE FROM users WHERE id = 1;
```

## 6. Flow Explanation

1. **INSERT** adds a row; the DB assigns values and validates constraints.
2. **SELECT** reads matching rows and returns them as a result set.
3. **UPDATE** locates rows via `WHERE`, changes specified columns.
4. **DELETE** removes rows via `WHERE`.
5. Each writes/reads are subject to indexes, locks, and transactions.

## 7. Memory Representation

Result set of `SELECT id, name FROM users;` is a table-like grid:

```
id | name
 1 | Alice
 2 | Bob
```

## 8. Simple Example

```sql
INSERT INTO users (name, email) VALUES ('Alice', 'alice@example.com');
INSERT INTO users (name, email) VALUES ('Bob', 'bob@example.com');
SELECT * FROM users;
```

## 9. Intermediate Example

```sql
INSERT INTO products (name, price) VALUES ('Keyboard', 49.99);
UPDATE products SET price = 44.99 WHERE name = 'Keyboard';
SELECT name, price FROM products WHERE price < 50 ORDER BY price;
DELETE FROM products WHERE name = 'Keyboard';
```

## 10. Advanced Example

```sql
-- Insert with a subquery
INSERT INTO archived_orders (id, user_id, total)
SELECT id, user_id, total FROM orders WHERE created_at < '2025-01-01';

-- Conditional update
UPDATE accounts
SET balance = balance - 100
WHERE id = 7 AND balance >= 100;
```

## 11. Output

`SELECT id, name FROM users WHERE id = 1;`

```
id | name
 1 | Alice
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| `UPDATE` without `WHERE` | Every row changed | Always filter |
| `DELETE` without `WHERE` | Table emptied | Add the filter; test with SELECT |
| Inserting wrong column types | Constraint error | Match table schema |
| `SELECT *` in production code | Too much data transferred | Select needed columns |

## 13. Best Practices

- Test `WHERE` clauses with `SELECT` before `UPDATE`/`DELETE`.
- Use transactions for multi-statement writes (see SQL-Advanced).
- Use parameterized queries in applications to prevent SQL injection.
- Return only the columns you need.
- Prefer soft deletes (a `deleted` flag) for important data if required.

## 14. Interview Questions

**Q: What is the difference between `DELETE` and `TRUNCATE`?**
`DELETE` removes rows (can be filtered, logged, rolled back); `TRUNCATE` removes all rows and resets identity, and cannot be rolled back as easily.

**Q: What happens if you run `UPDATE` without `WHERE`?**
Every row in the table is updated.

**Q: How do you safely delete one row?**
`DELETE FROM users WHERE id = <pk>` — filter by primary key.

## 15. Practice Exercises

1. Create a `books` table and insert three rows.
2. Select only title and author for one row.
3. Update the price of one book and delete another.

## 16. Coding Challenges

1. Write a statement that inserts a row only if a unique email doesn't already exist.
2. Update a balance while guaranteeing it never goes negative (see conditional update above).

## 17. Summary

- CRUD = INSERT / SELECT / UPDATE / DELETE.
- Always scope UPDATE/DELETE with `WHERE`.
- Filter by primary key to target single rows.
- Use parameters in apps to avoid injection.
- Next, learn to combine and filter data with **SQL-Queries.md**.