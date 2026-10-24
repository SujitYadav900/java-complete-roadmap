# Exercises — REST API

## 1. Introduction

Practice the full REST stack: design, JSON, validation, error handling, consumption, and documentation.

---

## 2. Why This Topic Is Important

- REST is how every system integrates.
- These skills are core to backend roles.
- Design + error handling are senior markers.

---

## 3. Real-World Analogy

Like **learning to write clear letters**: REST design is the format, JSON is the envelope, validation/errors are checking the address before sending. Practicing the whole flow makes you fluent.

---

## 4. Theory — Topic Map

| Topic | Key Skill |
|-------|-----------|
| Basics | Verbs + status codes |
| Design | Resources + versioning |
| JSON | Jackson mapping |
| Validation | @Valid + annotations |
| Errors | Global advice |
| Consumption | RestClient |
| Swagger | OpenAPI docs |

---

## 5. Syntax Reminder

```java
@PostMapping
public Task create(@Valid @RequestBody TaskRequest r) { ... }
```

```java
RestClient client = RestClient.builder().baseUrl("...").build();
client.get().uri("/x").retrieve().body(Task.class);
```

---

## 6. Practice Flow

```mermaid
flowchart LR
    A[Design API] --> B[Implement]
    B --> C[Validate]
    C --> D[Error handling]
    D --> E[Document]
    E --> F[Consume]
```

---

## 7. Memory Representation

Skip — practice module.

---

## 8. Simple Exercises

**Ex 1 — Design:** Plan `/api/v1/tasks` resources and verbs.

**Ex 2 — CRUD:** Implement task CRUD with proper status codes.

**Ex 3 — JSON:** Map a Task DTO with Jackson.

**Ex 4 — Params:** Query params for list filtering.

---

## 9. Intermediate Exercises

**Ex 5 — Validation:** @NotBlank/@Size on a TaskRequest.

**Ex 6 — Errors:** Global advice returning consistent 400/404 bodies.

**Ex 7 — Pagination:** page/size list endpoint.

**Ex 8 — Swagger:** Add springdoc; document operations.

**Ex 9 — Consume:** RestClient calling your own API.

---

## 10. Advanced Exercises

**Ex 10 — ProblemDetail:** RFC 7807 errors with custom properties.

**Ex 11 — Nested:** `/api/v1/users/{id}/tasks` nested resources.

**Ex 12 — Versioned:** v1 and v2 endpoints coexisting.

**Ex 13 — Full Service:** A validated, documented, error-consistent task API.

---

## 11. Expected Output Samples

**Ex 5 (blank title):**
```
400
{"error":"validation_error","message":"title: title is required","status":400}
```

**Ex 6 (missing task):**
```
404
{"error":"not_found","message":"Task 99 not found","status":404}
```

---

## 12. Common Mistakes

| Mistake | Fix |
|---------|-----|
| Verbs in URLs | Nouns + verbs |
| Missing @Valid | Validation skipped |
| Inconsistent errors | Global advice |
| No pagination | Huge lists |
| No docs | Add springdoc |

---

## 13. Best Practices

- Design before coding.
- Correct status codes everywhere.
- Validate all inputs; handle errors globally.
- Document with OpenAPI.
- Consume with RestClient when needed.

---

## 14. Interview Practice Questions

**Q1.** What are the REST constraints?
**Q2.** How do you version an API?
**Q3.** How does Jackson map JSON?
**Q4.** How do you validate a request?
**Q5.** RestClient vs WebClient?

---

## 15. Self-Check

- [ ] I can design RESTful resources.
- [ ] I can build validated endpoints.
- [ ] I return consistent errors.
- [ ] I can consume APIs.
- [ ] I document with Swagger.

---

## 16. Next Steps

- Secure APIs in [29 — Spring Security](../29-Spring-Security/README.md).
- Add persistence in [30 — Hibernate/JPA](../30-Hibernate-JPA/README.md).

---

## 17. Summary

- REST design: nouns, verbs, versioning, pagination.
- Validate input; consistent global errors.
- Jackson maps JSON automatically.
- Document with OpenAPI/Swagger.
- Consume with RestClient.
- These skills form a production-grade API.

➡️ Module complete! Continue to [29 — Spring Security](../29-Spring-Security/README.md).