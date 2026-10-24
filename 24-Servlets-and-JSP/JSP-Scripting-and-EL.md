# JSP Scripting and EL

## 1. Introduction

Raw scriptlets make JSP messy. Modern JSP uses the **Expression Language (EL)** — `${...}` — to read data without Java code, and **JSTL** tags to loop and branch declaratively. This produces clean, readable view templates and completes the servlet MVC picture.

## 2. Why This Topic Is Important

- EL and JSTL replace almost all scriptlets — this is how production JSP is written.
- Request attributes set by servlets are consumed with EL.
- It mirrors the model–view separation used by every template engine.

## 3. Real-World Analogy

Scriptlets are like **painting a wall with a toothbrush**. EL and JSTL are the **paint roller**: less effort, cleaner result, easier for anyone (including non-Java folks) to read.

## 4. Theory

### Expression Language

- `${expr}` reads values from page/request/session/application scope.
- `${user.name}` navigates bean properties (calls `getName()`).
- `${cart.total > 100 ? "Free" : "Paid"}` supports operators.
- `param.name` reads request parameters: `${param.name}`.

### Scope lookup order

Page → Request → Session → Application.

### JSTL core tags

| Tag | Purpose |
|-----|---------|
| `<c:if test="${...}">` | Conditional |
| `<c:forEach items="${list}" var="item">` | Loop |
| `<c:out value="${user.name}"/>` | Escaped output |
| `<c:set var="x" value="..."/>` | Set variable |

Add the dependency: `jakarta.servlet.jsp.jstl` (and the taglib directive).

## 5. Syntax

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<c:if test="${not empty user}">
  <p>Welcome, <c:out value="${user.name}"/></p>
</c:if>

<ul>
  <c:forEach items="${items}" var="item">
    <li><c:out value="${item}"/></li>
  </c:forEach>
</ul>
```

## 6. Flow Explanation

1. A servlet (controller) processes the request and sets attributes: `req.setAttribute("items", list)`.
2. It forwards to the JSP: `req.getRequestDispatcher("/view.jsp").forward(req, resp)`.
3. The JSP uses EL `${items}` to read the attribute.
4. JSTL tags render loops and conditionals.
5. `c:out` escapes values, preventing XSS.

## 7. Memory Representation

```
Servlet (controller)            JSP (view)
  req.setAttribute("items")  →  ${items}
  req.setAttribute("user")   →  ${user.name}
        └── forward ──────────────┘
```

## 8. Simple Example

```jsp
<p>Hello, ${param.name != null ? param.name : 'Guest'}!</p>
```

## 9. Intermediate Example

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<ul>
  <c:forEach items="${products}" var="p">
    <li>
      <c:out value="${p.name}"/> — $<c:out value="${p.price}"/>
      <c:if test="${p.stock == 0}">(out of stock)</c:if>
    </li>
  </c:forEach>
</ul>
```

## 10. Advanced Example

```java
// Controller servlet
@WebServlet("/orders")
public class OrderListServlet extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        req.setAttribute("orders", orderService.findAll());
        req.getRequestDispatcher("/WEB-INF/views/orders.jsp").forward(req, resp);
    }
}
```

```jsp
<%-- orders.jsp --%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<table border="1">
  <tr><th>Order</th><th>Total</th><th>Status</th></tr>
  <c:forEach items="${orders}" var="o">
    <tr>
      <td><c:out value="${o.id}"/></td>
      <td>$<c:out value="${o.total}"/></td>
      <td><c:out value="${o.status}"/></td>
    </tr>
  </c:forEach>
</table>
```

## 11. Output

```
Hello, Alice!

Products:
  Keyboard — $49.99
  Mouse — $19.99 (out of stock)
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Forgetting the taglib directive | JSTL tags unparsed | Add `<%@ taglib ... %>` |
| Using `<%= %>` when `${}` suffices | Scriptlet clutter | Prefer EL |
| Not escaping user data | XSS | Use `<c:out>` |
| Referencing a null bean | "Property not found" | Check with `empty` |

## 13. Best Practices

- Use EL + JSTL instead of scriptlets.
- Always escape dynamic output with `<c:out>`.
- Keep data access out of views — set attributes in controllers.
- Store JSPs under `/WEB-INF` to prevent direct access.

## 14. Interview Questions

**Q: What is EL and how does `${user.name}` work?**
The Expression Language reads bean properties from page/request/session/application scopes — `${user.name}` calls `getName()`.

**Q: Why prefer EL/JSTL over scriptlets?**
Cleaner views, no Java in templates, automatic escaping, and a clear MVC separation.

**Q: How does `c:out` differ from `<%= %>`?**
`c:out` HTML-escapes the value, preventing XSS.

## 15. Practice Exercises

1. Use `${param.name}` to greet a user.
2. Render a list of objects with `<c:forEach>` and `<c:out>`.
3. Add a conditional with `<c:if>` on stock level.

## 16. Coding Challenges

1. Build a controller servlet that forwards a list of orders to a JSP rendering a table with EL + JSTL.
2. Escape a malicious value (`<script>alert(1)</script>`) with `c:out` and verify it renders as text.

## 17. Summary

- EL `${...}` reads data from scopes without Java.
- JSTL provides `c:forEach`, `c:if`, `c:out`, `c:set`.
- Controllers set attributes and forward to views — classic MVC.
- Escape dynamic output to prevent XSS.
- Servlet + JSP is the foundation on which **Module 25 — Spring Core** and MVC frameworks are built.