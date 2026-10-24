# Sessions and Cookies

## 1. Introduction

HTTP is **stateless** — each request is independent. **Sessions** and **cookies** add state so the server remembers who you are between requests: your login, your shopping cart, your preferences.

## 2. Why This Topic Is Important

- Login, carts, and personalization all depend on session state.
- Interviewers love session/cookie questions — security pitfalls included.
- Spring Security, Spring MVC, and REST JWT flows all build on these concepts.

## 3. Real-World Analogy

A session is like a **restaurant table card**. When you sit down, the waiter writes a table number on a card they also hand you. On every request, you show the card, and the waiter knows your orders so far without you re-explaining. The card is the cookie; the kitchen's records are the session.

## 4. Theory

### Cookies

- Small key–value pairs the **server sends and the browser stores**.
- Sent back automatically with every request to the same domain.
- `JSESSIONID` is the session-tracking cookie used by servlet containers.

### HttpSession

- Server-side storage keyed by a session ID.
- `req.getSession()` creates/returns the session.
- `req.getSession(false)` returns null if none exists.
- Store objects with `setAttribute` / `getAttribute` / `removeAttribute`.
- Session expires after `maxInactiveInterval` (default 30 min) or on logout.

### The flow

1. First request → server creates a session and sends `Set-Cookie: JSESSIONID=...`.
2. Browser stores it and sends it back on each request.
3. Server looks up the session by ID and restores state.

## 5. Syntax

```java
HttpSession session = req.getSession();          // create or get
session.setAttribute("user", username);           // store
String user = (String) session.getAttribute("user"); // read
session.removeAttribute("user");                  // remove
session.invalidate();                             // destroy (logout)

// Read a cookie
Cookie[] cookies = req.getCookies();
// Send a cookie
resp.addCookie(new Cookie("theme", "dark"));
```

## 6. Flow Explanation

1. Browser hits `/login` with credentials.
2. On success, servlet calls `req.getSession()` and stores the user.
3. Server responds with `Set-Cookie: JSESSIONID=...`.
4. Browser echoes the cookie on subsequent requests.
5. Each protected servlet checks `session.getAttribute("user")`.
6. On logout, `session.invalidate()` clears server state.

## 7. Memory Representation

```
Browser                     Server
┌──────────┐  JSESSIONID    ┌──────────────┐
│ cookies  │ ─────────────▶ │ HttpSession  │
│ JSESSIONID=abc            │  user: admin │
└──────────┘                └──────────────┘
```

## 8. Simple Example

```java
@WebServlet("/visit")
public class VisitServlet extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws IOException {
        HttpSession session = req.getSession();
        Integer visits = (Integer) session.getAttribute("visits");
        visits = (visits == null ? 0 : visits) + 1;
        session.setAttribute("visits", visits);
        resp.setContentType("text/html");
        resp.getWriter().write("<h1>Visit #" + visits + "</h1>");
    }
}
```

## 9. Intermediate Example

```java
@WebServlet("/cart")
public class CartServlet extends HttpServlet {
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
            throws IOException {
        HttpSession session = req.getSession();
        List<String> cart = (List<String>) session.getAttribute("cart");
        if (cart == null) {
            cart = new ArrayList<>();
            session.setAttribute("cart", cart);
        }
        cart.add(req.getParameter("item"));
        resp.getWriter().write("Cart: " + cart);
    }
}
```

## 10. Advanced Example

```java
@WebServlet("/logout")
public class LogoutServlet extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws IOException {
        HttpSession session = req.getSession(false);
        if (session != null) {
            session.invalidate();
        }
        resp.sendRedirect("/login");
    }
}
```

## 11. Output

First visit:

```
HTTP/1.1 200 OK
Set-Cookie: JSESSIONID=ABC123; Path=/; HttpOnly

Visit #1
```

Second visit (browser sends the cookie):

```
Visit #2
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Storing sensitive data in the session | Memory bloat / leaks | Store user ID, not objects |
| Ignoring session expiry | Users "logged out" unexpectedly | Configure `maxInactiveInterval` |
| No `HttpOnly` on cookies | XSS can steal session | Set `HttpOnly`, `Secure` |
| Forgetting `invalidate()` on logout | Stolen sessions persist | Invalidate explicitly |
| Session fixation | Attacker fixes your session ID | Regenerate ID after login |

## 13. Best Practices

- Store minimal data in the session (e.g., user ID).
- Mark session cookies `HttpOnly` and `Secure`.
- Set a sane session timeout.
- Regenerate the session ID after successful login.
- Use `getSession(false)` when you only want to check for an existing session.

## 14. Interview Questions

**Q: What is the difference between a session and a cookie?**
A session is server-side state keyed by an ID; a cookie is client-side storage the browser sends back with each request. Sessions usually use cookies to carry the ID.

**Q: Why is HTTP called stateless?**
Each request is independent; the server keeps no memory between requests unless it explicitly stores state (session/cookie).

**Q: What is session fixation?**
An attack where an attacker gives the victim a known session ID; after the victim logs in, the attacker reuses it. Fixed by regenerating the ID after login.

## 15. Practice Exercises

1. Build a visit counter using `HttpSession`.
2. Add items to a session-backed cart.
3. Implement login/logout with session `invalidate()`.

## 16. Coding Challenges

1. Protect a servlet so it redirects to `/login` when `session.getAttribute("user")` is null.
2. Read a `theme` cookie and switch the page background; store it with `HttpOnly`.

## 17. Summary

- HTTP is stateless; sessions and cookies add state.
- `HttpSession` stores server-side data keyed by `JSESSIONID`.
- Cookies travel with every request; mark them `HttpOnly`/`Secure`.
- Invalidate sessions on logout and regenerate after login.
- This model reappears in Spring Security's filters — next, **JSP-Basics.md**.