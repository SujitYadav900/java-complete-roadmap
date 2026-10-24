# Exercises — Testing

## 1. Introduction

Practice the full testing stack: JUnit, AssertJ, Mockito, Spring slices, and a TDD workflow.

---

## 2. Why This Topic Is Important

- Tests are part of every production change.
- A strong suite is a professional marker.
- TDD is a discipline interviewers respect.

---

## 3. Real-World Analogy

Like **learning to proofread**: you check each sentence (unit tests), the whole paragraph (integration), and the full article (e2e). Practicing all levels makes your writing reliable — and your code trustworthy.

---

## 4. Theory — Topic Map

| Topic | Key Skill |
|-------|-----------|
| Basics | Pyramid, isolation |
| JUnit5 | Lifecycle + assertions |
| AssertJ | Fluent assertions |
| Mockito | Stub + verify |
| Boot testing | Slices, MockMvc |
| TDD | Red → Green → Refactor |

---

## 5. Syntax Reminder

```java
@Test
void method_condition_result() {
    when(mock.method()).thenReturn(value);
    assertThat(service.call()).isEqualTo(expected);
    verify(mock).method();
}
```

---

## 6. Practice Flow

```mermaid
flowchart LR
    A[Write test first] --> B[Run — RED]
    B --> C[Implement — GREEN]
    C --> D[Refactor]
    D --> E[Run all tests]
```

---

## 7. Memory Representation

Skip — practice module.

---

## 8. Simple Exercises

**Ex 1 — JUnit:** Test a Calculator (add, divide, divide-by-zero).

**Ex 2 — AssertJ:** Chained string + collection assertions.

**Ex 3 — Lifecycle:** @BeforeEach/@AfterEach ordering.

**Ex 4 — Parametrized:** @CsvSource for calculator cases.

---

## 9. Intermediate Exercises

**Ex 5 — Mockito:** Mock a repository in a service test.

**Ex 6 — Verify:** Assert call counts and arguments.

**Ex 7 — Exception:** doThrow for a payment failure.

**Ex 8 — @WebMvcTest:** Controller test with MockMvc + @MockBean.

**Ex 9 — @DataJpaTest:** Repository test with rollback.

---

## 10. Advanced Exercises

**Ex 10 — Full context:** @SpringBootTest + WebTestClient.

**Ex 11 — jsonPath:** Assert nested JSON in responses.

**Ex 12 — TDD:** Build a Slugify util test-first.

**Ex 13 — TDD service:** Discount service with test-first features.

**Ex 14 — Full project:** A task API with unit + slice + integration tests, developed via TDD.

---

## 11. Expected Output Samples

**Ex 8:**
```
greet_returnsMessage → passed
```

**Ex 11:**
```
$.name isEqualTo "Laptop" → passed
```

**Ex 13 (RED first):**
```
Total run: 1, Failures: 1   (then GREEN after implementation)
```

---

## 12. Common Mistakes

| Mistake | Fix |
|---------|-----|
| Testing everything with @SpringBootTest | Use slices |
| Real DB/network in unit tests | Mock dependencies |
| Big tests covering many behaviors | One behavior per test |
| Writing code before tests | TDD: test first |
| Not running tests in CI | Automate |

---

## 13. Best Practices

- Follow the pyramid.
- Behavior-focused, readable assertions.
- Mock dependencies; slice contexts.
- Red → Green → Refactor.
- Keep suites fast and deterministic.

---

## 14. Interview Practice Questions

**Q1.** What is the test pyramid?
**Q2.** JUnit lifecycle hooks?
**Q3.** @MockBean vs @Mock?
**Q4.** When use @WebMvcTest vs @SpringBootTest?
**Q5.** Walk through a TDD cycle.

---

## 15. Self-Check

- [ ] I can write JUnit tests.
- [ ] I use AssertJ fluently.
- [ ] I mock and verify with Mockito.
- [ ] I test Spring layers with slices.
- [ ] I practice TDD.

---

## 16. Next Steps

- Package everything with [35 — Docker](../35-Docker/README.md).
- Build real apps in [36 — Mini Projects](../36-Mini-Projects/README.md).

---

## 17. Summary

- JUnit + AssertJ + Mockito = unit-testing stack.
- Spring slices test layers fast.
- TDD: test first, minimal code, refactor.
- Pyramid: many unit, few integration.
- A solid suite is non-negotiable.

➡️ Module complete! Continue to [35 — Docker](../35-Docker/README.md).