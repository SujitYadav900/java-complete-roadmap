# SQL Queries

## 1. Introduction

Reading data is where SQL gets powerful. `SELECT` with **filters, sorting, joins, grouping, and aggregate functions** lets you answer business questions: "top customers", "sales per month", "items in a category with low stock".

## 2. Why This Topic Is Important

- Joins and aggregates appear in nearly every data-related interview.
- Reporting, dashboards, and analytics all run SELECT queries.
- Understanding query structure makes ORM queries and indexes easier to reason about.

## 3. Real-World Analogy

A SELECT query is like a **questionnaire for the library staff**: "Show me every book by this author, sorted by year, and count how many are in each genre." You describe what you want; the database figures out how to fetch it.

## 4. Theory

### Query clauses (in execution order)

| Clause | Purpose |
|--------|---------|
| `SELECT` | Choose columns |
| `FROM` | Choose tables |
| `WHERE` | Filter rows before grouping |
| `GROUP BY` | Group rows |
| `HAVING` | Filter groups |
| `ORDER BY` | Sort the result |
| `LIMIT` | Take only N rows |

### Joins

| Join | Result |
|------|--------|
| `INNER JOIN` | Only matching rows from both tables |
| `LEFT JOIN` | All left rows + matches (NULL if none) |
| `RIGHT JOIN` | All right rows + matches |
| `FULL OUTER JOIN` | All rows from both sides |

### Aggregates

`COUNT`, `SUM`, `AVG`, `MIN`, `MAX`.

## 5. Syntax

```sql
SELECT c.name, COUNT(o.id) AS order_count
FROM customers c
LEFT JOIN orders o ON o.customer_id = c.id
WHERE c.active = 1
GROUP BY c.id, c.name
HAVING COUNT(o.id) >= 1
ORDER BY order_count DESC
LIMIT 10;
```

## 6. Flow Explanation

1. `FROM` picks the base tables.
2. `JOIN` links them on matching keys.
3. `WHERE` filters individual rows.
4. `GROUP BY` buckets rows; aggregates compute per bucket.
5. `HAVING` filters the buckets.
6. `ORDER BY` sorts, `LIMIT` trims.

## 7. Memory Representation

An INNER JOIN conceptually:

```
customers              orders                    result
id|name                 id|customer_id|total     id|name |total
1 |Alice                10|1|49.99               1|Alice|49.99
2 |Bob                  11|1|19.99               1|Alice|19.99
```

## 8. Simple Example

```sql
SELECT name, email FROM users WHERE active = 1 ORDER BY name;
SELECT COUNT(*) FROM orders;
SELECT SUM(total) AS revenue FROM orders;
```

## 9. Intermediate Example

```sql
SELECT p.name, SUM(oi.quantity) AS units_sold
FROM products p
JOIN order_items oi ON oi.product_id = p.id
GROUP BY p.id, p.name
ORDER BY units_sold DESC;
```

## 10. Advanced Example

```sql
-- Customers with no orders (LEFT JOIN + NULL check)
SELECT c.name
FROM customers c
LEFT JOIN orders o ON o.customer_id = c.id
WHERE o.id IS NULL;

-- Monthly sales rollup
SELECT DATE_FORMAT(created_at, '%Y-%m') AS month,
       COUNT(*) AS orders,
       SUM(total) AS revenue
FROM orders
GROUP BY month
HAVING revenue > 1000
ORDER BY month;
```

## 11. Output

```
name | units_sold
Keyboard | 142
Mouse    | 98
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Filtering aggregate with `WHERE` | Error or wrong results | Use `HAVING` |
| Missing join condition (`ON`) | Cartesian product | Always specify `ON` |
| `GROUP BY` non-aggregated columns | Non-deterministic results | Group by every selected non-aggregate |
| Forgetting `LEFT JOIN` returns NULLs | Confusing results | Handle NULLs explicitly |

## 13. Best Practices

- Qualify column names (`o.total`) in multi-table queries.
- Use aliases for readability (`FROM customers c`).
- Prefer `INNER JOIN` unless you need rows with no match.
- Test aggregate queries on small data first.
- Explain/analyze slow queries to check index usage.

## 14. Interview Questions

**Q: Difference between `WHERE` and `HAVING`?**
`WHERE` filters rows before grouping; `HAVING` filters groups after aggregation.

**Q: What is the difference between `INNER JOIN` and `LEFT JOIN`?**
INNER returns only matches; LEFT returns all left-table rows, filling non-matches with NULL.

**Q: How do you get the top 5 by total?**
`SELECT ... ORDER BY total DESC LIMIT 5;`

## 15. Practice Exercises

1. Write a query that lists each user with their order count.
2. Find products with zero sales using a LEFT JOIN.
3. Compute average order total per customer, only for customers with >1 order.

## 16. Coding Challenges

1. Produce a "monthly revenue by category" report with two joins and grouping.
2. Find duplicate email addresses using `GROUP BY ... HAVING COUNT(*) > 1`.

## 17. Summary

- SELECT is declarative: describe the result, the engine finds it.
- WHERE (rows) vs HAVING (groups) is a classic gotcha.
- INNER/LEFT/RIGHT joins combine tables on keys.
- GROUP BY + aggregates answer analytical questions.
- Next, protect data with **SQL-Advanced.md**.