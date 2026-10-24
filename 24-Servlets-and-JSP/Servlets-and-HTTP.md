# Servlets and HTTP

## 1. Introduction

Servlets exist to speak **HTTP**. This topic maps HTTP methods, request data, and response details onto the `HttpServletRequest` / `HttpServletResponse` APIs, and shows how to forward requests between servlets.

## 2. Why This Topic Is Important

- Everything you read a request or write a response, you use these APIs.
- REST, forms, redirects, and error handling all build on them.
- Interviews probe `doGet` vs `doPost`, forwarding, and status codes.

## 3. Real-World Analogy

HTTP is the **conversation protocol** between browser and server: the browser asks a question (request), the server answers (response). Each answer has a "mood" — status code — that says success (200), not found (404), moved (302), or error (500).

## 4. Theory

### HTTP methods

| Method | Use | Data location |
|--------|-----|---------------|
| `GET` | Read/query | URL query string |
| `POST` | Create/submit | Request body |
| `PUT` | Replace | Body |
| `DELETE` | Remove | Path/params |

### Request API

- `req.getMethod()` — GET/POST/...
- `req.getParameter(name)` — form/query parameter.
- `req.getParameterMap()` — all parameters.
- `req.getHeader(name)` — headers.
- `req.getRequestURI()` — path.

### Response API

- `resp.setStatus(404)` — status code.
- `resp.setHeader(name, value)` — headers.
- `resp.setContentType("text/html")` — content type.
- `resp.getWriter()` / `resp.getOutputStream()` — body.

### Forward vs redirect

- **Forward** (server-side): `req.getRequestDispatcher("/page").forward(req, resp)` — same request, URL unchanged.
- **Redirect** (client-side): `resp.sendRedirect("/login")` — 302, new request.

## 5. Syntax

```java
@WebServlet("/form")
public class FormServlet extends HttpServlet {
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
            throws IOException {
        String name = req.getParameter("name");
        resp.setContentType("text/plain");
        resp.getWriter().write("Hello, " + name);
    }
}
```

## 6. Flow Explanation

1. Browser submits a form with POST to `/form`.
2. Container routes it to `FormServlet.doPost`.
3. `req.getParameter("name")` reads the submitted value.
4. The servlet writes the response body.
5. The container sends status, headers, and body to the browser.

## 7. Memory Representation

```
POST /form  HTTP/1.1
Host: localhost:8080
Content-Type: application/x-www-form-urlencoded

name=Alice        ← body parsed into getParameter("name")

HTTP/1.1 200 OK
Content-Type: text/plain

Hello, Alice
```

## 8. Simple Example

```java
protected void doGet(HttpServletRequest req, HttpServletResponse resp)
        throws IOException {
    resp.setStatus(HttpServletResponse.SC_OK);
    resp.setContentType("text/html");
    resp.getWriter().write("<html><body>GET works</body></html>");
}
```

## 9. Intermediate Example

```java
@WebServlet("/search")
public class SearchServlet extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws IOException {
        String q = req.getParameter("q");
        if (q == null || q.isBlank()) {
            resp.sendRedirect("/search?q=default");
            return;
        }
        resp.setContentType("text/plain");
        resp.getWriter().write("Searching for: " + q);
    }
}
```

## 10. Advanced Example

```java
@WebServlet("/login")
public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
            throws IOException {
        String user = req.getParameter("username");
        String pass = req.getParameter("password");

        if ("admin".equals(user) && "secret".equals(pass)) {
            HttpSession session = req.getSession();
            session.setAttribute("user", user);
            resp.sendRedirect("/welcome");
        } else {
            resp.setStatus(401);
            resp.getWriter().write("Invalid credentials");
        }
    }
}
```

## 11. Output

Form submitted with `username=admin&password=secret` → `302` redirect to `/welcome`.
Bad credentials → `401` with "Invalid credentials".

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Calling `getWriter` then `getOutputStream` | IllegalStateException | Choose one |
| Writing after redirect | Redirect ignored | Write, then stop; or return after `sendRedirect` |
| Trusting parameters blindly | Injection/XSS | Validate and escape |
| Setting content type after writing | Headers already sent | Set headers before writing body |

## 13. Best Practices

- Use POST for state changes, GET for reads.
- Redirect after a successful POST (Post/Redirect/Get) to avoid resubmission.
- Validate every parameter before use.
- Set status/content type before writing the body.
- Centralize error handling with an error page.

## 14. Interview Questions

**Q: Difference between forward and redirect?**
Forward is server-side, same request, URL unchanged; redirect is a client-side 302 causing a new request with a new URL.

**Q: How do you read form data?**
`req.getParameter("name")` reads the body (POST) or query string (GET).

**Q: Why redirect after a POST?**
To prevent double-submission when the user refreshes.

## 15. Practice Exercises

1. Create a servlet that echoes the request method and URI.
2. Handle both GET and POST with different responses.
3. Redirect a missing parameter to a default URL and observe the 302.

## 16. Coding Challenges

1. Build a servlet that forwards a request to another servlet and verify the URL does not change.
2. Return JSON with status 404 for an unknown resource and 200 for a known one.

## 17. Summary

- GET/POST/PUT/DELETE map to doGet/doPost/etc.
- Request object reads method, params, headers; response object sets status, headers, body.
- Forward = server-side same request; redirect = new client request.
- Validate input and set headers before writing.
- Next, manage state with **Sessions-and-Cookies.md**.