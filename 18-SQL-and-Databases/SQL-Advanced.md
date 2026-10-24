# SQL Advanced

## 1. Introduction

Advanced SQL covers the features that keep data **correct, fast, and safe**: constraints, indexes, transactions, and the ACID guarantees that make databases reliable even when things go wrong.

## 2. Why This Topic Is Important

- Transactions prevent corrupt data (money moving twice, half-orders).
- Indexes turn slow full-table scans into instant lookups.
- Constraints enforce business rules at the database layer.
- Every backend interview touches these topics.

## 3. Real-World Analogy

- **Transactions** are like a **bank transfer**: either the full money moves (commit) or none of it does (rollback). No intermediate "half-transferred" state.
- **Indexes** are like the **alphabetical tabs in a filing cabinet** — you jump straight to the drawer instead of reading every folder.
- **Constraints** are like the **rules printed on a form** (fields required, values in range).

## 4. Theory

### Constraints

| Constraint | Rule |
|------------|------|
| `NOT NULL` | Column must have a value |
| `UNIQUE` | Values never repeat |
| `PRIMARY KEY` | UNIQUE + NOT NULL + table identity |
| `FOREIGN KEY` | Value must exist in the referenced table |
| `CHECK` | Value must satisfy an expression |
| `DEFAULT` | Fallback value if none provided |

### Transactions & ACID

- **Atomicity** — all or nothing.
- **Consistency** — DB moves between valid states.
- **Isolation** — concurrent transactions don't interfere.
- **Durability** — committed changes survive crashes.

Commands: `BEGIN`, `COMMIT`, `ROLLBACK`.

### Indexes

An index speeds lookups/filtering/joins on a column at the cost of slower writes and extra storage. `PRIMARY KEY` and `UNIQUE` constraints create indexes automatically.

## 5. Syntax

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

```sql
CREATE INDEX idx_orders_customer ON orders (customer_id);
```

## 6. Flow Explanation

1. **Constraints** are validated on every INSERT/UPDATE.
2. **Transaction**: BEGIN opens one; writes are invisible to others until COMMIT.
3. On any error, ROLLBACK undoes the whole transaction.
4. **Indexes** are maintained by the engine as rows change, so SELECTs can skip full scans.
5. The optimizer chooses whether to use an index.

## 7. Memory Representation

A transaction lifecycle:

```
BEGIN → writes (locked) → COMMIT (visible)  |  or  → ROLLBACK (undone)
```

An index is a sorted structure mapping key → row location (like a hash/tree).

## 8. Simple Example

```sql
BEGIN;
INSERT INTO accounts (owner, balance) VALUES ('Alice', 500);
COMMIT;
```

## 9. Intermediate Example

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
-- if the second statement fails, roll back everything
ROLLBACK;
```

## 10. Advanced Example

```sql
-- Composite index for a common query pattern
CREATE INDEX idx_orders_customer_status
ON orders (customer_id, status);

-- Check constraint
ALTER TABLE products
ADD CONSTRAINT chk_price CHECK (price >= 0);
```

## 11. Output

```sql
SELECT * FROM users WHERE email = 'x@y.com';
-- with index on email: rows examined = 1 (fast)
-- without index: full table scan
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Forgetting COMMIT | Locks held, changes lost | Commit explicitly |
| Indexing every column | Slow writes, big storage | Index on WHERE/JOIN columns |
| Missing FK constraints | Orphaned rows | Add FKs |
| Not using transactions for multi-writes | Partial updates | Wrap in BEGIN/COMMIT |

## 13. Best Practices

- Wrap multi-statement business changes in transactions.
- Index columns used in `WHERE`, `JOIN`, `ORDER BY` — not all columns.
- Add `CHECK` and FK constraints at schema creation.
- Keep transactions short to reduce lock contention.
- Use `EXPLAIN` to verify indexes are used.

## 14. Interview Questions

**Q: Explain ACID.**
Atomicity (all-or-nothing), Consistency (valid states), Isolation (concurrent safety), Durability (persists after crash).

**Q: When does an index help and when does it hurt?**
Helps reads (filters/joins); hurts writes (maintenance cost) and storage.

**Q: What happens if a transaction rolls back?**
All changes made inside it are undone; the database returns to its pre-transaction state.

## 15. Practice Exercises

1. Write a fund-transfer transaction and test ROLLBACK.
2. Create an index on a frequently-filtered column and run `EXPLAIN`.
3. Add a CHECK constraint and try to insert an invalid value.

## 16. Coding Challenges

1. Create a schema where deleting a customer with orders is blocked by an FK — then decide and implement an `ON DELETE` strategy.
2. Write a transaction that deducts stock and inserts an order, failing atomically if stock is insufficient.

## 17. Summary

- Constraints enforce integrity: NOT NULL, UNIQUE, PK, FK, CHECK.
- Transactions give ACID guarantees; COMMIT or ROLLBACK.
- Indexes accelerate reads at the cost of writes/storage.
- Keep transactions short; verify with EXPLAIN.
- Next, design good schemas in **Database-Design.md**.