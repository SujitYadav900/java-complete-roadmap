# Caffeine and Local Caches

## 1. Introduction

**Caffeine** is a high-performance, in-process cache library for Java — the modern replacement for Guava caches. It offers size-based eviction, time-based expiration, and async loading, and it is the recommended local cache for Spring Boot.

## 2. Why This Topic Is Important

- Local caches are the fastest layer — no network, no serialization.
- Caffeine powers Spring Boot's default local cache when you add `spring-boot-starter-cache` + `caffeine`.
- Combining Caffeine (L1) with Redis (L2) is a common high-performance pattern.

## 3. Real-World Analogy

A Caffeine cache is like a **well-organized nightstand**: the things you grab constantly (phone, glasses) sit within arm's reach. It holds only what fits in the drawer (size bound) and clears out items you haven't touched recently (LRU).

## 4. Theory

### Building a Caffeine cache

```java
Cache<Long, Product> cache = Caffeine.newBuilder()
        .maximumSize(10_000)
        .expireAfterWrite(Duration.ofMinutes(10))
        .recordStats()
        .build();
```

### Features

| Feature | Meaning |
|---------|---------|
| `maximumSize` / `maximumWeight` | Bound by entry count/weight |
| `expireAfterWrite` | Expire N after write |
| `expireAfterAccess` | Expire N after last read |
| `expireAfter(Expiry)` | Custom expiry per entry |
| `refreshAfterWrite` | Async refresh after N (keeps stale serving) |
| `recordStats` | Hit rate metrics |

### Spring integration

Add `com.github.ben-manes.caffeine:caffeine`; Spring auto-configures a `CaffeineCacheManager` using the `spring.cache.cache-names` property.

## 5. Syntax

```java
// application.properties
spring.cache.type=caffeine
spring.cache.cache-names=products,users
```

```java
@Bean
public CaffeineCacheManager cacheManager() {
    CaffeineCacheManager cm = new CaffeineCacheManager();
    cm.setCaffeine(Caffeine.newBuilder()
            .maximumSize(10_000)
            .expireAfterWrite(Duration.ofMinutes(10))
            .recordStats());
    return cm;
}
```

## 6. Flow Explanation

1. `@Cacheable("products")` goes through the CaffeineCacheManager.
2. Lookup is an in-memory map read — sub-microsecond.
3. On miss, the method runs and the value is stored with the configured TTL.
4. Size/expiry eviction keeps memory bounded.
5. Stats track hit rate for monitoring.

## 7. Memory Representation

```
JVM heap
  └─ CaffeineCache "products"  (max 10_000, TTL 10min)
       42 -> Product{...}
       7  -> Product{...}
```

## 8. Simple Example

```java
public class App {
    public static void main(String[] args) {
        Cache<String, String> cache = Caffeine.newBuilder()
                .maximumSize(100)
                .expireAfterWrite(Duration.ofSeconds(30))
                .build();

        cache.put("key", "value");
        System.out.println(cache.getIfPresent("key"));  // value
    }
}
```

## 9. Intermediate Example

```java
@Service
public class CatalogService {
    private final Cache<Long, Product> cache = Caffeine.newBuilder()
            .maximumSize(50_000)
            .expireAfterWrite(Duration.ofMinutes(30))
            .recordStats()
            .build();

    public Product getById(Long id) {
        return cache.get(id, this::loadFromDb);   // atomic load
    }
}
```

## 10. Advanced Example

```java
// Two-tier: Caffeine L1 + Redis L2
@Service
public class ProductService {
    private final Cache<Long, Product> local = Caffeine.newBuilder()
            .maximumSize(10_000)
            .expireAfterWrite(Duration.ofMinutes(2))
            .build();

    public Product getById(Long id) {
        Product p = local.getIfPresent(id);
        if (p == null) {
            p = redisTemplate.opsForValue().get("products:" + id);
            if (p == null) {
                p = repository.findById(id).orElseThrow();
                redisTemplate.opsForValue().set("products:" + id, p, Duration.ofMinutes(10));
            }
            local.put(id, p);
        }
        return p;
    }
}
```

## 11. Output

```text
Cache stats after load test:
hitRate = 0.91, evictionCount = 120
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| No size bound | OOM | `maximumSize` always |
| No TTL | Stale data | `expireAfterWrite` |
| Caching mutable shared objects | Concurrency bugs | Cache copies/immutables |
| Large objects cached | Memory pressure | Weight-based bounds |
| Ignoring stats | Can't tune | `recordStats()` |

## 13. Best Practices

- Always bound size and set an expiry.
- Use `cache.get(key, loader)` for atomic load-on-miss.
- Prefer immutable value objects.
- Use Caffeine for L1, Redis for L2, DB as source of truth.
- Monitor hit rate and tune sizes.

## 14. Interview Questions

**Q: What is Caffeine?**
A high-performance Java in-process caching library with size bounds, TTLs, and stats — the default local cache provider in Spring Boot.

**Q: How is a local cache different from Redis?**
Local is in-JVM, fastest, but per-instance; Redis is shared across instances at network cost.

**Q: How do you bound a Caffeine cache?**
`maximumSize(...)` limits entry count; `maximumWeight` limits by weight; combined with expiry policies.

## 15. Practice Exercises

1. Build a Caffeine cache with a size bound and 1-minute TTL.
2. Use `cache.get(key, loader)` and verify load-once behavior.
3. Enable `recordStats` and print the hit rate.

## 16. Coding Challenges

1. Implement the L1+L2 two-tier pattern (Caffeine + a fake Redis) and log which layer serves each read.
2. Set `refreshAfterWrite` and verify reads refresh a stale entry without blocking.

## 17. Summary

- Caffeine is the fast, in-process cache for Java.
- Bound with `maximumSize`, expire with `expireAfterWrite/Access`.
- `cache.get(key, loader)` gives atomic load-on-miss.
- Pair Caffeine (L1) with Redis (L2) for best results.
- Next, design caching safely in **Caching-Best-Practices.md**.