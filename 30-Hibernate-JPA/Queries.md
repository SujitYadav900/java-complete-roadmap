# Queries

## 1. Introduction

Beyond derived queries, JPA offers **JPQL** (object queries) and **native SQL** via `@Query`. This is where complex data access happens.

---

## 2. Why This Topic Is Important

- Complex queries can't always be derived method names.
- @Query gives full control and performance.
- JPQL vs native SQL is a classic interview question.
- Modifying queries (@Modifying) are needed for bulk updates.

---

## 3. Real-World Analogy

JPQL is like asking a **librarian in shelf terms**: "give me books in the sci-fi section, sorted by year" — you talk about the library's organization (entities), not the physical building. Native SQL is pointing directly at the building's wiring — powerful but tied to the structure.

---

## 4. Theory

### JPQL vs Native SQL
| Aspect | JPQL | Native SQL |
|--------|------|------------|
| Works on | Entities | Tables/columns |
| Portable | Yes | Database-specific |
| Performance | Good | Max control |
| Syntax | `FROM Product p` | `FROM product` |

### @Query Styles
```java
// JPQL
@Query("SELECT p FROM Product p WHERE p.price > :min")
List<Product> expensive(@Param("min") double min);

// Native SQL
@Query(value = "SELECT * FROM product WHERE price > :min",
       nativeQuery = true)
List<Product> expensiveNative(@Param("min") double min);

// Update
@Modifying
@Query("UPDATE Product p SET p.price = :newPrice WHERE p.id = :id")
int updatePrice(@Param("id") Long id, @Param("newPrice") double newPrice);
```

### @Param Bindings
- Named: `:name` with `@Param("name")`.
- Positional: `?1`, `?2` (legacy).

---

## 5. Syntax

```java
@Query("SELECT p FROM Product p WHERE p.name LIKE %:part%")
List<Product> search(@Param("part") String part);
```

---

## 6. Flow Explanation

```mermaid
flowchart LR
    A[@Query annotation] --> B[Parse JPQL/SQL]
    B --> C[Bind params]
    C --> D[Execute]
    D --> E[Map results]
```

---

## 7. Memory Representation

Skip — query definitions.

---

## 8. Simple Example — JPQL Select

```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;
import java.util.List;

public interface ProductRepository extends JpaRepository<Product, Long> {

    @Query("SELECT p FROM Product p WHERE p.price > :min ORDER BY p.price DESC")
    List<Product> findExpensive(@Param("min") double min);

    @Query("SELECT p FROM Product p WHERE LOWER(p.name) LIKE LOWER(CONCAT('%', :part, '%'))")
    List<Product> findByNamePart(@Param("part") String part);
}
```

---

## 9. Intermediate Example — Native SQL + Modifying

```java
import org.springframework.data.jpa.repository.*;
import org.springframework.transaction.annotation.Transactional;

public interface ProductRepository extends JpaRepository<Product, Long> {

    @Query(value = "SELECT * FROM product WHERE price > :min", nativeQuery = true)
    List<Product> expensiveNative(@Param("min") double min);

    @Modifying
    @Transactional
    @Query("UPDATE Product p SET p.price = :price WHERE p.id = :id")
    int updatePrice(@Param("id") Long id, @Param("price") double price);

    @Modifying
    @Transactional
    @Query("DELETE FROM Product p WHERE p.price < :min")
    int deleteCheap(@Param("min") double min);
}
```

---

## 10. Advanced Example — Projection Query

```java
import org.springframework.data.jpa.repository.*;

// Interface projection
interface ProductSummary {
    String getName();
    double getPrice();
}

public interface ProductRepository extends JpaRepository<Product, Long> {

    @Query("SELECT p.name AS name, p.price AS price FROM Product p")
    List<ProductSummary> summaries();
}

@RestController
@RequestMapping("/api/products")
class ProductApi {

    private final ProductRepository repo;

    ProductApi(ProductRepository repo) { this.repo = repo; }

    @GetMapping("/summaries")
    public java.util.List<ProductSummary> summaries() {
        return repo.summaries();
    }
}
```

---

## 11. Output

For the **Simple Example**:

```
findExpensive(500)
Hibernate: select p1_0.* from product p1_0 where p1_0.price > ? order by p1_0.price desc
```

For the **Intermediate Example**:

```
updatePrice(3, 999.0)
Hibernate: update product set price=? where id=?
→ rows affected: 1
```

For the **Advanced Example**:

```
GET /api/products/summaries →
[{"name":"Laptop","price":1200.0},{"name":"Mouse","price":25.0}]
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| Native SQL for everything | Use JPQL when possible |
| Forgetting @Modifying for updates | Query fails |
| Missing @Transactional on @Modifying | Add it |
| SQL injection risk | Use :params, never concat |
| Wrong entity field names | Use Java property names |

---

## 13. Best Practices

- Use JPQL for portability.
- Native SQL only when necessary.
- Always bind params (no concatenation).
- @Modifying + @Transactional for updates.
- Use projections to fetch only needed fields.

---

## 14. Interview Questions

**Q1. JPQL vs native SQL?**
*JPQL queries entities (portable); native SQL uses tables (DB-specific).*

**Q2. What is @Modifying?*
*Marks update/delete queries that change data.*

**Q3. How do you bind parameters?**
*:name + @Param.*

**Q4. What is a projection?**
*Selecting only some fields via interface or DTO.*

**Q5. Why not concatenate SQL?**
*SQL injection — always bind.*

---

## 15. Practice Exercises

1. Write a JPQL query by price.
2. Write a native query.
3. Add a @Modifying update.
4. Create an interface projection.

---

## 16. Coding Challenges

1. **Easy:** JPQL search by name.
2. **Medium:** Bulk price update + delete.
3. **Hard:** Projection summaries + aggregation query.

---

## 17. Summary

- @Query for complex queries (JPQL preferred).
- Native SQL when needed.
- @Modifying + @Transactional for changes.
- Always bind parameters.
- Projections fetch only what you need.

➡️ Continue to: [Relationships](Relationships.md)