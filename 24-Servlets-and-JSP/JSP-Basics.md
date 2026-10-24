# JSP Basics

## 1. Introduction

**JSP** (JavaServer Pages) lets you write **HTML with embedded Java** to build dynamic web pages. JSP files are compiled into servlets by the container, giving you the view layer for servlet-based applications.

## 2. Why This Topic Is Important

- JSP is how servlet apps historically rendered views.
- It introduces the **MVC split**: servlets = controller, JSP = view.
- Understanding JSP makes the template engines that followed (Thymeleaf, FreeMarker) easier to grasp.

## 3. Real-World Analogy

JSP is like a **mail-merge document**. The letter (HTML) has blank slots; the Java bits fill them with data each time the page is requested. The same template produces personalized pages for every user.

## 4. Theory

### JSP elements

| Element | Syntax | Purpose |
|---------|--------|---------|
| Directive | `<%@ page ... %>` | Page settings (imports, session) |
| Scriptlet | `<% ... %>` | Java code block |
| Expression | `<%= expr %>` | Print a value |
| Declaration | `<%! ... %>` | Declare methods/fields |
| Comment | `<%-- --%>` | Server-side comment |

### How JSP works

1. The `.jsp` file is compiled into a servlet class.
2. HTML becomes `out.write(...)` calls.
3. Expressions and scriptlets become Java in `_jspService()`.
4. The compiled servlet renders the page per request.

## 5. Syntax

```jsp
<%@ page import="java.time.LocalDateTime" %>
<html>
<body>
  <h1>Hello</h1>
  <p>Server time: <%= LocalDateTime.now() %></p>
</body>
</html>
```

## 6. Flow Explanation

1. Browser requests `/welcome.jsp`.
2. Container finds/compiles the JSP into a servlet.
3. The servlet executes scriptlets and expressions.
4. HTML is emitted to the response as the page.
5. The browser receives the rendered HTML.

## 7. Memory Representation

```
welcome.jsp ──compile──▶ Welcome_jsp extends HttpJspBase
  ├─ HTML ──────▶ out.write("<h1>Hello</h1>")
  ├─ <%= now %> ─▶ out.write(now)
  └─ <% code %> ─▶ inline Java
```

## 8. Simple Example

```jsp
<html>
<body>
  <h1>Welcome</h1>
  <p>2 + 3 = <%= 2 + 3 %></p>
</body>
</html>
```

## 9. Intermediate Example

```jsp
<%@ page import="java.util.*" %>
<html>
<body>
  <ul>
    <%
      List<String> names = List.of("Alice", "Bob", "Carol");
      for (String n : names) {
    %>
      <li><%= n %></li>
    <%
      }
    %>
  </ul>
</body>
</html>
```

## 10. Advanced Example

```jsp
<%@ page import="com.example.UserService" %>
<%@ page import="com.example.User" %>
<%
    UserService service = new UserService();
    List<User> users = service.findAll();
%>
<html>
<body>
  <table border="1">
    <tr><th>ID</th><th>Name</th></tr>
    <% for (User u : users) { %>
      <tr><td><%= u.getId() %></td><td><%= u.getName() %></td></tr>
    <% } %>
  </table>
</body>
</html>
```

## 11. Output

```
Hello
2 + 3 = 5
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Missing `<%@ page import %>` | Compilation error | Import Java classes used |
| Business logic in JSP | Unmaintainable views | Keep logic in servlets/services |
| Forgetting to close scriptlets | Broken page | Match `<%` and `%>` |
| Not escaping user data | XSS | Escape output (c:out) |

## 13. Best Practices

- Keep JSP purely for **view** (MVC) — logic lives elsewhere.
- Use expressions and later EL/JSTL instead of raw scriptlets.
- Import only what the page needs.
- Put shared layout in include files or tag files.

## 14. Interview Questions

**Q: What is JSP?**
A Java view technology — HTML with embedded Java that the container compiles into a servlet.

**Q: How is JSP related to servlets?**
A JSP is compiled into a servlet at runtime; JSP is the view layer, servlets are the controller.

**Q: What are the main JSP elements?**
Directives, scriptlets, expressions, declarations, and comments.

## 15. Practice Exercises

1. Create a JSP that shows the current date.
2. Loop over a list and render each item.
3. Import a utility class and use it in the page.

## 16. Coding Challenges

1. Build a JSP that reads a `name` query parameter and greets it (with escaping).
2. Combine a servlet (controller) with a JSP (view) by forwarding request attributes.

## 17. Summary

- JSP = HTML + embedded Java, compiled into servlets.
- Elements: directives, scriptlets, expressions, declarations.
- Use it as the view in servlet MVC apps.
- Avoid heavy logic in JSP.
- Next, clean up JSP with **JSP-Scripting-and-EL.md**.