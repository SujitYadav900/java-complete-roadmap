# AOP (Aspect-Oriented Programming)

## 1. Introduction

**AOP** lets you add cross-cutting behavior (logging, security, transactions) to methods without modifying them. It's the magic behind Spring `@Transactional` and `@PreAuthorize`.

---

## 2. Why This Topic Is Important

- Logging, transactions, and auditing become declarative.
- It removes duplicated boilerplate across methods.
- Interviewers ask "how does @Transactional work?"
- Understanding AOP demystifies framework annotations.

---

## 3. Real-World Analogy

A **security guard at a bank**:
- Instead of every employee checking badges, a guard at the door checks everyone automatically.
- The guard is an **aspect** — cross-cutting logic applied uniformly.
- Employees (business methods) don't change; the guard intercepts on the way in/out.

---

## 4. Theory

### Core Terms
| Term | Meaning |
|------|---------|
| **Aspect** | Module of cross-cutting logic (e.g., LoggingAspect) |
| **Join point** | A point in execution (method call) |
| **Advice** | What the aspect does at a join point |
| **Pointcut** | Expression selecting join points |
| **Target** | The object being advised |

### Advice Types
| Advice | Runs |
|--------|------|
| `@Before` | Before the method |
| `@After` | After (success or failure) |
| `@AfterReturning` | After success |
| `@AfterThrowing` | After exception |
| `@Around` | Wraps the whole call (most powerful) |

### Why AOP
| Concern | Without AOP | With AOP |
|---------|-------------|----------|
| Logging | Every method | One aspect |
| Transactions | Manual begin/commit | @Transactional |
| Security | Manual checks | @PreAuthorize |

---

## 5. Syntax

```java
@Aspect
@Component
class LoggingAspect {
    @Around("@annotation(com.example.Timed)")
    public Object time(ProceedingJoinPoint pjp) throws Throwable {
        long start = System.nanoTime();
        Object result = pjp.proceed();
        System.out.println(pjp.getSignature().getName()
                + " took " + (System.nanoTime() - start) / 1_000_000 + "ms");
        return result;
    }
}
```

---

## 6. Flow Explanation

```mermaid
flowchart LR
    A[Call method] --> B[Aspect @Around]
    B --> C[before logic]
    C --> D[proceed -> real method]
    D --> E[after logic]
    E --> F[Return to caller]
```

---

## 7. Memory Representation

Spring creates a **proxy** of the target bean; calls pass through the proxy where advice runs.

---

## 8. Simple Example — @Before Logging

```java
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.*;

@Aspect
@Component
class LoggingAspect {
    @Before("execution(* com.example.Service.*(..))")
    public void logCall(JoinPoint jp) {
        System.out.println(">> Calling " + jp.getSignature().getName());
    }
}

@Service
class Service {
    public void doWork() { System.out.println("Working..."); }
}
```

---

## 9. Intermediate Example — Around with Timing

```java
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;

@Aspect
@Component
class TimingAspect {
    @Around("execution(* com.example.*Service.*(..))")
    public Object time(ProceedingJoinPoint pjp) throws Throwable {
        long start = System.nanoTime();
        Object result = pjp.proceed();
        long ms = (System.nanoTime() - start) / 1_000_000;
        System.out.println(pjp.getSignature().getName() + " took " + ms + " ms");
        return result;
    }
}

@Service
class OrderService {
    public void placeOrder() throws InterruptedException {
        Thread.sleep(50);          // simulate work
        System.out.println("Order placed");
    }
}
```

---

## 10. Advanced Example — Custom @Timed Annotation

```java
import java.lang.annotation.*;

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@interface Timed { }

@Aspect
@Component
class TimedAspect {
    @Around("@annotation(Timed)")
    public Object timed(ProceedingJoinPoint pjp) throws Throwable {
        long start = System.nanoTime();
        Object result = pjp.proceed();
        long ms = (System.nanoTime() - start) / 1_000_000;
        System.out.println("[" + pjp.getSignature().getName() + "] " + ms + " ms");
        return result;
    }
}

@Service
class ReportService {
    @Timed
    public void generate() throws InterruptedException {
        Thread.sleep(30);
        System.out.println("Report generated");
    }

    public void quick() { System.out.println("Quick task"); }
}
```

---

## 11. Output

For the **Simple Example**:

```
>> Calling doWork
Working...
```

For the **Intermediate Example**:

```
Order placed
placeOrder took 50 ms
```

For the **Advanced Example**:

```
[generate] 30 ms
Report generated
Quick task
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| AOP not active | Add @EnableAspectJAutoProxy |
| Self-invocation bypasses proxy | Call via a different bean |
| Pointcut too broad | Narrow with annotations/params |
| @Around without proceed() | The target never runs |
| Forgetting AspectJ dependency | spring-boot-starter-aop |

---

## 13. Best Practices

- Keep aspects focused (one concern each).
- Prefer @Around only when needed.
- Use annotation-based pointcuts for control.
- Avoid self-invocation (proxy caveat).
- Rely on Spring Boot's auto-config for AOP.

---

## 14. Interview Questions

**Q1. What is AOP?**
*Cross-cutting concerns applied without touching business code.*

**Q2. What is a pointcut?**
*An expression selecting which method calls get advice.*

**Q3. Types of advice?**
*Before, After, AfterReturning, AfterThrowing, Around.*

**Q4. How does @Transactional work?**
*AOP proxy starts/commits/rolls back the transaction around the method.*

**Q5. Proxy self-invocation issue?**
*Internal calls skip the proxy — inject the bean or split classes.*

---

## 15. Practice Exercises

1. Add a @Before logging aspect.
2. Time methods with @Around.
3. Create a custom @Timed annotation.
4. Log exceptions with @AfterThrowing.

---

## 16. Coding Challenges

1. **Easy:** Aspect logging all service calls.
2. **Medium:** Timing aspect with ms output.
3. **Hard:** Custom annotation + aspect that also captures method arguments.

---

## 17. Summary

- AOP = cross-cutting concerns via proxies.
- Advice types: before/after/around/throwing.
- Pointcuts select where advice applies.
- @Transactional and @PreAuthorize use AOP.
- Avoid self-invocation; enable AspectJ auto-proxy.

➡️ Continue to: [Annotations](Annotations.md)