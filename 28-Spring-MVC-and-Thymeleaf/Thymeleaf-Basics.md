# Thymeleaf Basics

## 1. Introduction

**Thymeleaf** is the modern server-side template engine for Spring MVC. It renders HTML with data from the model, works with **natural templates** (valid HTML that also previews in a browser), and replaces JSP for most new Spring applications.

## 2. Why This Topic Is Important

- Thymeleaf is the default view engine in Spring Boot.
- Server-rendered Spring apps, admin panels, and email templates use it.
- Knowing `th:` attributes lets you build dynamic pages without writing Java in templates.

## 3. Real-World Analogy

Thymeleaf is like a **fill-in-the-blank party invitation**. The printed invitation has blank spaces; when the party (request) happens, someone fills in the names and dates (`th:text`). Before the party, the blank invitation is still perfectly readable as a normal HTML file.

## 4. Theory

### Core `th:` attributes

| Attribute | Purpose |
|-----------|---------|
| `th:text` | Set text content |
| `th:each` | Loop over a collection |
| `th:if` / `th:unless` | Conditionals |
| `th:href` / `th:src` | Dynamic URLs/links |
| `th:field` | Form binding |
| `th:class` | Dynamic CSS classes |
| `th:object` | Bean navigation context |

### Expression syntax

- `${product.name}` — model/bean value.
- `*{name}` — value relative to `th:object`.
- `#{...}` — messages (i18n).
- `@{/products/1}` — URL expression.

### Setup

Spring Boot: add `spring-boot-starter-thymeleaf`; templates go in `src/main/resources/templates`.

## 5. Syntax

```html
<html xmlns:th="http://www.thymeleaf.org">
<body>
  <h1 th:text="${title}">Default title</h1>
  <ul>
    <li th:each="p : ${products}"
        th:text="${p.name}">Product name</li>
  </ul>
</body>
</html>
```

## 6. Flow Explanation

1. Controller sets model attributes and returns the view name.
2. ThymeleafViewResolver locates the template.
3. The template engine evaluates `th:` attributes against the model.
4. Dynamic values replace the placeholder text in the HTML.
5. The rendered HTML is returned to the browser.

## 7. Memory Representation

```
Model {title: "Welcome", products: [Keyboard, Mouse]}
   ↓
<h1 th:text="${title}">...</h1>  →  <h1>Welcome</h1>
<li th:each="p : ${products}" .../> → two <li> elements
```

## 8. Simple Example

```html
<h1 th:text="${message}">Hello!</h1>
<p>Current time: <span th:text="${now}">12:00</span></p>
```

## 9. Intermediate Example

```html
<ul>
  <li th:each="p : ${products}" th:if="${#lists.size(products) > 0}">
    <a th:href="@{/products/{id}(id=${p.id})}"
       th:text="${p.name}">Product</a>
    <span th:unless="${p.stock > 0}" th:text="'Out of stock'"></span>
  </li>
</ul>
```

## 10. Advanced Example

```html
<tr th:each="o : ${orders}"
    th:class="${o.status == 'CANCELLED'} ? 'danger' : 'ok'">
  <td th:text="${o.id}">1</td>
  <td th:text="${#numbers.formatDecimal(o.total, 1, 2)}">0.00</td>
  <td th:text="${o.status}">NEW</td>
</tr>
```

## 11. Output

With model `{title: "Welcome", products: [...]}`, the browser sees:

```html
<h1>Welcome</h1>
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Missing `xmlns:th` | IDE warnings / broken preview | Add the namespace |
| Forgetting Thymeleaf dependency | HTML returned literally | Add the starter |
| Using `${}` for URLs | Wrong path | Use `@{...}` |
| Complex Java logic in templates | Unmaintainable | Precompute in controller |

## 13. Best Practices

- Keep templates declarative — logic lives in the controller.
- Use `th:each` with a sensible loop variable (`th:each="p : ${products}"`).
- Use `@{...}` for URLs so context path is handled.
- Preview templates as natural HTML where possible.
- Reuse fragments with `th:fragment` / `th:replace` for shared layout.

## 14. Interview Questions

**Q: What is Thymeleaf?**
A modern server-side Java template engine that renders HTML by evaluating `th:` attributes against model data.

**Q: How does Thymeleaf integrate with Spring MVC?**
Spring Boot auto-configures ThymeleafViewResolver; controllers return view names that resolve to templates under `templates/`.

**Q: What is a natural template?**
An HTML file that is both valid HTML (renders in a browser with placeholder text) and a template (dynamic values via `th:` attributes).

## 15. Practice Exercises

1. Render a model attribute with `th:text`.
2. Loop over a list with `th:each`.
3. Add a conditional `th:if`/`th:unless` based on a boolean field.

## 16. Coding Challenges

1. Build a product table where rows get a CSS class based on stock level.
2. Create a shared header fragment and include it in two pages with `th:replace`.

## 17. Summary

- Thymeleaf = natural HTML + `th:` attributes.
- `${...}` model values, `@{...}` URLs, `th:each` loops, `th:if` conditions.
- Templates live in `src/main/resources/templates`.
- Keep logic in controllers, not templates.
- Next, handle forms in **Forms-and-Validation.md**.