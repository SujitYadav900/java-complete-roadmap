# AssertJ

## 1. Introduction

**AssertJ** gives fluent, readable assertions: `assertThat(x).isEqualTo(y).contains(z).isNotNull()`. It's bundled with Spring Boot's test starter.

---

## 2. Why This Topic Is Important

- Readable tests are maintainable tests.
- Fluent chaining expresses complex checks clearly.
- Failure messages are descriptive.
- It's the standard in Spring testing.

---

## 3. Real-World Analogy

JUnit assertions are like **simple yes/no gauges**. AssertJ is a **digital inspection report**: it not only says pass/fail but describes exactly what was inspected — "string contains 'abc'", "list has size 3" — in natural language.

---

## 4. Theory

### Core Idea
```java
assertThat(actual).method(expected);
```

### Common Assertions
| Assertion | Meaning |
|-----------|---------|
| `.isEqualTo(x)` | Equality |
| `.isNotNull()` / `.isNull()` | Null checks |
| `.isTrue()` / `.isFalse()` | Booleans |
| `.contains(x)` | String/list contains |
| `.hasSize(n)` | Collection size |
| `.startsWith()` / `.endsWith()` | String bounds |
| `.isPositive()` / `.isNegative()` | Numeric sign |
| `.isGreaterThan()` / `.isLessThan()` | Ordering |
| `.isInstanceOf(Class)` | Type check |
| `.extracting("field")` | Field projection |

### Exception Assertions
```java
assertThatThrownBy(() -> code)
    .isInstanceOf(...)
    .hasMessage("...");
```

---

## 5. Syntax

```java
import static org.assertj.core.api.Assertions.assertThat;

assertThat(actual).isEqualTo(expected);
assertThat(list).hasSize(3).contains("a", "b");
```

---

## 6. Flow Explanation

```mermaid
flowchart LR
    A[assertThat(actual)] --> B[Chain checks]
    B --> C[Pass/Fail with message]
```

---

## 7. Memory Representation

Skip — assertion library.

---

## 8. Simple Example — Strings & Numbers

```java
import static org.assertj.core.api.Assertions.assertThat;

class AssertJBasicsTest {

    @org.junit.jupiter.api.Test
    void stringAssertions() {
        String s = "Hello, Java";
        assertThat(s)
                .startsWith("Hello")
                .endsWith("Java")
                .contains("lo, J")
                .isNotNull();
    }

    @org.junit.jupiter.api.Test
    void numericAssertions() {
        assertThat(42)
                .isPositive()
                .isGreaterThan(10)
                .isLessThan(100);
    }
}
```

---

## 9. Intermediate Example — Collections

```java
import org.junit.jupiter.api.Test;
import java.util.List;
import static org.assertj.core.api.Assertions.assertThat;

class ListTest {

    @Test
    void collectionAssertions() {
        List<String> names = List.of("Ajay", "Priya", "Ravi");

        assertThat(names)
                .hasSize(3)
                .contains("Ajay", "Ravi")
                .doesNotContain("Bob")
                .containsExactly("Ajay", "Priya", "Ravi");
    }

    @Test
    void extractingFields() {
        record User(String name, int age) { }
        List<User> users = List.of(
                new User("Ajay", 25),
                new User("Priya", 30));

        assertThat(users)
                .extracting(User::name)
                .contains("Ajay", "Priya");

        assertThat(users)
                .extracting(User::age)
                .allMatch(age -> age >= 25);
    }
}
```

---

## 10. Advanced Example — Exceptions & Maps

```java
import org.junit.jupiter.api.Test;
import java.util.Map;
import static org.assertj.core.api.Assertions.*;

class AdvancedAssertJTest {

    @Test
    void exceptionAssertions() {
        assertThatThrownBy(() -> {
            throw new IllegalArgumentException("bad id");
        })
                .isInstanceOf(IllegalArgumentException.class)
                .hasMessage("bad id")
                .hasMessageContaining("bad");
    }

    @Test
    void mapAssertions() {
        Map<String, Integer> scores = Map.of(
                "java", 90, "dsa", 85);

        assertThat(scores)
                .containsKey("java")
                .containsValue(85)
                .hasSize(2);
    }
}
```

---

## 11. Output

For the **Simple Example**:

```
Tests run: 2, Failures: 0
```

For a failing case, AssertJ reports:

```
Expecting:
 <"Hello, World">
to start with:
 <"Hello, Java">
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| Forgetting the static import | assertThat unresolved |
| Mixing JUnit + AssertJ assertEquals | Pick one style |
| Over-chaining beyond clarity | Split when needed |
| Asserting on wrong actual | Assert the behavior result |
| Not using descriptive methods | hasSize/contains for lists |

---

## 13. Best Practices

- Use AssertJ for readable assertions.
- Chain related checks.
- Use extracting for field checks.
- assertThatThrownBy for exceptions.
- Fail messages guide fixing.

---

## 14. Interview Questions

**Q1. What is AssertJ?**
*A fluent assertion library with readable failures.*

**Q2. AssertJ vs JUnit assertions?*
*JUnit assertEquals is basic; AssertJ is fluent/descriptive.*

**Q3. How do you test collections?*
*hasSize, contains, containsExactly.*

**Q4. How to test exceptions fluently?*
*assertThatThrownBy(...).isInstanceOf(...).hasMessage(...).*

**Q5. What does extracting do?*
*Projects a field/method for collective assertions.*

---

## 15. Practice Exercises

1. Assert on a string chain.
2. Assert collection contents and size.
3. Use extracting on records.
4. Test an exception with AssertJ.

---

## 16. Coding Challenges

1. **Easy:** String + number assertions.
2. **Medium:** Collection ordering/contents.
3. **Hard:** Complex DTO assertions with nested fields.

---

## 17. Summary

- AssertJ = fluent, readable assertions.
- Chained checks with descriptive failures.
- Collections: hasSize/contains/extracting.
- Exceptions: assertThatThrownBy.
- Default in Spring Boot testing.

➡️ Continue to: [Mockito](Mockito.md)