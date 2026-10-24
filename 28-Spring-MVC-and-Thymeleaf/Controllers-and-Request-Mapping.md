# Controllers and Request Mapping

## 1. Introduction

Controllers are the classes that handle requests. The **request mapping** annotations (`@GetMapping`, `@PostMapping`, `@RequestMapping`, and friends) declare which URL, HTTP method, and parameters each handler accepts.

## 2. Why This Topic Is Important

- Mapping annotations are the heart of Spring MVC and REST controllers alike.
- Knowing how to extract data — path variables, query params, headers — is a daily skill.
- Interviews quiz URL design and annotation usage constantly.

## 3. Real-World Analogy

Mappings are like the **menu of a restaurant with a floor plan**. The URL is the address; the HTTP method is the occasion (GET = "I want to see the menu", POST = "I'm ordering"). Path variables are table numbers, and query params are special requests ("no onions").

## 4. Theory

### Core annotations

| Annotation | Meaning |
|------------|---------|
| `@Controller` | Class is a Spring MVC controller (views) |
| `@RestController` | Controller returning data directly (JSON) |
| `@RequestMapping(path, method)` | Base mapping |
| `@GetMapping` / `@PostMapping` / `@PutMapping` / `@DeleteMapping` | Shortcuts for one HTTP method |

### Reading request data

| Annotation | Reads |
|------------|-------|
| `@PathVariable` | `{id}` in the URL path |
| `@RequestParam` | Query/URL parameter |
| `@RequestHeader` | HTTP header |
| `@RequestBody` | Request body (JSON) |
| `@ModelAttribute` | Form fields bound to an object |

### Mapping examples

```text
@GetMapping("/users/{id}")      → GET /users/42
@GetMapping("/search?q=java")   → q = "java"
@PostMapping("/users")          → POST /users with body
```

## 5. Syntax

```java
@Controller
public class UserController {

    @GetMapping("/users/{id}")
    public String view(@PathVariable Long id, Model model) {
        model.addAttribute("user", userService.findById(id));
        return "user-view";
    }
}
```

## 6. Flow Explanation

1. DispatcherServlet receives the request.
2. HandlerMapping matches the URL + method to a handler method.
3. Spring resolves method parameters from the request (path, params, headers, body).
4. The method executes and returns a view name (or body).
5. The view renders the response.

## 7. Memory Representation

```
GET /users/42?debug=true
   ├─ path variable: id = 42
   ├─ request param: debug = "true"
   └─ header: Accept: text/html
```

## 8. Simple Example

```java
@GetMapping("/welcome")
public String welcome(@RequestParam(defaultValue = "Guest") String name,
                      Model model) {
    model.addAttribute("name", name);
    return "welcome";
}
```

## 9. Intermediate Example

```java
@PostMapping("/products")
public String create(@ModelAttribute ProductForm form, Model model) {
    model.addAttribute("created", productService.save(form));
    return "product-confirmation";
}
```

## 10. Advanced Example

```java
@GetMapping("/orders/{id}")
public String show(
        @PathVariable Long id,
        @RequestParam(required = false) boolean detailed,
        @RequestHeader(value = "X-Region", defaultValue = "eu") String region,
        Model model) {
    model.addAttribute("order", orderService.findById(id, region, detailed));
    return "order-view";
}
```

## 11. Output

`GET /orders/7?detailed=true` with header `X-Region: us` → the view renders order `7`, detailed, region `us`.

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Missing `@PathVariable` name | 500 / type mismatch | Match the name or set it explicitly |
| Wrong HTTP method | 405 Method Not Allowed | Use the matching mapping annotation |
| `@RequestBody` on a form submit | Unparseable / missing body | Use `@ModelAttribute` |
| Two mappings to the same URL | Mapping conflict at startup | Keep URLs unique |

## 13. Best Practices

- Keep URLs plural and consistent (`/products/{id}`).
- Use `@RequestParam(defaultValue=...)` for optional params.
- Validate all path/query input before use.
- Use `@ModelAttribute` for form binding, `@RequestBody` for JSON.

## 14. Interview Questions

**Q: What is the difference between `@RequestParam` and `@PathVariable`?**
`@RequestParam` reads query/form parameters (`?id=1`); `@PathVariable` reads values embedded in the URL path (`/orders/1`).

**Q: What does `@ModelAttribute` do?**
It binds request parameters (form fields) to an object's properties and adds it to the model.

**Q: How do you restrict a method to POST?**
Use `@PostMapping` or `@RequestMapping(method = RequestMethod.POST)`.

## 15. Practice Exercises

1. Create a controller with GET and POST mappings.
2. Read a path variable and a query parameter in one method.
3. Bind a form to an object with `@ModelAttribute`.

## 16. Coding Challenges

1. Build a `/products` controller with a list view (GET) and a create form (POST).
2. Add a `@RequestHeader` requirement and test a request without it.

## 17. Summary

- `@GetMapping`/`@PostMapping`/etc. map HTTP methods + URLs to handlers.
- `@PathVariable`, `@RequestParam`, `@RequestHeader`, `@ModelAttribute` extract request data.
- Keep controllers thin and URLs consistent.
- Next, pass data to views with **Model-and-View-Resolution.md**.