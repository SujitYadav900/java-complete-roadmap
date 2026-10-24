# 26 — Spring Boot

> **Experience level: 🔴 Expert (3 – 5 years)**

**Spring Boot** turns Spring into fast, production-ready apps with auto-configuration, embedded servers, and starters. This module covers the Boot fundamentals: setup, REST, configuration, and deployment.

---

## 📂 Files in This Module

| File | What You'll Learn |
|------|-------------------|
| [Spring-Boot-Overview.md](Spring-Boot-Overview.md) | Why Boot, starters, auto-config |
| [Getting-Started.md](Getting-Started.md) | Setup, Spring Initializr, first app |
| [Controllers-and-REST.md](Controllers-and-REST.md) | @RestController and REST APIs |
| [Application-Properties.md](Application-Properties.md) | Configuration and profiles |
| [Spring-Boot-Annotations.md](Spring-Boot-Annotations.md) | @SpringBootApplication & friends |
| [Actuator.md](Actuator.md) | Monitoring and health checks |
| [Deployment.md](Deployment.md) | Packaging to JAR, running anywhere |
| [Exercises.md](Exercises.md) | Practice tasks for this module |

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:

1. ✅ Scaffold a Spring Boot app with Spring Initializr.
2. ✅ Build REST endpoints with @RestController.
3. ✅ Configure apps with application.properties and profiles.
4. ✅ Monitor with Actuator.
5. ✅ Package and run as a standalone JAR.

---

## 🧭 Module Map

```mermaid
flowchart LR
    A[Boot Overview] --> B[Getting Started]
    B --> C[Controllers & REST]
    C --> D[Application Properties]
    D --> E[Boot Annotations]
    E --> F[Actuator]
    F --> G[Deployment]
    G --> H[Exercises]
```

---

## 📌 Quick Preview

```java
@RestController
class HelloController {

    @GetMapping("/hello")
    public String hello(@RequestParam String name) {
        return "Hello, " + name + "!";
    }
}
```

---

## Next Module

➡️ [27 — REST API](../27-REST-API/README.md) — design and build real REST services.