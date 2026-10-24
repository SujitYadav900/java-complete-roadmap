# Validation & Error Handling

## 1. Introduction

Good APIs **validate input** and return **consistent, useful errors**. Spring uses Bean Validation (`@Valid`) plus `@RestControllerAdvice` for clean error responses.

---

## 2. Why This Topic Is Important

- Invalid input must fail with 400, not 500.
- Consistent errors make clients reliable.
- Validation annotations remove boilerplate.
- Interviewers test exception handling patterns.

---

## 3. Real-World Analogy

A **bank teller** checks your ID before every transaction (validation) and clearly tells you why something failed (error handling) — "no account found" instead of just "sorry." Your API should do the same.

---

## 4. Theory

### Bean Validation Annotations
| Annotation | Rule |
|------------|------|
| `@NotNull` | Value required |
| `@NotBlank` | String not empty/whitespace |
| `@Size(min, max)` | Length/count bounds |
| `@Min` / `@Max` | Numeric bounds |
| `@Email` | Email format |
| `@Pattern` | Regex match |
| `@Positive` | Greater than zero |

### Enabling Validation
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

### Error Handling Strategy
| Piece | Role |
|-------|------|
| `@Valid` | Trigger validation on the body |
| `@RestControllerAdvice` | Global exception handler |
| `@ExceptionHandler` | Map exceptions → responses |
| `ProblemDetail` | Standard RFC 7807 error body |

---

## 5. Syntax

```java
@PostMapping
public Task create(@Valid @RequestBody TaskRequest request) { ... }
```

```java
@RestControllerAdvice
class ApiExceptionHandler {
    @ExceptionHandler(NotFoundException.class)
    @ResponseStatus(HttpStatus.NOT_FOUND)
    public ErrorBody notFound(NotFoundException e) { ... }
}
```

---

## 6. Flow Explanation

```mermaid
flowchart TD
    A[Request] --> B[@Valid]
    B -->|invalid| C[MethodArgumentNotValidException]
    C --> D[Advice builds 400 body]
    B -->|valid| E[Service]
    E -->|error| F[Custom exception]
    F --> G[Advice maps status]
```

---

## 7. Memory Representation

Skip — handling pattern.

---

## 8. Simple Example — DTO Validation

```java
import jakarta.validation.constraints.*;

record TaskRequest(
        @NotBlank(message = "title is required")
        String title,

        @Size(min = 3, max = 100, message = "description 3-100 chars")
        String description,

        @Positive(message = "priority must be positive")
        int priority
) { }
```

```java
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/tasks")
class TaskController {

    @PostMapping
    public String create(@Valid @RequestBody TaskRequest request) {
        return "Created: " + request.title();
    }
}
```

---

## 9. Intermediate Example — Global Error Body

```java
import org.springframework.http.*;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.bind.MethodArgumentNotValidException;

record ErrorBody(String error, String message, int status) { }

@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    public ErrorBody handleValidation(MethodArgumentNotValidException ex) {
        String message = ex.getBindingResult().getFieldErrors().stream()
                .map(e -> e.getField() + ": " + e.getDefaultMessage())
                .reduce((a, b) -> a + "; " + b)
                .orElse("validation failed");
        return new ErrorBody("validation_error", message, 400);
    }

    @ExceptionHandler(NotFoundException.class)
    @ResponseStatus(HttpStatus.NOT_FOUND)
    public ErrorBody handleNotFound(NotFoundException ex) {
        return new ErrorBody("not_found", ex.getMessage(), 404);
    }
}

class NotFoundException extends RuntimeException {
    NotFoundException(String m) { super(m); }
}
```

---

## 10. Advanced Example — RFC 7807 ProblemDetail

```java
import org.springframework.http.*;
import org.springframework.web.bind.annotation.*;

@RestControllerAdvice
public class ProblemDetailAdvice {

    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<ProblemDetail> handleBadRequest(IllegalArgumentException ex) {
        ProblemDetail pd = ProblemDetail.forStatusAndDetail(
                HttpStatus.BAD_REQUEST, ex.getMessage());
        pd.setTitle("Invalid argument");
        pd.setProperty("timestamp", System.currentTimeMillis());
        return ResponseEntity.badRequest().body(pd);
    }
}

@RestController
@RequestMapping("/api/orders")
class OrderController {

    @GetMapping("/{id}")
    public String get(@PathVariable Long id) {
        if (id < 1) {
            throw new IllegalArgumentException("id must be >= 1");
        }
        return "Order " + id;
    }
}
```

---

## 11. Output

For the **Simple Example** (`POST /api/tasks` with blank title):

```
400 Bad Request

{
  "error": "validation_error",
  "message": "title: title is required",
  "status": 400
}
```

For the **Advanced Example**:

```
400 Bad Request
{
  "type": "about:blank",
  "title": "Invalid argument",
  "status": 400,
  "detail": "id must be >= 1",
  "timestamp": 1720000000000
}
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| Missing @Valid | Validation never runs |
| No validation starter | Annotations ignored |
| Throwing 500 for client errors | Map to 400/404 |
| Inconsistent error bodies | One global advice |
| Leaking stack traces | Keep messages user-safe |

---

## 13. Best Practices

- Validate every @RequestBody with @Valid.
- Use a global @RestControllerAdvice.
- Return consistent error bodies.
- Prefer ProblemDetail (RFC 7807).
- Never expose stack traces to clients.

---

## 14. Interview Questions

**Q1. How do you validate a request?**
*Bean Validation annotations + @Valid.*

**Q2. What is @RestControllerAdvice?**
*Global exception handler returning structured errors.*

**Q3. 400 vs 404?**
*400 bad input; 404 resource missing.*

**Q4. What is ProblemDetail?**
*RFC 7807 standard error format.*

**Q5. Why consistent errors?**
*Clients depend on the shape for handling.*

---

## 15. Practice Exercises

1. Add validation to a DTO.
2. Build a global exception handler.
3. Return a custom 404 body.
4. Use ProblemDetail for errors.

---

## 16. Coding Challenges

1. **Easy:** Validate title on a task DTO.
2. **Medium:** Global handler for validation + not-found.
3. **Hard:** RFC 7807 errors with custom properties.

---

## 17. Summary

- @Valid + Bean Validation annotations.
- @RestControllerAdvice for global errors.
- Consistent bodies: status, error, message.
- ProblemDetail = RFC 7807 standard.
- Validate input; map errors to right codes.

➡️ Continue to: [REST Consumption](REST-Consumption.md)