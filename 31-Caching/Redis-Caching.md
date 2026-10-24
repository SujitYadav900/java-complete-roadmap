# Redis Caching

## 1. Introduction

**Redis** is an in-memory key-value store used as a **distributed cache** shared by all application instances. Unlike an in-process cache, every node sees the same data — which is essential in clustered and microservice deployments.

## 2. Why This Topic Is Important

- Distributed caches are the standard for multi-instance applications.
- Redis powers caching, sessions, rate limiting, and queues.
- Spring Boot integrates Redis caching with minimal configuration.

## 3. Real-World Analogy

A local cache is a **note pinned to your own desk**; a Redis cache is a **shared bulletin board in the office corridor**. Everyone reads and writes the same board, so the whole team stays consistent — at the cost of a short walk to the board (network round-trip).

## 4. Theory

### Redis basics

- In-memory store with optional persistence (RDB snapshots, AOF log).
- Data structures: strings, hashes, lists, sets, sorted sets.
- Common Java client: **Lettuce** (Spring Boot's default) or **Jedis**.

### Using Redis as a Spring cache

1. Add `spring-boot-starter-data-redis` (and a JSON serializer for values).
2. Configure `spring.data.redis.host`/`port`.
3. Define a `RedisCacheManager` with TTLs per cache.
4. Your existing `@Cacheable` annotations now run on Redis.

### Why TTL matters in Redis

Redis has no eviction by default — set TTLs or maxmemory policies so it does not grow forever.

## 5. Syntax

```java
// application.properties
spring.data.redis.host=localhost
spring.data.redis.port=6379
```

```java
@Bean
public RedisCacheManager cacheManager(RedisConnectionFactory cf) {
    RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
            .entryTtl(Duration.ofMinutes(10))
            .serializeValuesWith(SerializationPair.fromSerializer(
                new GenericJackson2JsonRedisSerializer()));
    return RedisCacheManager.builder(cf)
            .cacheDefaults(config)
            .build();
}
```

## 6. Flow Explanation

1. A `@Cacheable` miss triggers the real method.
2. The result is serialized to JSON and stored in Redis under the cache name + key.
3. Other instances read the same entry — a hit on any node avoids the database.
4. TTL expires entries; `@CacheEvict` removes them immediately.

## 7. Memory Representation

```
App instance A ─┐
                ├──▶ Redis (shared) : products:42 -> {...}
App instance B ─┘
```

## 8. Simple Example

```java
// dependency: spring-boot-starter-data-redis
@Cacheable(value = "products", key = "#id")
public Product getById(Long id) { ... }
```

## 9. Intermediate Example

```java
@Bean
public RedisCacheManager cacheManager(RedisConnectionFactory cf) {
    RedisCacheConfiguration defaults = RedisCacheConfiguration.defaultCacheConfig()
            .entryTtl(Duration.ofMinutes(5));
    return RedisCacheManager.builder(cf)
            .withCacheConfiguration("products",
                 defaults.entryTtl(Duration.ofMinutes(30)))
            .withCacheConfiguration("users",
                 defaults.entryTtl(Duration.ofMinutes(15)))
            .build();
}
```

## 10. Advanced Example

```java
// Manual Redis use via StringRedisTemplate
@Service
public class SessionStore {
    private final StringRedisTemplate redis;

    public void save(String token, String userId, Duration ttl) {
        redis.opsForValue().set("sess:" + token, userId, ttl);
    }

    public String findUser(String token) {
        return redis.opsForValue().get("sess:" + token);
    }
}
```

## 11. Output

From the Redis CLI:

```text
> GET products:42
{"id":42,"name":"Keyboard","price":49.99}
> TTL products:42
(integer) 1799
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| No TTL | Redis grows unbounded | Set `entryTtl` |
| Serializing Java objects to default binary | Cache unreadable/version-coupled | Use JSON serializer |
| Pointing all envs at one Redis | Dev/prod interference | Separate instances |
| Redis down = app down | Availability risk | Cache fallback + resilience |
| No password/network isolation | Data exposure | Secure the Redis instance |

## 13. Best Practices

- Always set per-cache TTLs.
- Use a JSON serializer for readable, interoperable values.
- Treat Redis as disposable — cache can be rebuilt from the source.
- Add resilience so a Redis outage degrades, not breaks, the app.
- Monitor `hitrate` and memory.

## 14. Interview Questions

**Q: Why use Redis over an in-process cache?**
Instances share one consistent store — essential for clusters and microservices where each JVM's local cache would diverge.

**Q: How does Spring Boot wire Redis into `@Cacheable`?**
Auto-configuration provides a `RedisConnectionFactory`; define a `RedisCacheManager` bean and annotations start using it.

**Q: How do you prevent Redis from growing forever?**
Set TTLs on entries, configure `maxmemory` eviction policies, and evict explicitly.

## 15. Practice Exercises

1. Start Redis (native or Docker) and add `spring-boot-starter-data-redis`.
2. Cache a service method and verify entries appear in the Redis CLI.
3. Set a 30-second TTL and watch the key expire.

## 16. Coding Challenges

1. Configure two caches with different TTLs and confirm the TTLs in Redis.
2. Add a manual `StringRedisTemplate` session store with a token → user mapping and TTL.

## 17. Summary

- Redis is a shared, distributed in-memory cache.
- Spring Boot + `RedisCacheManager` wires it into `@Cacheable`.
- Use TTLs, JSON serialization, and resilience for production.
- Redis is also used for sessions and queues (see Module 33).
- Next, tune local caches in **Caffeine-and-Local-Caches.md**.