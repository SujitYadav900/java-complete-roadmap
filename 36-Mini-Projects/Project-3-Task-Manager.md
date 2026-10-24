# Project 3 — Task Manager (Secure Full-Stack API)

## 1. Introduction

Extend Project 2 into a **secure, full-featured task manager**: user authentication with JWT, per-user tasks, roles, and comprehensive tests. This is the closest thing to a real production backend you will build.

---

## 2. Why This Project Is Important

- Adds **Spring Security + JWT** (module 29) to a real API.
- Introduces **ownership** — users only see their own tasks.
- Forces you to think about security, validation, and testing together.

---

## 3. Real-World Analogy

A **shared project board** (like Jira/Trello): every person logs in, and each sees only their own tasks. Your API must know *who* is asking and only return that person's data.

---

## 4. Theory — Design

### Features
| Feature | Method | URL |
|---------|--------|-----|
| Register | `POST` | `/api/auth/register` |
| Login | `POST` | `/api/auth/login` |
| List my tasks | `GET` | `/api/tasks` |
| Create task | `POST` | `/api/tasks` |
| Update task | `PUT` | `/api/tasks/{id}` |
| Delete task | `DELETE` | `/api/tasks/{id}` |
| Admin: list users | `GET` | `/api/admin/users` |

### Security Model
- Passwords hashed with **BCrypt**.
- JWT issued on login, sent as `Authorization: Bearer <token>`.
- Tasks are filtered by the authenticated user's id.

---

## 5. Syntax (Core Pattern)

```java
// A secured endpoint — the current user is injected
@GetMapping
public List<Task> myTasks(@AuthenticationPrincipal User user) {
    return service.findByOwner(user.getId());
}
```

---

## 6. Flow Explanation

```mermaid
flowchart TD
    A[Login] --> B[Verify password]
    B --> C[Issue JWT]
    C --> D[Client stores token]
    D --> E[Request with Bearer token]
    E --> F[JWT filter validates]
    F --> G[SecurityContext has user]
    G --> H[Task query scoped to user]
```

---

## 7. Memory Representation

Each `Task` has an `owner` (User) reference. Queries use that to scope results — a foreign-key relationship in the database and an object reference in memory.

---

## 8. Simple Example — User Entity + Security Config

```java
@Entity
public class User {
    @Id @GeneratedValue
    private Long id;
    @Column(unique = true)
    private String username;
    private String password;
    private String role = "ROLE_USER";
    // getters & setters
}
```

```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    return http
        .csrf(csrf -> csrf.disable())
        .authorizeHttpRequests(auth -> auth
            .requestMatchers("/api/auth/**").permitAll()
            .requestMatchers("/api/admin/**").hasRole("ADMIN")
            .anyRequest().authenticated())
        .sessionManagement(sm -> sm.sessionCreationPolicy(STATELESS))
        .addFilterBefore(jwtFilter, UsernamePasswordAuthenticationFilter.class)
        .build();
}
```

---

## 9. Intermediate Example — JWT Utility

```java
@Component
public class JwtUtil {
    private final SecretKey key = Keys.hmacShaKeyFor(secret.getBytes());
    private final long expiration = 3600_000; // 1 hour

    public String generate(String username) {
        return Jwts.builder()
            .subject(username)
            .issuedAt(new Date())
            .expiration(new Date(System.currentTimeMillis() + expiration))
            .signWith(key)
            .compact();
    }

    public String extractUsername(String token) {
        return Jwts.parser().verifyWith(key).build()
            .parseSignedClaims(token).getPayload().getSubject();
    }
}
```

---

## 10. Advanced Example — JWT Filter + Owner-Scoped Service

```java
@Component
public class JwtAuthFilter extends OncePerRequestFilter {
    private final JwtUtil jwtUtil;
    private final UserRepository users;

    public JwtAuthFilter(JwtUtil jwtUtil, UserRepository users) {
        this.jwtUtil = jwtUtil;
        this.users = users;
    }

    @Override
    protected void doFilterInternal(HttpServletRequest req,
            HttpServletResponse res, FilterChain chain) throws ServletException, IOException {
        String header = req.getHeader("Authorization");
        if (header != null && header.startsWith("Bearer ")) {
            String token = header.substring(7);
            String username = jwtUtil.extractUsername(token);
            users.findByUsername(username).ifPresent(u -> {
                var auth = new UsernamePasswordAuthenticationToken(
                    u, null, List.of(new SimpleGrantedAuthority(u.getRole())));
                SecurityContextHolder.getContext().setAuthentication(auth);
            });
        }
        chain.doFilter(req, res);
    }
}
```

```java
@Service
public class TaskService {
    // ...
    public List<Task> findForUser(Long ownerId) {
        return repo.findByOwnerId(ownerId);  // scoped query
    }
}
```

---

## 11. Output

```
POST /api/auth/register
{"username":"alice","password":"secret"}
→ 201 Created

POST /api/auth/login {"username":"alice","password":"secret"}
→ 200 OK {"token":"eyJhbGciOiJIUzI1NiJ9..."}

GET /api/tasks
Authorization: Bearer eyJhbGciOiJIUzI1NiJ9...
→ 200 OK [{"id":1,"title":"Write README","done":false,"ownerId":1}]
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| Storing plain passwords | Always BCrypt |
| Not scoping queries | Add `ownerId` to every query |
| Forgetting the filter in the chain | `.addFilterBefore(...)` |
| JWT too long-lived | Short expiry + refresh token |
| Missing `STATELESS` sessions | Set session policy |

---

## 13. Best Practices

- BCrypt for passwords; never log them.
- JWT expiry + refresh flow.
- Server-side scoping: never trust client-sent ids.
- Role checks for admin endpoints.
- Test the security chain (401 without token, 403 wrong role).

---

## 14. Interview Questions

**Q1. How does JWT authentication work end to end?**
*Login → token → filter validates → SecurityContext → authorization.*

**Q2. Why BCrypt over plain or MD5?**
*Adaptive, salted hashing; slow by design.*

**Q3. What is `OncePerRequestFilter`?**
*Ensures the filter runs once per request dispatch.*

**Q4. How do you prevent a user reading another user's tasks?**
*Never trust input; scope queries by the authenticated user.*

**Q5. Stateless vs stateful sessions?**
*JWT = stateless (no server session); easier to scale.*

---

## 15. Practice Exercises

1. Add a refresh token endpoint.
2. Add `GET /api/tasks?done=false` filtering.
3. Add rate limiting on the login endpoint.
4. Add a password-change endpoint.

---

## 16. Coding Challenges

1. **Easy:** Add task deadlines and overdue flags.
2. **Medium:** Allow sharing tasks with another user (many-to-many).
3. **Hard:** Add refresh-token rotation stored in the database.

---

## 17. Summary

- JWT authentication with Spring Security.
- Owner-scoped data access.
- Stateless, testable, production-style backend.
- Blueprint for real enterprise projects.

➡️ Continue to: [Project 4 — Bank Console App](Project-4-Bank-Console-App.md)