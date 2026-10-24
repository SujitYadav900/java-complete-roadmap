# Model and View Resolution

## 1. Introduction

A Spring MVC handler returns a **view name** and populates a **Model**. The **ViewResolver** turns that name into an actual template, and the **View** renders it using the model data. Understanding this pipeline is the key to passing data into pages.

## 2. Why This Topic Is Important

- Every server-rendered page flows through Model → ViewResolver → View.
- Misplaced data (in the wrong scope or not in the model at all) is a common bug.
- Interviewers ask how view names resolve to templates.

## 3. Real-World Analogy

The controller is like a **chef who fills a ticket and names the dish**: "Plate 3, spaghetti with meatballs." The ViewResolver is the **server who knows the kitchen's naming convention** — "Plate 3" means "Table 3, order 12" — and fetches the exact dish (template) to serve.

## 4. Theory

### The Model

- A map of attribute name → value, passed to the view.
- Filled with `model.addAttribute("key", value)`.
- `ModelAndView` combines the model and view name in one object.

### ViewResolver

- Maps a logical view name to an actual view.
- Thymeleaf's resolver adds a prefix (`classpath:/templates/`) and suffix (`.html`).
- So `return "home"` → `templates/home.html`.

### Returning styles

```java
// Style 1: Model + view name
public String list(Model model) { model.addAttribute(...); return "list"; }

// Style 2: ModelAndView
public ModelAndView list() {
    ModelAndView mav = new ModelAndView("list");
    mav.addObject("products", products);
    return mav;
}
```

## 5. Syntax

```java
@GetMapping("/dashboard")
public String dashboard(Model model) {
    model.addAttribute("stats", statsService.summarize());
    model.addAttribute("recent", auditService.recent());
    return "dashboard";
}
```

## 6. Flow Explanation

1. The controller adds attributes to the Model.
2. It returns a logical view name (e.g., `"dashboard"`).
3. DispatcherServlet asks the ViewResolver to resolve it.
4. ThymeleafViewResolver builds `templates/dashboard.html`.
5. The view renders HTML using the model attributes.
6. The response is returned to the browser.

## 7. Memory Representation

```
Controller → Model { stats: {...}, recent: [...] }
             view name: "dashboard"
ViewResolver → templates/dashboard.html
View → dashboard.html + Model → rendered HTML
```

## 8. Simple Example

```java
@GetMapping("/about")
public String about(Model model) {
    model.addAttribute("company", "Acme Inc");
    return "about";
}
```

## 9. Intermediate Example

```java
@GetMapping("/reports")
public ModelAndView reports() {
    ModelAndView mav = new ModelAndView("report-list");
    mav.addObject("reports", reportService.all());
    return mav;
}
```

## 10. Advanced Example

```java
@GetMapping("/redirect/order/{id}")
public String afterSave(@PathVariable Long id) {
    // redirect: sends a 302; the browser then GETs /orders/{id}
    return "redirect:/orders/" + id;
}
```

## 11. Output

`GET /about` renders `about.html` with `Acme Inc` where `${company}` appears.

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Returning a name with no matching template | 500 | Match file under `templates/` |
| Adding to request scope instead of Model | Empty `${...}` in view | Use `model.addAttribute` |
| Using `@RestController` for pages | JSON instead of HTML | Use `@Controller` |
| Returning `ModelAndView` with no view name | Error | Set the view name |

## 13. Best Practices

- Prefer `Model` parameters for clarity (immutable style).
- Use `redirect:` for post-save navigation (PRG pattern).
- Keep view names simple and matching filenames.
- Put shared data (current user) via `@ModelAttribute` methods or session attributes.

## 14. Interview Questions

**Q: What does the ViewResolver do?**
It maps a logical view name to an actual view — Thymeleaf's resolver appends `templates/` prefix and `.html` suffix.

**Q: How do you pass data to a view?**
Add attributes to the Model; the view accesses them with `${...}`.

**Q: What is a redirect in MVC terms?**
Returning `"redirect:/url"` sends a 302 so the browser issues a new GET — used after form submissions to prevent resubmission.

## 15. Practice Exercises

1. Add several attributes to the Model and display them in a page.
2. Use `ModelAndView` for one controller method.
3. Add a redirect after a successful form POST.

## 16. Coding Challenges

1. Build a dashboard controller that passes three different data structures to one view.
2. Implement the Post/Redirect/Get pattern: POST creates, then redirects to the detail page.

## 17. Summary

- Model carries data; view name picks the template.
- ViewResolver maps `"home"` → `templates/home.html`.
- `ModelAndView` bundles both.
- Use `redirect:` for post-submit navigation.
- Next, build dynamic pages with **Thymeleaf-Basics.md**.