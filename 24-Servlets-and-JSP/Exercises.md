# Servlets & JSP Exercises

> Practice the Servlets & JSP concepts from Module 24. Run a servlet container (e.g., Tomcat via Maven's `cargo`/`tomcat7-maven-plugin`, or an embedded Tomcat).

## 1. Beginner Exercises

1. Create a servlet mapped to `/hello` that returns "Hello Servlet".
2. Read a `name` parameter and greet the user.
3. Return JSON with `Content-Type: application/json`.

## 2. Lifecycle Exercises

1. Add console prints to `init`, `doGet`, and `destroy`.
2. Use `loadOnStartup = 1` and confirm `init` runs at startup.
3. Hit the servlet repeatedly and observe one init, many service calls.

## 3. HTTP Exercises

1. Handle both GET and POST on one servlet.
2. Redirect to `/login` when a parameter is missing.
3. Forward to another servlet and verify the URL does not change.

## 4. Session & Cookie Exercises

1. Build a visit counter with `HttpSession`.
2. Store a `theme` cookie and read it back on the next request.
3. Implement a protected page: redirect to `/login` when not in session.

## 5. JSP Exercises

1. Create a JSP that prints the server time.
2. Loop over a list with `<c:forEach>` and print items.
3. Greet the user using `${param.name}` and `<c:out>`.

## 6. Challenge: Login + Cart

1. Build a login servlet that stores the user in the session.
2. Add a session-backed shopping cart servlet.
3. On logout, `invalidate()` the session and redirect.

## 7. Challenge: MVC with JSTL

1. Write a controller servlet that sets a list of orders as a request attribute.
2. Forward to a JSP that renders the table with EL + JSTL.
3. Add a condition that highlights out-of-stock rows.

> **You've mastered servlets.** Move to **[25 — Spring Core](../25-Spring-Core/README.md)** — the modern framework built on everything you just learned.