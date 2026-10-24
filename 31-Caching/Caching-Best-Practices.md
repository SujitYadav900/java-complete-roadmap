# Caching Best Practices

## 1. Introduction

Caches fail in predictable ways: **stale data**, **cache stampedes**, and **inconsistent multi-instance views**. This topic consolidates the production patterns that keep caches correct and fast — the difference between "caching that works" and "caching that causes outages."

## 2. Why This Topic Is Important

- A misconfigured cache can serve stale or wrong data silently.
- Stampedes (thundering herds) can knock out a database at peak load.
- Interviewers at senior level probe invalidation and consistency.

## 3. Real-World Analogy

A cache is like a **cookbook shortcut**: you memorized a recipe (cache) instead of reading the book (database). That's great until someone updates the recipe — if your memory doesn't refresh (invalidation), you keep cooking the old version. And if ten people all re-read the book at once (stampede), the kitchen gets overwhelmed.

## 4. Theory

### The core rules

1. **Cache read-heavy, slow, stable data.** Don't cache things that change every request.
2. **TTL on everything.** Unbounded entries are guaranteed staleness or memory problems.
3. **Invalidate on writes.** When the source changes, evict or refresh the entry.
4. **Bounded caches.** Size limits prevent OOM.
5. **Design for cache loss.** The source of truth must survive a full cache flush.

### Invalidation strategies

| Strategy | Behavior |
|----------|----------|
| **TTL expiry** | Entry self-expires after N seconds |
| **Write-through** | Update cache and DB together |
| **Evict-on-write** | Remove entry; next read reloads |
| **Versioning** | Key includes a version to force refresh |

### Stampede mitigation

- **Single-flight** — one thread loads a missing key; others wait.
- **Early expiration** — refresh slightly before the TTL with jitter.
- **Refresh-after-write** — serve stale while refreshing in the background.

## 5. Syntax

```java
// Single-flight with Caffeine (atomic load)
Product p = cache.get(id, this::loadFromDb);

// Jittered TTL to avoid mass expiry
Duration ttl = Duration.ofSeconds(60 + ThreadLocalRandom.current().nextInt(10));
```

## 6. Flow Explanation

1. Read hits the cache; on miss, load from source atomically (single-flight).
2. Writes invalidate/refresh the relevant keys.
3. TTLs expire entries; jitter spreads expiries.
4. Monitoring (hit rate, latency) confirms the cache is earning its memory.

## 7. Memory Representation

```
Read:  app -> cache (hit?) -> miss -> single-flight load -> DB -> cache
Write: app -> DB -> evict/refresh cache entry
Expiry: entries expire at TTL (+jitter), removed by eviction policy
```

## 8. Simple Example

```java
@CacheEvict(value = "products", key = "#id")
public void updateProduct(Long id) {
    productRepository.update(id);
}
```

## 9. Intermediate Example

```java
@Cacheable(value = "products", key = "#id", unless = "#result == null")
public Product getById(Long id) { ... }

@CacheEvict(value = "products", key = "#id")
public Product update(Long id, ProductForm form) { ... }
```

## 10. Advanced Example

```java
// Background refresh: serve stale while updating
Cache<Long, Product> cache = Caffeine.newBuilder()
        .maximumSize(100_000)
        .expireAfterWrite(Duration.ofMinutes(10))
        .refreshAfterWrite(Duration.ofMinutes(5))   // async refresh
        .build();
```

```java
// Jitter TTLs across a fleet to avoid synchronized expiry
Duration ttl = Duration.ofSeconds(300 + ThreadLocalRandom.current().nextInt(30));
```

## 11. Output

```text
Hit rate 95% | p95 latency 4ms | evictions 0 (healthy)
vs
Hit rate 40% | p95 latency 90ms | cache miss storms
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| No invalidation on writes | Stale reads | Evict/refresh on write |
| Synchronized TTLs | Stampede at expiry | Add jitter |
| Caching before measuring | Complexity without benefit | Profile first |
| No single-flight | Thundering herd | Atomic load |
| Ignoring cache-loss scenarios | Wrong results after flush | Rebuild from source |

## 13. Best Practices

- Measure before adding a cache; verify hit rate after.
- Never cache mutable per-user data without scoping the key.
- Use single-flight loads for expensive misses.
- Add jitter to TTLs in distributed deployments.
- Design every cache as a pure optimization — removing it must not break correctness.

## 14. Interview Questions

**Q: How do you keep a cache consistent with the database?**
Invalidate or update the cache in the same transaction/flow as the write; use TTL as a backstop.

**Q: What is a cache stampede and how do you prevent it?**
Mass concurrent misses hitting the source simultaneously. Prevent with single-flight loading, jittered TTLs, and background refresh.

**Q: When should you NOT cache?**
Frequently-changing data, user-specific data without proper keys, or when the source is already fast and cache misses dominate.

## 15. Practice Exercises

1. Add eviction to a service write method and verify the next read reloads.
2. Log hit/miss ratio for a workload and report it.
3. Add single-flight loading and simulate concurrent misses.

## 16. Coding Challenges

1. Build a demo where 50 threads request a missing key and prove only one DB load occurs.
2. Add jittered TTLs and show that expiries spread out over time.

## 17. Summary

- Cache read-heavy, stable data; always with TTL and bounds.
- Invalidate on writes; prefer atomic single-flight loads.
- Jitter TTLs to avoid stampedes; refresh in the background when possible.
- Measure hit rate — caches must earn their complexity.
- Caching pairs perfectly with microservices — next, **Module 32 — Microservices**.