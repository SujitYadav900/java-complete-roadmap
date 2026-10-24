# Forms and Validation

## 1. Introduction

Server-rendered apps live on **forms**: creating, editing, and submitting data. Spring MVC binds form fields to objects with `@ModelAttribute`, and **Bean Validation** (`@Valid` + annotations) checks the data before it reaches your service layer.

## 2. Why This Topic Is Important

- Validation errors must be re-displayed to the user with the original input.
- Bean Validation annotations are the standard across all Spring apps (including REST).
- Interviewers frequently ask how form binding and validation work together.

## 3. Real-World Analogy

A form is like a **job application paper**. You fill it out (the browser), the clerk (Spring) transcribes it onto a standard card (the object), and the reviewer (validator) checks every required field — returning the card with notes if something is missing, rather than silently discarding it.

## 4. Theory

### Form binding

- `@ModelAttribute ProductForm form` binds request parameters to the object's setters.
- The same object is added to the Model, so the form can re-display input.
- Thymeleaf binds with `th:field="*{name}"` under `th:object="${productForm}"`.

### Bean Validation annotations

| Annotation | Rule |
|------------|------|
| `@NotNull` | Value is not null |
| `@NotBlank` | String not null/empty/whitespace |
| `@Size(min, max)` | Length in range |
| `@Min` / `@Max` | Numeric bounds |
| `@Email` | Valid email format |
| `@Pattern` | Regex match |

### The validation flow

1. `@Valid` triggers validation of the bound object.
2. Errors go into a `BindingResult`.
3. If errors exist, return the form view to show them.
4. Otherwise, process and redirect.

## 5. Syntax

```java
public class ProductForm {
    @NotBlank(message = "Name is required")
    private String name;

    @NotNull(message = "Price is required")
    @DecimalMin(value = "0.0", message = "Price must be >= 0")
    private BigDecimal price;
    // getters/setters
}
```

```java
@PostMapping("/products")
public String create(@Valid @ModelAttribute ProductForm form,
                     BindingResult result) {
    if (result.hasErrors()) {
        return "product-form";   // re-render with errors
    }
    productService.save(form);
    return "redirect:/products";
}
```

## 6. Flow Explanation

1. Browser submits the form to POST `/products`.
2. Spring binds fields to a new `ProductForm`.
3. `@Valid` runs Bean Validation rules.
4. Violations land in `BindingResult`.
5. `result.hasErrors()` → return the form view (errors displayed).
6. No errors → save and redirect.

## 7. Memory Representation

```
POST name=&price=-5
  ↓ binding
ProductForm {name: "", price: -5}
  ↓ @Valid
errors: name "required", price "must be >= 0"
  ↓
return "product-form"  →  errors shown, input preserved
```

## 8. Simple Example

```java
@PostMapping("/register")
public String register(@Valid @ModelAttribute UserForm form,
                       BindingResult result) {
    if (result.hasErrors()) return "register";
    userService.register(form);
    return "redirect:/login";
}
```

## 9. Intermediate Example

```java
public class UserForm {
    @NotBlank @Size(min = 3, max = 20)
    private String username;

    @NotBlank @Email
    private String email;

    @NotBlank @Size(min = 8)
    private String password;
}
```

```html
<form th:action="@{/register}" th:object="${userForm}" method="post">
  <input type="text" th:field="*{username}"/>
  <span th:errors="*{username}"></span>

  <input type="email" th:field="*{email}"/>
  <span th:errors="*{email}"></span>

  <input type="password" th:field="*{password}"/>
  <span th:errors="*{password}"></span>

  <button type="submit">Register</button>
</form>
```

## 10. Advanced Example

```java
@GetMapping("/products/{id}/edit")
public String editForm(@PathVariable Long id, Model model) {
    model.addAttribute("productForm", toForm(productService.findById(id)));
    return "product-form";
}

@PostMapping("/products/{id}")
public String update(@PathVariable Long id,
                     @Valid @ModelAttribute ProductForm form,
                     BindingResult result) {
    if (result.hasErrors()) return "product-form";
    productService.update(id, form);
    return "redirect:/products/" + id;
}
```

## 11. Output

On error, the same page renders with messages:

```
Name is required
Price must be >= 0
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Missing `BindingResult` right after `@Valid` | Spring throws BindException | Keep them adjacent |
| Forgetting `@Valid` | No validation runs | Add `@Valid` |
| Returning redirect on error | Error messages lost | Return the form view |
| Field name mismatch | Binding silently empty | Match names or use `@RequestParam` |

## 13. Best Practices

- Put `BindingResult` immediately after the validated parameter.
- Validate at the boundary (controller) and keep services defensive.
- Add a class-level form object for edit vs create flows.
- Use `th:errors` for messages, `th:field` for binding.
- Never trust the client — always validate server-side.

## 14. Interview Questions

**Q: How does `@ModelAttribute` work with forms?**
It binds request parameters to an object's setters and adds the object to the model for the view.

**Q: What is the role of `BindingResult`?**
It captures validation errors from `@Valid`; check `hasErrors()` and re-render the form if needed.

**Q: Why must `BindingResult` come directly after the validated parameter?**
Spring requires that method signature; otherwise a BindException is thrown.

## 15. Practice Exercises

1. Create a form object with `@NotBlank` and `@Size`.
2. Bind it in a POST handler with `@Valid` + `BindingResult`.
3. Display `th:errors` in the view.

## 16. Coding Challenges

1. Build a full create form: GET shows the empty form, POST validates and re-renders with errors, then redirects on success.
2. Add `@Email` and `@Pattern` validation and test messages for each field.

## 17. Summary

- `@ModelAttribute` binds forms; `@Valid` + annotations validate.
- `BindingResult` carries errors; return the view on failure.
- `th:field` and `th:errors` keep input and messages on the page.
- Always validate server-side.
- Next, handle failures gracefully in **MVC-Error-Handling.md**.