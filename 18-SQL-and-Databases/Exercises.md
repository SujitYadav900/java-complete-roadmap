# SQL Exercises

> Practice the SQL concepts from Module 18. Use MySQL (or any relational DB) and the `myapp` database.

## 1. Beginner Exercises

1. Create a database `store` with `utf8mb4`.
2. Create a `products` table: `id`, `name`, `price`, `stock`.
3. Insert five products.
4. Select all products ordered by price descending.

## 2. CRUD Exercises

1. Update the price of one product.
2. Delete a product by primary key.
3. Insert a product with only the required columns and observe defaults.
4. Run `SELECT * FROM products` after each operation to verify.

## 3. Query Exercises

1. Find products with `stock < 10`.
2. Compute the total value of stock: `SUM(price * stock)`.
3. Group products by a `category` column and count per category.

## 4. Join Exercises

1. Create `customers` and `orders` (FK to customers).
2. Write a `LEFT JOIN` that shows all customers and their order totals (or NULL).
3. Find customers with no orders.

## 5. Constraints & Transactions

1. Add a `CHECK (price >= 0)` constraint and try to violate it.
2. Write a two-step transaction (transfer between two accounts) and roll it back.
3. Create an index on `orders(customer_id)` and run `EXPLAIN` to confirm it's used.

## 6. Challenge: Design a schema

1. Design and create a `blog` database: `users`, `posts`, `comments`.
2. Enforce FKs and NOT NULL constraints.
3. Write a query joining all three tables to show each post with author and comment count.

## 7. Challenge: Reporting query

1. Produce a report of revenue per month using `GROUP BY` on `created_at`.
2. Add a `HAVING` filter for months above a threshold.
3. Verify the output with a plain `SELECT`.

> **You've mastered SQL.** Move to **[19 — JDBC](../19-JDBC/README.md)** — connect Java to the database you just built.