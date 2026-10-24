# Caching Concepts

## 1. Introduction

Caching stores **frequently accessed results** so later requests skip slow work — database queries, external API calls, expensive computations. A cache is a fast, temporary store placed between the application and a slower source of truth.

## 2. Why This Topic Is Important

- Caching is the #1 performance lever for read-heavy applications.
- Every high-traffic backend uses one or more cache layers.
- Interviews ask about cache-aside, TTL, eviction, and invalidation.

## 3. Real-World Analogy

Caching is like a **shopping list on the fridge**. You write the items you buy every week (hot data). Instead of walking the whole store each time, you glance at the list (cache hit) and only walk the aisles for items not on it (cache miss, the slow path).

## 4. Theory

### Key terms

| Term | Meaning |
|------|---------|
| **Cache hit** | Data found in the cache — fast |
| **Cache miss** | Data not found — go to source, then store it |
| **TTL** | Time-to-live: how long an entry stays valid |
| **Eviction** | Removing entries when the cache is full |
| **Cache-aside** | App checks cache, on miss loads from source and populates |
| **Write-through** | Writes go to cache and source together |

### Eviction policies

| Policy | Behavior |
|--------|----------|
| **LRU** | Evict least-recently-used entries |
| **LFU** | Evict least-frequently-used entries |
| **FIFO** | Evict oldest first |
| **TTL-based** | Evict expired entries |

### Where caches live

- **Local (in-process)** — inside the JVM (Caffeine). Fastest; not shared across instances.
- **Distributed** — shared store (Redis/Memcached). Consistent across instances; adds a network round-trip.

## 5. Syntax

```java
// cache-aside, manually
public Product getById(Long id) {
    Product p = cache.get(id);            // 1. try cache
    if (p == null) {                      // 2. miss
        p = repository.findById(id);      // 3. load source
        cache.put(id, p);                 // 4. populate
    }
    return p;
}
```

## 6. Flow Explanation

1. Request asks for a key.
2. Cache lookup: hit → return instantly.
3. Miss → fetch from the slow source.
4. Store the result with a TTL.
5. Return; subsequent requests hit until TTL, eviction, or invalidation.

## 7. Memory Representation

```
Request --> Cache (fast, bounded) --miss--> Database (slow)
              | hit
              v
            return fast
```

## 8. Simple Example

```java
public class SimpleCache {
    private final Map<String, CachedValue> store = new HashMap<>();
    private static final long TTL_MS = 60_000;

    public String getOrLoad(String key, Supplier<String> loader) {
        CachedValue e = store.get(key);
        if (e != null && System.currentTimeMillis() - e.createdAt < TTL_MS) {
            return e.value;                    // hit
        }
        String value = loader.get();           // miss
        store.put(key, new CachedValue(value, System.currentTimeMillis()));
        return value;
    }
}
```

## 9. Intermediate Example

```java
// Cache-aside with explicit refresh
public class WeatherService {
    private final Map<String, Weather> cache = new HashMap<>();

    public Weather getWeather(String city) {
        return cache.computeIfAbsent(city, this::fetchFromApi);
    }

    public void refresh(String city) {
        cache.put(city, fetchFromApi(city));   // force reload
    }
}
```

## 10. Advanced Example

```java
// Two-tier cache: local L1 + distributed L2
public Product getById(Long id) {
    Product p = localCache.getIfPresent(id);      // L1
    if (p == null) {
        p = redisCache.get(id, Product.class);    // L2
        if (p == null) {
            p = repository.findById(id);          // source
            redisCache.put(id, p);
        }
        localCache.put(id, p);
    }
    return p;
}
```

## 11. Output

Typical performance:

```text
Cold (no cache):  98 ms
Warm (cache hit):   3 ms
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| No TTL | Stale data forever | Set a sensible TTL |
| Caching user-specific data globally | Data leaks | Key by user/permission |
| Unbounded cache | OutOfMemoryError | Bound size + eviction |
| Forgetting invalidation on writes | Stale reads | Evict/update on write |

## 13. Best Practices

- Cache read-heavy, slow, rarely-changing data.
- Always set a TTL and a size bound.
- Invalidate or update the cache on writes.
- Cache only serializable, immutable values.
- Measure the hit ratio — if it's low, the cache is not helping.

## 14. Interview Questions

**Q: What is cache-aside?**
The application checks the cache first; on a miss it loads from the source and populates the cache. Most Spring caching uses this pattern.

**Q: What is a cache stampede (thundering herd)?**
Many requests miss simultaneously and all hit the source. Mitigations: locking/single-flight loads and probabilistic early expiration.

**Q: Local vs distributed cache?**
Local (Caffeine) is in-process and fastest but not shared; distributed (Redis) is shared across instances at network cost.

## 15. Practice Exercises

1. Implement a simple TTL cache with a `HashMap`.
2. Add an LRU-style bound by size.
3. Simulate cache-aside with a fake slow repository and log hit/miss.

## 16. Coding Challenges

1. Build a single-flight loader that lets only one thread load a missing key (others wait) — preventing a stampede.
2. Add explicit invalidation on an update and verify the next read reloads.

## 17. Summary

- Caches trade memory for speed on read-heavy paths.
- Patterns: cache-aside, write-through; policies: LRU/LFU/TTL.
- Local caches are fast but per-instance; distributed caches are shared.
- TTL, bounds, and invalidation prevent staleness and OOM.
- Next, apply these via Spring in **Spring-Cache-Abstraction.md**.