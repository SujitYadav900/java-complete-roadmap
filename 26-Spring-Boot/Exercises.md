# Exercises — Spring Boot

## 1. Introduction

Practice the Spring Boot workflow end-to-end: scaffolding, REST controllers, configuration, monitoring, and deployment.

---

## 2. Why This Topic Is Important

- Boot is the industry standard for Java APIs.
- These skills are used daily in real projects.
- Hands-on practice = job-ready confidence.

---

## 3. Real-World Analogy

Like a **test drive before buying**: you already know Spring (module 25); now you drive the full Boot experience — start, configure, monitor, and deploy.

---

## 4. Theory — Topic Map

| Topic | Key Skill |
|-------|-----------|
| Overview | Starters + auto-config |
| Getting started | Initializr + wrapper |
| Controllers | REST endpoints |
| Properties | Profiles + config |
| Annotations | @SpringBootApplication |
| Actuator | Health + metrics |
| Deployment | JAR + Docker |

---

## 5. Syntax Reminder

```java
@SpringBootApplication
@RestController
class App {
    @GetMapping("/x") public String x() { return "ok"; }
}
```

```bash
./mvnw spring-boot:run
java -jar target/app.jar
```

---

## 6. Practice Flow

```mermaid
flowchart LR
    A[Scaffold] --> B[Write controller]
    B --> C[Configure]
    C --> D[Run]
    D --> E[Actuator check]
    E --> F[Deploy]
```

---

## 7. Memory Representation

Skip — practice module.

---

## 8. Simple Exercises

**Ex 1 — Scaffold:** Generate a Boot project with web starter.

**Ex 2 — Ping:** A /ping endpoint returning "pong".

**Ex 3 — Config:** Set a custom port via properties.

**Ex 4 — Params:** Greet endpoint with a name param + default.

---

## 9. Intermediate Exercises

**Ex 5 — CRUD:** In-memory CRUD controller with all verbs.

**Ex 6 — Profiles:** dev/prod profiles with distinct messages.

**Ex 7 — @Value:** Read app.name/version and expose /info.

**Ex 8 — Actuator:** Enable health+info; check JSON.

**Ex 9 — Scheduling:** A @Scheduled job every 10 seconds.

---

## 10. Advanced Exercises

**Ex 10 — DTO + ResponseEntity:** Return a DTO with custom status.

**Ex 11 — ConfigurationProperties:** Group config into a typed bean.

**Ex 12 — Health Indicator:** Custom indicator for a dependency.

**Ex 13 — Docker:** Multi-stage Dockerfile + container run.

**Ex 14 — Full Service:** A mini REST service with config, actuator, and Docker deploy.

---

## 11. Expected Output Samples

**Ex 2:**
```
GET /ping → pong
```

**Ex 6 (dev):**
```
GET /env → DEV environment
```

**Ex 12 (up):**
```
{"status":"UP","details":{"database":"reachable"}}
```

---

## 12. Common Mistakes

| Mistake | Fix |
|---------|-----|
| No web starter | Endpoints 404 |
| Main class not at root | Beans missed |
| Actuator not exposed | Set exposure.include |
| Hardcoded secrets | Use env vars |
| Health probe missing | k8s can't route |

---

## 13. Best Practices

- Spring Initializr for scaffolding.
- Root-package main class.
- Profile-based config; env vars for secrets.
- Actuator health in every deploy.
- DTOs, not entities, at the API boundary.

---

## 14. Interview Practice Questions

**Q1.** What does @SpringBootApplication do?
**Q2.** How do you build a REST endpoint?
**Q3.** How do you switch environments?
**Q4.** What does Actuator provide?
**Q5.** How do you deploy a Boot app?

---

## 15. Self-Check

- [ ] I can scaffold and run a Boot app.
- [ ] I can build REST controllers.
- [ ] I can configure with properties/profiles.
- [ ] I can monitor with Actuator.
- [ ] I can deploy with Docker.

---

## 16. Next Steps

- Design real APIs in [27 — REST API](../27-REST-API/README.md).
- Secure them in [29 — Spring Security](../29-Spring-Security/README.md).

---

## 17. Summary

- Boot = fast, auto-configured Spring.
- Controllers, properties, actuator, deployment covered.
- Root package, profiles, DTOs, health checks.
- Practice end-to-end: scaffold → code → run → monitor → deploy.
- Boot mastery is the foundation for the REST module next.

➡️ Module complete! Continue to [27 — REST API](../27-REST-API/README.md).