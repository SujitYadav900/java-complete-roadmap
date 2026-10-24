# What are Servlets?

## 1. Introduction

A **servlet** is a Java class that runs on a web server (like Tomcat) and handles **HTTP requests**. The browser sends a request; the servlet processes it and writes an HTTP response. Servlets are the foundation of Java web technology.

## 2. Why This Topic Is Important

- Every Java web framework (including Spring Boot) sits on the servlet model.
- Understanding servlets explains how requests map to Java methods and how the container manages your code.
- Interviewers use servlet questions to probe your understanding of web fundamentals.

## 3. Real-World Analogy

A servlet is like a **restaurant kitchen order ticket window**. The waiter (browser) slides an order (HTTP request) through the window; the chef (servlet) cooks the dish (business logic) and slides the result (HTTP response) back. Many orders arrive at once, so the kitchen runs many chefs in parallel.

## 4. Theory

### The request/response model

- **HTTP request** — method (GET/POST), URL, headers, body (form data/JSON).
- **HTTP response** — status code (200/404/500), headers, body (HTML/JSON).
- The **servlet container** (Tomcat, Jetty) accepts TCP connections, parses HTTP, and calls your servlet.

### Servlet APIs

- `javax.servlet.http.HttpServlet` — base class to extend.
- `HttpServletRequest` — read method, headers, parameters, session.
- `HttpServletResponse` — set status, headers, and write the body.

### A servlet maps to a URL

```java
@WebServlet("/hello")   // or web.xml <servlet-mapping>
```

## 5. Syntax

```java
@WebServlet("/hello")
public class HelloServlet extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws IOException {
        resp.setContentType("text/html");
        resp.getWriter().write("<h1>Hello World</h1>");
    }
}
```

## 6. Flow Explanation

1. Browser requests `http://localhost:8080/hello`.
2. Tomcat matches the URL to the servlet mapping.
3. Tomcat creates/borrows a `HttpServletRequest` and `HttpServletResponse`.
4. Tomcat calls `servlet.doGet(req, resp)`.
5. The servlet writes HTML to the response.
6. Tomcat sends the response back to the browser.

## 7. Memory Representation

```
Browser ──GET /hello──▶ Tomcat ──▶ HelloServlet.doGet()
   ▲                              │ writes response body
   └────────── 200 OK + HTML ─────┘
```

The container manages servlet instances in a pool; each request runs in its own thread.

## 8. Simple Example

```java
@WebServlet("/time")
public class TimeServlet extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws IOException {
        resp.setContentType("text/plain");
        resp.getWriter().write("Server time: " + LocalDateTime.now());
    }
}
```

## 9. Intermediate Example

```java
@WebServlet("/greet")
public class GreetServlet extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws IOException {
        String name = req.getParameter("name");
        name = (name == null) ? "World" : name;
        resp.setContentType("text/html");
        resp.getWriter().write("<h1>Hello, " + name + "!</h1>");
    }
}
```

## 10. Advanced Example

```java
@WebServlet("/api/status")
public class StatusServlet extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws IOException {
        resp.setContentType("application/json");
        resp.setStatus(HttpServletResponse.SC_OK);
        resp.getWriter().write("{\"status\":\"UP\"}");
    }
}
```

## 11. Output

Request: `GET /greet?name=Alice`

Response:
```
HTTP/1.1 200 OK
Content-Type: text/html
<h1>Hello, Alice!</h1>
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Forgetting `setContentType` | Plain text instead of HTML/JSON | Set it before writing |
| Writing HTML in servlets | Hard-to-maintain pages | Use JSP for view |
| Not encoding user input | XSS vulnerabilities | Escape output |
| Overriding wrong method | 405 Method Not Allowed | Override `doGet`/`doPost` as needed |

## 13. Best Practices

- One servlet per responsibility (login, register, order).
- Keep business logic in separate service classes, not the servlet.
- Validate and sanitize all request parameters.
- Use request forwarding (not redirects) for internal page flow.

## 14. Interview Questions

**Q: What is a servlet?**
A Java class running on a servlet container that processes HTTP requests and produces responses.

**Q: How are servlets different from plain Java classes?**
They live in a container that manages their lifecycle (init → service → destroy), concurrency, and HTTP parsing.

**Q: Is a servlet thread-safe?**
Not by default — a single servlet instance is shared across request threads, so instance fields must be handled with care (prefer method-local state).

## 15. Practice Exercises

1. Create a servlet that returns the current time as text.
2. Read a `name` parameter and greet the user.
3. Return JSON from a servlet with the correct content type.

## 16. Coding Challenges

1. Build a small calculator servlet that takes `a`, `b`, and `op` parameters and returns the result.
2. Escape user input before echoing it back, and verify a `<script>` tag is neutralized.

## 17. Summary

- Servlets handle HTTP request/response in Java.
- Extend `HttpServlet`, override `doGet`/`doPost`, map with `@WebServlet`.
- The container manages lifecycle and threading.
- Keep logic out of the view; use services and JSP.
- Next, understand the servlet's life in **Servlet-Lifecycle.md**.