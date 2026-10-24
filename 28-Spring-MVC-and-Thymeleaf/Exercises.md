# Spring MVC & Thymeleaf Exercises

> Practice the Spring MVC & Thymeleaf concepts from Module 28. Use Spring Boot with `spring-boot-starter-thymeleaf`.

## 1. Beginner Exercises

1. Create a `@Controller` with a `GET /` returning a `home` view.
2. Add a model attribute and render it with `th:text`.
3. Add a `GET /greet?name=...` that defaults the name and shows it.

## 2. Mapping Exercises

1. Add a route with a path variable: `GET /products/{id}`.
2. Read a `@RequestParam` with a default value.
3. Create a POST endpoint and test it with a form.

## 3. Thymeleaf Exercises

1. Loop over a list of products with `th:each`.
2. Show/hide rows with `th:if` based on a `stock` field.
3. Build a URL with `@{/products/{id}(id=${p.id})}` and verify the link.

## 4. Forms & Validation Exercises

1. Create a `ProductForm` with `@NotBlank` and `@DecimalMin`.
2. Bind it with `@ModelAttribute` + `@Valid` + `BindingResult`.
3. Show `th:errors` next to each field and confirm input is preserved.

## 5. Layout & Error Exercises

1. Create a shared header fragment with `th:fragment` and include it on two pages.
2. Add a `@ControllerAdvice` that maps a custom exception to a 404 view.
3. Add a custom `error.html` and trigger a 500.

## 6. Challenge: Product CRUD

1. Build list (GET), create form (GET/POST), and detail (GET) views.
2. Validate the create form and re-render with errors on failure.
3. Redirect after a successful save (Post/Redirect/Get).

## 7. Challenge: Orders dashboard

1. Render an orders table with a CSS class based on status.
2. Add an order-detail page with formatted currency using `#numbers`.
3. Handle a missing order with a friendly 404 page via `@ControllerAdvice`.

> **You've mastered Spring MVC.** Move to **[29 — Spring Security](../29-Spring-Security/README.md)** — protect the apps you just built.