# Caching Exercises

> Practice the caching concepts from Module 31. Use Spring Boot with `spring-boot-starter-cache` and a simple in-memory cache; add Redis for the distributed exercises.

## 1. Beginner Exercises

1. Add `@EnableCaching` and a `@Cacheable` method that logs every call.
2. Call it twice with the same argument and confirm the method runs once.
3. Call it with a different argument and confirm a separate entry.

## 2. Key & Eviction Exercises

1. Customize the cache key with SpEL: `key = "#id"`.
2. Add `@CacheEvict` to an update method and verify the next read reloads.
3. Use `allEntries = true` to clear the whole cache region.

## 3. Conditional Exercises

1. Add `unless = "#result == null"` and confirm nulls are not cached.
2. Add a `condition` that only caches ids under a threshold.
3. Verify behavior with logs.

## 4. Caffeine Exercises

1. Add the Caffeine dependency and configure a `CaffeineCacheManager` with `maximumSize` and `expireAfterWrite`.
2. Enable `recordStats` and log the hit rate after a small load.
3. Use `cache.get(key, loader)` to prove single-flight loading.

## 5. Redis Exercises

1. Start Redis and add `spring-boot-starter-data-redis`.
2. Define a `RedisCacheManager` with a 2-minute TTL.
3. Verify keys appear in the Redis CLI and expire on time.

## 6. Challenge: Two-tier cache

1. Implement Caffeine (L1) + Redis (L2) + repository (source) reads.
2. Log which layer serves each request.
3. Confirm a Redis flush still recovers from the repository.

## 7. Challenge: Stampede prevention

1. Simulate 50 concurrent misses on one key with `cache.get(key, loader)`.
2. Count repository invocations — they must be 1.
3. Add jittered TTLs and observe expiry spread.

> **You've mastered caching.** Move to **[32 — Microservices](../32-Microservices/README.md)** — split the monolith into services.