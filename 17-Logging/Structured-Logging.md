# Structured Logging

## 1. Introduction

**Structured logging** emits each log event as **machine-readable data** (JSON), not free text. It also attaches **context** — user ID, request ID, order ID — to every message, so you can search, filter, and correlate logs at scale.

## 2. Why This Topic Is Important

- Modern observability (Elasticsearch/Loki/CloudWatch) is built on structured, searchable logs.
- Correlating logs across microservices requires correlation IDs.
- Interviewers increasingly ask how to make logs useful beyond "a wall of text".

## 3. Real-World Analogy

Free-text logs are like a **notebook with handwritten notes** — you must read everything to find anything. Structured logs are a **spreadsheet**: each row has fixed columns you can sort and filter in an instant.

## 4. Theory

### Key ideas

- **Key–value pairs:** `"userId": 42`, `"durationMs": 231`.
- **MDC** (Mapped Diagnostic Context): thread-local key–value pairs automatically attached to every log line emitted on that thread.
- **Correlation ID**: a unique ID passed across services so one user's request can be traced end-to-end.

### JSON encoder in Logback

`net.logstash.logback.encoder.LogstashEncoder` produces JSON with timestamp, level, logger, message, and MDC fields.

## 5. Syntax

```xml
<appender name="JSON" class="ch.qos.logback.core.ConsoleAppender">
    <encoder class="net.logstash.logback.encoder.LogstashEncoder"/>
</appender>
```

```java
import org.slf4j.MDC;

MDC.put("userId", "42");
log.info("Order placed");
MDC.remove("userId");
```

## 6. Flow Explanation

1. Code puts context into the MDC (`MDC.put("userId", "42")`).
2. The thread-local MDC holds those pairs.
3. Every log event on that thread is encoded with the MDC as JSON fields.
4. `MDC.remove(...)` cleans up so the value doesn't leak to the next operation.
5. Log aggregation tools index the JSON fields for filtering.

## 7. Memory Representation

```
Thread ── MDC {userId: "42", requestId: "abc"}
    └─ log.info("Order placed")
         └─ {"time":"...","level":"INFO","logger":"...","message":"Order placed","userId":"42","requestId":"abc"}
```

## 8. Simple Example

```java
import org.slf4j.MDC;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Demo {
    private static final Logger log = LoggerFactory.getLogger(Demo.class);

    public static void main(String[] args) {
        MDC.put("env", "test");
        log.info("Bootstrapping");
        MDC.remove("env");
    }
}
```

## 9. Intermediate Example

```java
public class OrderController {
    private static final Logger log = LoggerFactory.getLogger(OrderController.class);

    public void createOrder(String userId, String orderId) {
        MDC.put("userId", userId);
        MDC.put("orderId", orderId);
        log.info("Order created");
        MDC.clear();
    }
}
```

## 10. Advanced Example

```java
// Correlation ID passed through a web filter
public class CorrelationIdFilter implements Filter {
    public void doFilter(...) {
        String id = request.getHeader("X-Request-Id");
        if (id == null) id = UUID.randomUUID().toString();
        MDC.put("requestId", id);
        try {
            chain.doFilter(request, response);
        } finally {
            MDC.remove("requestId");
        }
    }
}
```

## 11. Output

```json
{"@timestamp":"2026-08-14T11:32:07.000Z","level":"INFO",
 "logger":"com.app.OrderController","message":"Order created",
 "userId":"42","orderId":"O-123","requestId":"7f3a9c"}
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Forgetting `MDC.remove` | Values leak across requests | Use try/finally or clear |
| Logging secrets in JSON fields | Structured data leaks | Redact before logging |
| No correlation ID | Can't trace requests | Generate in filter, propagate |
| Free-text-only logs | Weak searchability | Use JSON encoder in prod |

## 13. Best Practices

- Always set a correlation/request ID at the entry point.
- Put stable fields (userId, orderId, tenant) in the MDC.
- Keep `MDC` scoped: set, log, remove in `finally`.
- Use a JSON encoder in production, human pattern in local dev.
- Never log credentials or tokens.

## 14. Interview Questions

**Q: What is the MDC and why use it?**
The MDC is a thread-local map of key–value pairs that are automatically included in every log line, giving per-request context without threading it through every method.

**Q: What is a correlation ID?**
A unique identifier assigned at the request entry point and propagated through services, letting you trace one request across distributed logs.

**Q: Why structured (JSON) logs?**
They are filterable, aggregatable, and greppable by field in modern observability tools.

## 15. Practice Exercises

1. Add MDC `userId`/`orderId` and log three events.
2. Configure the LogstashEncoder and confirm JSON output.
3. Simulate a request filter that sets and removes a `requestId`.

## 16. Coding Challenges

1. Build a small web-like flow (controller → service → repository on one thread) that carries one correlation ID through all logs.
2. Add a structured field `durationMs` measured around a method call and log it.

## 17. Summary

- Structured logs are machine-readable (JSON) with key–value context.
- The MDC attaches thread-local context to every log line.
- Correlation IDs trace requests across services.
- Scope MDC carefully to avoid leaking values.
- Logging is a core production skill — next, learn to store data in **Module 18 — SQL & Databases**.