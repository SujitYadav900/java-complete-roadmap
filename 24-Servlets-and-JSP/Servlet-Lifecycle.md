# Servlet Lifecycle

## 1. Introduction

The servlet **lifecycle** is the sequence of events the container drives: **load and init, serve many requests, then destroy**. Knowing this explains when code runs once, when it runs per-request, and why thread-safety matters.

## 2. Why This Topic Is Important

- Lifecycle questions are a classic interview topic.
- It clarifies where to put setup (init) and cleanup (destroy).
- It reveals why instance fields on a servlet are shared across threads.

## 3. Real-World Analogy

A servlet is like a **staffed checkout register at a store**: the cashier comes in once (init), handles hundreds of customers (service calls) throughout the day, and goes home at closing (destroy). The register itself is one shared piece of equipment used by all customers.

## 4. Theory

### The three lifecycle methods

| Method | When | Purpose |
|--------|------|---------|
| `init()` | Once, when the servlet is first loaded | Open connections, load config |
| `service()` | Every request | Dispatch to doGet/doPost/... |
| `destroy()` | Once, before the servlet is removed | Close resources, cleanup |

### Lifecycle rules

- The container instantiates the servlet and calls `init()` once.
- Each request runs `service()` on its own thread — possibly concurrently.
- `destroy()` is called before shutdown/reload.
- If `init()` throws, the servlet is discarded.

## 5. Syntax

```java
@WebServlet("/demo", loadOnStartup = 1)
public class LifecycleServlet extends HttpServlet {

    @Override
    public void init() throws ServletException {
        System.out.println("init() called once");
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws IOException {
        System.out.println("service/doGet called per request");
        resp.getWriter().write("done");
    }

    @Override
    public void destroy() {
        System.out.println("destroy() called");
    }
}
```

## 6. Flow Explanation

1. Container loads the servlet class (at startup if `loadOnStartup`, else on first request).
2. `init()` runs once — set up shared resources here.
3. For each request, `service(req, resp)` dispatches to the matching `doGet`/`doPost`.
4. On shutdown or redeploy, `destroy()` runs — release resources.
5. After destroy, the instance is garbage-collected.

## 7. Memory Representation

```
load → init() [once]
        ├─ request1 → service() → doGet
        ├─ request2 → service() → doGet   (may run in parallel)
        └─ ...
shutdown → destroy() [once]
```

## 8. Simple Example

```java
@WebServlet("/once")
public class OnceServlet extends HttpServlet {
    private int hits = 0;   // shared across threads!

    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws IOException {
        hits++;                          // NOT thread-safe
        resp.getWriter().write("hits=" + hits);
    }
}
```

## 9. Intermediate Example

```java
@WebServlet("/config", loadOnStartup = 1)
public class ConfigServlet extends HttpServlet {
    private DataSource dataSource;

    @Override
    public void init() {
        // Runs once at startup
        dataSource = createDataSource();
    }

    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws IOException {
        resp.getWriter().write("DataSource ready: " + (dataSource != null));
    }
}
```

## 10. Advanced Example

```java
public class ManagedServlet extends HttpServlet {
    private ExecutorService pool;

    @Override
    public void init() {
        pool = Executors.newFixedThreadPool(4);
    }

    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws IOException {
        // Use the pool for background work
        resp.getWriter().write("ok");
    }

    @Override
    public void destroy() {
        if (pool != null) {
            pool.shutdownNow();   // clean shutdown
        }
    }
}
```

## 11. Output

Server console (one instance, many requests):

```
init() called once
service/doGet called per request
service/doGet called per request
service/doGet called per request
destroy() called
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Putting per-request setup in `init` | Wrong data shared across requests | Use method-local state |
| Mutable instance fields | Race conditions | Avoid or synchronize |
| Creating connections per request in `init` | Leaked resources | Close them in `destroy` |
| Throwing from `init` | Servlet discarded | Handle setup failures gracefully |

## 13. Best Practices

- `init` for one-time setup; `destroy` for one-time cleanup.
- Keep instance state immutable or thread-safe.
- Use `loadOnStartup = 1` to initialize important servlets eagerly.
- Do request-scoped work in `service`/`doGet` with local variables.

## 14. Interview Questions

**Q: What are the lifecycle methods of a servlet?**
`init()`, `service()`, `destroy()`.

**Q: Is a servlet instance created per request?**
No — one instance is created and reused for all requests; only the threads differ.

**Q: What happens if `init()` throws?**
The servlet is discarded and not used; the container may retry later.

## 15. Practice Exercises

1. Print from `init`, `doGet`, and `destroy` and observe the order.
2. Add `loadOnStartup = 1` and confirm `init` runs at startup.
3. Hit the servlet 10 times and confirm one init, ten service calls.

## 16. Coding Challenges

1. Use `init` to open a single shared resource and `destroy` to close it; prove it is created once with console logs.
2. Detect the shared-instance behavior by returning a counter and observing interleaved hits under concurrency.

## 17. Summary

- Lifecycle: load → `init()` (once) → `service()` (per request) → `destroy()` (once).
- One instance serves all requests — shared fields need thread-safety.
- `init`/`destroy` are for resource setup/cleanup.
- `loadOnStartup` controls eager loading.
- Next, master the HTTP details in **Servlets-and-HTTP.md**.