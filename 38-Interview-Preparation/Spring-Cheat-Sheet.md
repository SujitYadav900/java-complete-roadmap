# Spring Cheat Sheet

## 1. Introduction

Condense modules 24–33 into interview-ready nuggets: Spring Core, Boot, MVC, JPA, Security, Caching, and Microservices. One section per framework area with the exact answers interviewers want.

---

## 2. Why It's Important

- Spring is the #1 skill on Java job postings.
- Behavioral + technical: you'll be asked "how does Spring do X?" constantly.

---

## 3. Real-World Analogy

Spring is a **framework that assembles your app's employees**: it creates the objects (beans), wires who reports to whom (DI), and gives them standard uniforms (annotations) so your code just does the actual work.

---

## 4. Theory — The Core Concepts

| Concept | One-Liner |
|---------|-----------|
| IoC Container | Manages beans and their lifecycle |
| Dependency Injection | Spring supplies dependencies, not `new` |
| `@Component` vs `@Bean` | Component: your class. Bean: config method |
| Auto-configuration | Boot configures based on classpath |
| `@Transactional` | Proxy wraps method in a transaction |
| `@RestController` | Controller + JSON responses |
| JPA | Map objects to tables, queries generated |
| Security filter chain | Requests filtered → auth → authorization |

---

## 5. Syntax — The Patterns to Quote

```java
// DI: constructor injection (preferred)
@Service
public class OrderService {
    private final OrderRepository repo;
    public OrderService(OrderRepository repo) { this.repo = repo; }
}

// Transaction
@Transactional
public void transfer() { ... }

// Cache
@Cacheable("products")
public Product get(Long id) { ... }

// Security: permit/authenticate
.authorizeHttpRequests(auth -> auth
    .requestMatchers("/api/auth/**").permitAll()
    .anyRequest().authenticated())
```

---

## 6. Flow — A Request in Spring

```mermaid
flowchart TD
    A[HTTP request] --> B[Filter chain: security, jwt]
    B --> C[DispatcherServlet]
    C --> D[HandlerMapping → Controller]
    D --> E[Service (tx proxy)]
    E --> F[Repository / JPA]
    F --> G[(DB)]
    G --> H[JSON via Jackson]
```

---

## 7. Memory Representation

Spring holds beans in the **application context** (a map of bean names → instances). Scopes: `singleton` (one shared), `prototype` (new each time), `request`/`session` (per web request/session). Transactions work via **proxies** — the bean you get is a wrapper.

---

## 8. Spring Boot — Auto-configuration Example

```java
// Just having these on the classpath auto-configures:
// spring-boot-starter-data-jpa → DataSource, EntityManager, repositories
// spring-boot-starter-security  → default login, filter chain
// spring-boot-starter-web       → embedded Tomcat, Jackson
@SpringBootApplication
public class App { public static void main(String[] args) {
    SpringApplication.run(App.class, args);
} }
```

---

## 9. JPA — What to Say

- `@Entity` maps a class to a table; `@Id` the key.
- `JpaRepository<Book, Long>` gives CRUD for free.
- Relationships: `@OneToMany`, `@ManyToOne`, `@ManyToMany`.
- N+1 problem: lazy loading fetches one query per row → use `join fetch` or DTO projections.
- Use `@Transactional` for multi-statement operations.

---

## 10. Security — JWT Flow One-Minute Answer

```text
1. POST /api/auth/login  → verify BCrypt → issue JWT
2. Client sends "Authorization: Bearer <token>" on each request
3. OncePerRequestFilter validates signature + expiry
4. Sets Authentication in SecurityContext
5. authorizeHttpRequests checks roles
6. Stateless: no session, token holds the identity
```

---

## 11. Microservices — The Keywords

```text
Service discovery  → Eureka / Consul (register + look up by name)
API gateway        → routes, auth, rate limiting at one entry
Circuit breaker    → Resilience4j: fail fast + fallback
Config server      → centralize application.yml
Distributed tracing→ Micrometer + Zipkin (correlation ids)
Database per service
```

---

## 12. Common Mistakes to Avoid Saying

| Wrong | Right |
|-------|-------|
| "Field injection is fine" | Prefer constructor injection |
| "@RestController and @Controller are the same" | @RestController = @Controller + @ResponseBody |
| "Transaction ends at method return" | Commit happens after the proxy method completes |
| "JPA fetches everything eagerly" | Default lazy for collections |
| "One DB for all services" | Database per service in microservices |

---

## 13. Best Practices to Mention

- Constructor injection, `final` fields.
- `@Transactional` on service methods, not controllers.
- DTOs over entities on the API boundary.
- `@CacheEvict` when data changes.
- Tests: `@WebMvcTest`, `@DataJpaTest`, `@SpringBootTest`.

---

## 14. Interview Questions

**Q1. What is dependency injection?**
*Spring provides dependencies instead of the class creating them → testable, decoupled.*

**Q2. `@Component`, `@Service`, `@Repository`?**
*All beans; stereotypes for clarity. @Repository adds exception translation.*

**Q3. How do transactions work in Spring?**
*Proxy + @Transactional; commits after success, rolls back on RuntimeException.*

**Q4. What's the N+1 problem?**
*One query for the parent, N for children; fix with fetch joins or projections.*

**Q5. How do you secure a REST API?**
*JWT: stateless filter chain, BCrypt, role-based authorization.*

---

## 15. Practice Exercises

1. Draw the request flow from memory.
2. Explain `@Transactional` proxy behavior aloud.
3. List 5 Spring Boot annotations and their jobs.

---

## 16. Coding Challenges

1. **Easy:** `@RestController` with `@GetMapping` — explain lifecycle.
2. **Medium:** Design a `@Cacheable` invalidation strategy.
3. **Hard:** Walk through securing a multi-tenant API with JWT roles.

---

## 17. Summary

- IoC, DI, Boot, JPA, Security, Caching, Microservices in one sheet.
- The one-minute JWT answer is essential.
- Practice the flow diagrams aloud.

➡️ Continue to: [DSA Interview Questions](DSA-Interview-Questions.md)