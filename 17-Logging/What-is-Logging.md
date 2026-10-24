# What is Logging?

## 1. Introduction

Logging is the practice of recording **diagnostic events** from a running program — messages that say what happened, when, and with what context. A logging framework writes these to the console, files, or external systems, with configurable severity levels and formatting.

## 2. Why This Topic Is Important

- Production systems must be observable; logs are the primary observability source for many teams.
- Interviewers expect you to know log levels and best practices.
- `System.out.println` cannot be filtered, leveled, or routed — it is a debugging crutch, not logging.

## 3. Real-World Analogy

A **flight recorder** on an airplane continuously records every reading. When something goes wrong, investigators don't ask the pilot to remember — they read the recorder. Logs are your application's flight recorder.

## 4. Theory

### The five standard levels (SLF4J/Logback)

| Level | Purpose | Example |
|-------|---------|---------|
| `TRACE` | Ultra-detailed flow (noise) | Every loop iteration |
| `DEBUG` | Fine detail for developers | Request headers in dev |
| `INFO` | Notable lifecycle events | Server started, user logged in |
| `WARN` | Suspicious but non-fatal | Retrying a failed connection |
| `ERROR` | Something failed | Exception caught at boundary |

### Why not `System.out.println`?

- No levels or filtering.
- Goes to `stdout`, mixing with user-facing output.
- No timestamps, thread names, or structured fields by default.
- Cannot be disabled in production without code changes.

### Loggers and appenders

- A **logger** is named (usually by class) and emits messages.
- Loggers inherit level and appenders from their parent (`root`).
- An **appender** is a destination: console, file, socket, database.

## 5. Syntax

```java
log.trace("trace message");
log.debug("debug message");
log.info("info message");
log.warn("warn message");
log.error("error message", exception);
```

## 6. Flow Explanation

1. Your code calls `log.info("...")` on a logger.
2. The logger checks the message level against its effective level.
3. If enabled, the message goes through filters and formatters.
4. The appender writes it to its destination (console/file).
5. Thresholds are configured, so `DEBUG` can be on in dev and off in prod.

## 7. Memory Representation

```
YourCode -> Logger(name, level) -> Appender(destination)
              |-- inherits from parent loggers
              |-- root logger default level (e.g., INFO)
```

## 8. Simple Example

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Demo {
    private static final Logger log = LoggerFactory.getLogger(Demo.class);

    public static void main(String[] args) {
        log.info("Hello from the logger");
        log.warn("Disk space low: {}%", 12);
        log.error("Operation failed", new IllegalStateException("demo"));
    }
}
```

## 9. Intermediate Example

```java
public class OrderService {
    private static final Logger log = LoggerFactory.getLogger(OrderService.class);

    public void placeOrder(String orderId) {
        log.info("Placing order {}", orderId);
        try {
            // business logic
            log.debug("Calling payment gateway for order {}", orderId);
        } catch (Exception e) {
            log.error("Payment failed for order {}", orderId, e);
        }
    }
}
```

## 10. Advanced Example

```java
// Guarding expensive argument construction
if (log.isDebugEnabled()) {
    log.debug("Payload: {}", serialize(orders));   // serialize() only runs when debug is on
}
```

## 11. Output

```
11:32:07.142 [main] INFO  Demo - Hello from the logger
11:32:07.148 [main] WARN  Demo - Disk space low: 12%
11:32:07.148 [main] ERROR Demo - Operation failed
java.lang.IllegalStateException: demo
	at Demo.main(Demo.java:12)
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Logging sensitive data (passwords, tokens) | Data leaks | Redact or exclude |
| `System.out.println` for logging | No levels/filtering | Use a logger |
| Logging stack traces at WARN | Noise | Use ERROR for exceptions |
| Concatenating strings instead of `{}` placeholders | Unnecessary cost | Use parameterized messages |

## 13. Best Practices

- Use `{}` placeholders instead of string concatenation.
- Log at the correct level; don't over-log at INFO.
- Log exceptions with the exception object as the last argument.
- Never log passwords, tokens, or full credit-card numbers.
- Include context: order IDs, user IDs, request IDs.

## 14. Interview Questions

**Q: What are the standard log levels and when do you use them?**
TRACE (detailed flow), DEBUG (developer detail), INFO (lifecycle), WARN (suspicious), ERROR (failures). Use the highest level that captures what matters, typically INFO in production.

**Q: Why is logging better than `System.out.println`?**
Levels, filtering, destinations, formatting, structured output, and no mixing with program output.

**Q: How do you avoid logging sensitive data?**
Never log secrets; mask/redact fields like passwords, tokens, and card numbers; configure filters.

## 15. Practice Exercises

1. Add an SLF4J logger to a class and emit all five levels.
2. Change the message to use `{}` placeholders with two arguments.
3. Log an exception with its stack trace at ERROR.

## 16. Coding Challenges

1. Build a small `OrderService` that logs the full flow: INFO on start, DEBUG per step, ERROR on failure — all parameterized.
2. Redact a field: log a user object but ensure the `password` field never appears in output.

## 17. Summary

- Logging records leveled, filterable, routable diagnostic events.
- Levels: TRACE, DEBUG, INFO, WARN, ERROR.
- Use parameterized messages and never log secrets.
- `println` is not logging — use a framework.
- Next, explore the built-in **java-util-Logging.md**.