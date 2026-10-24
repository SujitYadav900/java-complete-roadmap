# JWT (JSON Web Tokens)

## 1. Introduction

**JWT** is a stateless, signed token used for API authentication. The client keeps the token; the server verifies its signature — no server-side sessions needed.

---

## 2. Why This Topic Is Important

- Stateless auth scales across services (microservices).
- JWT is the default for modern APIs.
- Interviewers test the JWT flow and claims.
- Understanding it demystifies Authorization headers.

---

## 3. Real-World Analogy

A JWT is a **stamped concert wristband**:
- The venue (server) issues it when you show your ticket (login).
- The band has a watermark (signature) so anyone can verify it's genuine.
- You show the band at every entrance (each API call) — no need to check your name again (no session). The band expires at midnight (expiry).

---

## 4. Theory

### JWT Structure
```
header.payload.signature
```

| Part | Content |
|------|---------|
| **Header** | Algorithm, type (`{"alg":"HS256","typ":"JWT"}`) |
| **Payload** | Claims: sub, roles, exp, iat |
| **Signature** | `HMAC(header.payload, secret)` |

### Common Claims
| Claim | Meaning |
|-------|---------|
| `sub` | Subject (user id) |
| `roles` / `authorities` | Permissions |
| `exp` | Expiration |
| `iat` | Issued at |
| `iss` | Issuer |

### Auth Flow
```mermaid
flowchart LR
    A[Login] --> B[Verify credentials]
    B --> C[Issue JWT]
    C --> D[Client stores token]
    D --> E[Request + Authorization: Bearer <token>]
    E --> F[Verify signature]
    F -->|ok| G[Authorize + serve]
```

### Libraries
- `jjwt` (io.jsonwebtoken) — most common
- `nimbus-jose-jwt` — alternative

---

## 5. Syntax

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiJ9...
```

```java
// Generate
String token = Jwts.builder()
        .subject("ajay")
        .claim("roles", "ADMIN")
        .expiration(new Date(System.currentTimeMillis() + 3600_000))
        .signWith(secretKey)
        .compact();
```

---

## 6. Flow Explanation

```mermaid
flowchart TD
    A[Login endpoint] --> B[Issue JWT]
    B --> C[Client stores it]
    C --> D[Every request sends Bearer]
    D --> E[JwtFilter validates]
    E --> F[Set SecurityContext]
    F --> G[Authorize routes]
```

---

## 7. Memory Representation

The token is a stateless string; the server parses and verifies it per request.

---

## 8. Simple Example — Generate a Token (jjwt)

```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.12.6</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.12.6</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId>
    <version>0.12.6</version>
    <scope>runtime</scope>
</dependency>
```

```java
import io.jsonwebtoken.Jwts;
import javax.crypto.SecretKey;
import javax.crypto.spec.SecretKeySpec;
import java.util.Date;

public class JwtUtil {

    private static final String SECRET = "a-very-long-secret-key-for-hs256-signing-must-be-32-bytes";
    private static final SecretKey KEY =
            new SecretKeySpec(SECRET.getBytes(), "HmacSHA256");

    public static String generate(String username, String role) {
        return Jwts.builder()
                .subject(username)
                .claim("roles", role)
                .issuedAt(new Date())
                .expiration(new Date(System.currentTimeMillis() + 3600_000))
                .signWith(KEY)
                .compact();
    }

    public static String parseSubject(String token) {
        return Jwts.parser()
                .verifyWith(KEY)
                .build()
                .parseSignedClaims(token)
                .getPayload()
                .getSubject();
    }
}
```

---

## 9. Intermediate Example — Login + Protect

```java
import org.springframework.web.bind.annotation.*;

record LoginRequest(String username, String password) { }
record LoginResponse(String token) { }

@RestController
public class AuthController {

    @PostMapping("/api/login")
    public LoginResponse login(@RequestBody LoginRequest request) {
        // In real apps: authenticate via AuthenticationManager.
        if ("ajay".equals(request.username()) && "pass123".equals(request.password())) {
            return new LoginResponse(JwtUtil.generate(request.username(), "USER"));
        }
        throw new org.springframework.security.authentication.BadCredentialsException("Invalid credentials");
    }

    @GetMapping("/api/secure")
    public String secure() {
        return "This endpoint requires a valid JWT";
    }
}
```

---

## 10. Advanced Example — JWT Filter

```java
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.web.filter.OncePerRequestFilter;
import jakarta.servlet.FilterChain;
import jakarta.servlet.http.*;
import java.util.List;

public class JwtAuthFilter extends OncePerRequestFilter {

    @Override
    protected void doFilterInternal(
            HttpServletRequest request,
            HttpServletResponse response,
            FilterChain chain) throws java.io.IOException, jakarta.servlet.ServletException {

        String header = request.getHeader("Authorization");

        if (header != null && header.startsWith("Bearer ")) {
            String token = header.substring(7);
            try {
                String username = JwtUtil.parseSubject(token);
                var auth = new UsernamePasswordAuthenticationToken(
                        username, null, List.of(new SimpleGrantedAuthority("ROLE_USER")));
                SecurityContextHolder.getContext().setAuthentication(auth);
            } catch (Exception e) {
                SecurityContextHolder.clearContext();
            }
        }
        chain.doFilter(request, response);
    }
}
```

---

## 11. Output

For the **Intermediate Example**:

```
POST /api/login {"username":"ajay","password":"pass123"}
→ {"token":"eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhamF5IiwiZXhwIjoxNz... "}
```

```
GET /api/secure  (with Bearer token) → "This endpoint requires a valid JWT"
GET /api/secure  (no token)          → 401 / 403
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| Storing secrets in code | Use env vars |
| Weak secret keys | ≥32 random bytes |
| No expiry | Always set exp |
| JWT in URLs | Send in Authorization header |
| Not validating signature | parser.verifyWith(KEY) |

---

## 13. Best Practices

- Short expiry (minutes/hours).
- Store secret in environment.
- Always verify the signature.
- Use claims for roles, not user data.
- Add the JWT filter to the security chain.

---

## 14. Interview Questions

**Q1. What is a JWT?**
*A signed, stateless token with header, payload, signature.*

**Q2. Why stateless?**
*No server sessions — scales across services.*

**Q3. What's in the payload?**
*Claims: sub, roles, exp, iat.*

**Q4. How is it verified?**
*Recompute the HMAC signature with the shared secret.*

**Q5. JWT vs session?**
*JWT is stateless and portable; sessions are server-side and revocable.*

---

## 15. Practice Exercises

1. Generate and parse a JWT with jjwt.
2. Build a login endpoint issuing tokens.
3. Validate the Bearer header in a filter.
4. Add roles to claims and enforce them.

---

## 16. Coding Challenges

1. **Easy:** Generate/parse a token.
2. **Medium:** Login endpoint + protected route.
3. **Hard:** Full JWT filter chain + role enforcement.

---

## 17. Summary

- JWT = stateless, signed auth tokens.
- header.payload.signature with exp claims.
- Login issues; filter validates per request.
- Strong secret + short expiry.
- Bearer header everywhere; no sessions.

➡️ Continue to: [CSRF & CORS](CSRF-and-CORS.md)