# 32 — Microservices

> **Experience level: 🔴 Expert (3 – 5 years)**

**Microservices** split a system into independent, deployable services. This module covers architecture, service discovery, communication, and resilience patterns with Spring Cloud.

---

## 📂 Files in This Module

| File | What You'll Learn |
|------|-------------------|
| [Microservices-Overview.md](Microservices-Overview.md) | Monolith vs microservices |
| [Service-Discovery.md](Service-Discovery.md) | Eureka, registry patterns |
| [Inter-Service-Communication.md](Inter-Service-Communication.md) | REST, Feign, messaging |
| [API-Gateway.md](API-Gateway.md) | Spring Cloud Gateway |
| [Resilience.md](Resilience.md) | Circuit breaker, retries, fallback |
| [Centralized-Configuration.md](Centralized-Configuration.md) | Spring Cloud Config |
| [Exercises.md](Exercises.md) | Practice tasks for this module |

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:

1. ✅ Decide when microservices make sense.
2. ✅ Register services with Eureka.
3. ✅ Call services via Feign/RestClient.
4. ✅ Route through an API Gateway.
5. ✅ Add resilience with circuit breakers.

---

## 🧭 Module Map

```mermaid
flowchart LR
    A[Overview] --> B[Discovery]
    B --> C[Communication]
    C --> D[Gateway]
    D --> E[Resilience]
    E --> F[Central Config]
    F --> G[Exercises]
```

---

## 📌 Quick Preview

```java
@SpringBootApplication
@EnableDiscoveryClient
public class OrderService {
    public static void main(String[] args) {
        SpringApplication.run(OrderService.class, args);
    }
}
```

---

## Next Module

➡️ [34 — Testing](../34-Testing/README.md) — verify everything you've built.