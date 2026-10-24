# MVC Error Handling

## 1. Introduction

In Spring MVC, errors should be handled **consistently** — not in every controller. A `@ControllerAdvice` class centralizes exception handling, while a custom error page covers everything else. This keeps controllers clean and users informed.

## 2. Why This Topic Is Important

- Centralized handling prevents duplicated try/catch and inconsistent responses.
- A polished error page separates a professional app from a stack trace dump.
- Interviewers ask how Spring handles exceptions and when `@ControllerAdvice` runs.

## 3. Real-World Analogy

A `@ControllerAdvice` is like the **help desk that handles every complaint in a building**. Tenants (controllers) don't each invent their own way to deal with leaks; they all file reports to the help desk, which has one consistent response procedure.

## 4. Theory

### How exceptions flow in Spring MVC

1. A controller method throws an exception.
2. Spring looks for a matching `@ExceptionHandler` in the controller first.
3. If none, it looks in `@ControllerAdvice` classes (global).
4. A matching handler runs — it can return a view name, a `ModelAndView`, or a `ResponseEntity`.

### Key annotations

| Annotation | Purpose |
|------------|---------|
| `@ControllerAdvice` | Global handlers for all controllers |
| `@ExceptionHandler(Exception.class)` | Handle a specific exception type |
| `@ResponseStatus(HttpStatus.NOT_FOUND)` | Set the response status |

### Error page (Spring Boot)

Customize `templates/error.html` — Spring Boot renders it automatically for unhandled errors and forwards to `/error`.

## 5. Syntax

```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    @ResponseStatus(HttpStatus.NOT_FOUND)
    public String handleNotFound(ResourceNotFoundException ex, Model model) {
        model.addAttribute("message", ex.getMessage());
        return "error/not-found";
    }

    @ExceptionHandler(Exception.class)
    public String handleOther(Exception ex, Model model) {
        model.addAttribute("message", "Something went wrong.");
        return "error/generic";
    }
}
```

## 6. Flow Explanation

1. Controller throws `ResourceNotFoundException`.
2. Spring scans `@ExceptionHandler` methods (local, then global advice).
3. The matching method runs.
4. It sets a 404 status and adds a message to the Model.
5. The view `error/not-found` renders.

## 7. Memory Representation

```
Controller → throws ResourceNotFoundException
   │
   ▼
GlobalExceptionHandler.handleNotFound()
   → status 404, model {message}, view "error/not-found"
```

## 8. Simple Example

```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
    public String handleGeneric(Model model) {
        model.addAttribute("message", "We hit an unexpected problem.");
        return "error/generic";
    }
}
```

## 9. Intermediate Example

```java
public class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String message) {
        super(message);
    }
}
```

```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    @ResponseStatus(HttpStatus.NOT_FOUND)
    public String handleNotFound(Model model) {
        model.addAttribute("message", "That resource does not exist.");
        return "error/not-found";
    }
}
```

## 10. Advanced Example

```java
// Per-controller handler (rare) plus global advice
@Controller
public class OrderController {

    @ExceptionHandler(OrderLockedException.class)
    public String handleLocked(OrderLockedException ex, Model model) {
        model.addAttribute("message", ex.getMessage());
        return "error/order-locked";
    }
}
```

```html
<!-- templates/error.html -->
<html>
<body>
  <h1>Oops</h1>
  <p th:text="${message}">Something went wrong.</p>
  <a th:href="@{/}">Back to home</a>
</body>
</html>
```

## 11. Output

Requesting a missing order renders `error/not-found` with status 404 and the friendly message.

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Catching `Exception` everywhere | Swallowed bugs | Let them bubble to the advice |
| Returning 200 on errors | Confusing clients | Set `@ResponseStatus` |
| Logging the exception twice | Duplicate noise | Log once in the handler |
| Exposing raw exception messages | Information leak | Show generic messages |

## 13. Best Practices

- Centralize handling in `@ControllerAdvice`.
- Map domain exceptions to specific statuses and views.
- Log the exception (with stack trace) in the handler.
- Provide a generic fallback handler plus a custom `error.html`.
- Keep user-facing messages generic; log the details.

## 14. Interview Questions

**Q: What is `@ControllerAdvice`?**
A class that applies advice to all controllers, most commonly holding global `@ExceptionHandler` methods.

**Q: What order are exception handlers consulted?**
Local controller handlers first, then `@ControllerAdvice` handlers; the most specific matching type wins.

**Q: How does Spring Boot render a default error page?**
Unhandled errors forward to `/error`, which Spring Boot handles — customizable via `templates/error.html`.

## 15. Practice Exercises

1. Create a `@ControllerAdvice` with a generic handler returning an error view.
2. Throw a custom exception and map it to 404.
3. Add `templates/error.html` and trigger a 500 to see it.

## 16. Coding Challenges

1. Build handlers for three exception types with three distinct status codes and views.
2. Log each handled exception once, with stack trace, and confirm the response never leaks internal details.

## 17. Summary

- `@ControllerAdvice` centralizes exception handling.
- `@ExceptionHandler` + `@ResponseStatus` map exceptions to views/statuses.
- Custom `error.html` covers unhandled errors.
- Log details; show users generic messages.
- You can now build complete server-rendered apps — next, secure them in **Module 29 — Spring Security**.