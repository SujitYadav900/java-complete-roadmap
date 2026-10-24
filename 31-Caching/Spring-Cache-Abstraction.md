# Spring Cache Abstraction

## 1. Introduction

Spring's **cache abstraction** adds caching to services with **annotations** instead of manual code. Declarative annotations — `@Cacheable`, `@CacheEvict`, `@CachePut` — are backed by a pluggable `CacheManager` (Caffeine, Redis, Simple, etc.).

## 2. Why This Topic Is Important

- Adding a cache becomes a one-line annotation change.
- The abstraction lets you swap backends (local → Redis) without touching business code.
- Interviewers ask about cache keys, invalidation, and `@Cacheable` semantics.

## 3. Real-World Analogy

Spring's cache abstraction is like a **concierge with a clipboard**. You (the code) just tell the concierge "remember this answer for product 42" (`@Cacheable`). The concierge decides whether to use a notepad, a safe, or a warehouse (the cache provider) — you don't care which.

## 4. Theory

### The annotations

| Annotation | Behavior |
|------------|----------|
| `@Cacheable` | Check cache; on miss, run the method and store the result |
| `@CacheEvict` | Remove entries (after writes/deletes) |
| `@CachePut` | Always run the method and update the cache |
| `@Caching` | Combine multiple of the above |

### Key concepts

- **Cache name** — a named region: `@Cacheable("products")`.
- **Key** — defaults to all method args; customize with `key = "#id"`.
- **Conditional** — `condition`/`unless` restrict caching.
- **Self-invocation** — calling a `@Cacheable` method from within the same bean **bypasses** the cache (proxy limitation).

### Enabling caching

```java
@EnableCaching   // on a @Configuration or the main app class
```

## 5. Syntax

```java
@Cacheable(value = "products", key = "#id")
public Product getById(Long id) {
    return productRepository.findById(id).orElseThrow();
}

@CacheEvict(value = "products", key = "#id")
public void delete(Long id) { ... }

@CachePut(value = "products", key = "#product.id")
public Product save(Product product) { ... }
```

## 6. Flow Explanation

1. Spring creates a proxy around the bean.
2. Call `getById(42)` → proxy checks the `products` cache for key `42`.
3. Hit → return cached object, method never runs.
4. Miss → run the real method, store result under key `42`.
5. `@CacheEvict` removes the key after a write; `@CachePut` refreshes it.

## 7. Memory Representation

```
getById(42)
  └─> proxy: products["42"]?
        ├─ hit  → return
        └─ miss → real method → store products["42"]
```

## 8. Simple Example

```java
@Cacheable("greetings")
public String greet(String name) {
    log.info("Computing greeting for {}", name);   // only on miss
    return "Hello, " + name;
}
```

## 9. Intermediate Example

```java
@Cacheable(value = "products", key = "#id", unless = "#result == null")
public Product getById(Long id) {
    return productRepository.findById(id).orElse(null);
}

@CacheEvict(value = "products", key = "#id")
public void deleteProduct(Long id) {
    productRepository.deleteById(id);
}
```

## 10. Advanced Example

```java
@Caching(evict = {
    @CacheEvict(value = "products", allEntries = true),
    @CacheEvict(value = "product-list", allEntries = true)
})
public void resetAllProducts() { ... }
```

```java
// Cache the first 10 users, skip the rest
@Cacheable(value = "users", key = "#id", condition = "#id <= 10")
public User getUser(Long id) { ... }
```

## 11. Output

```text
First call:  Computing greeting for Alice      <- miss
Second call: (no log)                          <- hit, from cache
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Self-invocation | Cache bypassed silently | Inject the bean, or refactor |
| Not setting `key` | Wrong cache entries for composite keys | Use SpEL key |
| Caching mutable objects | Shared-state bugs | Cache immutable copies |
| No `@EnableCaching` | Annotations ignored | Enable caching |
| Never evicting on writes | Stale data | `@CacheEvict`/`@CachePut` |

## 13. Best Practices

- Name caches clearly (`"products"`, `"products-by-category"`).
- Use `unless` to avoid caching nulls where that matters.
- Evict on writes, or use `@CachePut` to refresh.
- Prefer immutable value objects in caches.
- Test cache behavior with integration tests.

## 14. Interview Questions

**Q: How does `@Cacheable` work under the hood?**
Spring wraps the bean in a proxy; on invocation it checks the cache manager for the key, and on miss runs the method and stores the result.

**Q: What is the self-invocation problem?**
A `@Cacheable` method called from inside the same bean bypasses the proxy, so the cache logic never runs.

**Q: `@CacheEvict` vs `@CachePut`?**
Evict removes an entry (reload on next read); Put runs the method and writes the result into the cache immediately.

## 15. Practice Exercises

1. Add `@EnableCaching` and a `@Cacheable` method; verify the method runs once.
2. Customize the key with SpEL and call with different arguments.
3. Evict on an update and confirm the next read reloads.

## 16. Coding Challenges

1. Build a service with both a `@Cacheable` read and `@CacheEvict` write, and prove hit/miss behavior with logs.
2. Use `allEntries = true` to clear a whole cache region after a bulk operation.

## 17. Summary

- `@Cacheable` / `@CacheEvict` / `@CachePut` add caching declaratively.
- Keys, conditions, and `unless` control caching precisely.
- Enable with `@EnableCaching`; watch for self-invocation.
- Evict or put on writes to avoid staleness.
- Next, run caches in Redis with **Redis-Caching.md**.