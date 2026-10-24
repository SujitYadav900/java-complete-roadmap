# java.util.logging (JUL)

## 1. Introduction

`java.util.logging` (JUL) is the logging API built into the JDK. With **zero external dependencies**, you can add leveled, formatted, file-backed logging to any Java program out of the box.

## 2. Why This Topic Is Important

- It works everywhere — no dependencies, no configuration files required to start.
- Many older libraries use JUL, so you will see its output in real projects.
- Understanding JUL's `Logger`, `Handler`, `Formatter`, and `Level` model makes every other logging framework easy to learn.

## 3. Real-World Analogy

JUL is like the **basic toolkit your car comes with** — a jack and a wrench in the trunk. It is not the fanciest garage, but it works without buying anything extra and covers most roadside needs.

## 4. Theory

### Main components

| Component | Role |
|-----------|------|
| `Logger` | Named object you call methods on |
| `Handler` | Destination: `ConsoleHandler`, `FileHandler` |
| `Formatter` | Turns a `LogRecord` into text (`SimpleFormatter`, `XMLFormatter`) |
| `Level` | Severity: `SEVERE`, `WARNING`, `INFO`, `FINE`, `FINER`, `FINEST` |

### Level mapping

JUL level → common level:
- `SEVERE` ≈ ERROR
- `WARNING` ≈ WARN
- `INFO` ≈ INFO
- `FINE` ≈ DEBUG
- `FINER`/`FINEST` ≈ TRACE

### Global logger & logger hierarchy

`Logger.getLogger("name")` — dotted names form a hierarchy (`com.app` is parent of `com.app.service`). Levels inherit down the tree.

## 5. Syntax

```java
import java.util.logging.*;

Logger log = Logger.getLogger(Demo.class.getName());
log.info("Hello");
log.warning("Careful");
log.severe("Bad error");
```

## 6. Flow Explanation

1. Obtain a logger via `Logger.getLogger(...)`.
2. Call a leveled method (`log.info(...)`, `log.severe(...)`).
3. The logger builds a `LogRecord` and passes it to the root logger.
4. Handlers filter by level and format it with a Formatter.
5. The handler writes to its destination.

## 7. Memory Representation

```
Logger("com.app.Main", level=INFO)
   └─ Handlers: ConsoleHandler(level=INFO) -> System.err
                FileHandler(level=INFO) -> app.log
```

## 8. Simple Example

```java
import java.util.logging.Logger;

public class Demo {
    private static final Logger log = Logger.getLogger(Demo.class.getName());

    public static void main(String[] args) {
        log.info("Application started");
        log.warning("Low memory");
        log.severe("Failed to connect to database");
    }
}
```

## 9. Intermediate Example

```java
import java.util.logging.*;

public class FileLogging {
    public static void main(String[] args) throws Exception {
        FileHandler fh = new FileHandler("app.log", true);   // append
        fh.setFormatter(new SimpleFormatter());
        Logger log = Logger.getLogger("com.app");
        log.addHandler(fh);
        log.setLevel(Level.FINE);
        log.fine("Debug detail written to file");
        log.info("Info line");
    }
}
```

## 10. Advanced Example

```java
// Set the global level from a property
// java -Djava.util.logging.config.file=logging.properties MyApp
// logging.properties:
// handlers=java.util.logging.FileHandler
// java.util.logging.FileHandler.pattern=app.log
// java.util.logging.FileHandler.limit=1000000
// com.app.level=FINE
```

```java
Logger root = Logger.getLogger("");
Handler[] handlers = root.getHandlers();
for (Handler h : handlers) {
    h.setLevel(Level.FINE);
}
log.setLevel(Level.FINE);
```

## 11. Output

```
Jan 01, 2026 11:32:07 AM Demo main
INFO: Application started
WARNING: Low memory
SEVERE: Failed to connect to database
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Forgetting to set the level | Messages silently dropped | `log.setLevel(...)` + handler level |
| Level on logger only | Handler still filters | Set level on handler too |
| Default `INFO` root | FINE messages invisible | Raise the level explicitly |
| `System.out` interleaving | Messy output | Use console handler (stderr) |

## 13. Best Practices

- Use the class name as the logger name: `Demo.class.getName()`.
- Prefer `log.isLoggable(level)` before building expensive messages.
- Configure levels via `logging.properties` for production tuning.
- For new projects, consider SLF4J + Logback instead — it is the industry standard.

## 14. Interview Questions

**Q: What is the default level of the JUL root logger?**
`INFO`, so `FINE` and below are suppressed by default.

**Q: How do you write JUL output to a file?**
Add a `FileHandler` to the logger and configure its pattern/limit in `logging.properties` or in code.

**Q: JUL vs SLF4J?**
JUL is built-in but dated; SLF4J is a facade with richer backends (Logback/Log4j2), better formatting and structured logging.

## 15. Practice Exercises

1. Create a logger and emit INFO, WARNING, and SEVERE.
2. Add a `FileHandler` and verify a `.log` file is created.
3. Enable FINE output by setting levels on both logger and handler.

## 16. Coding Challenges

1. Build a tiny `java.util.logging` configuration that rotates a file every 1 MB (use `FileHandler.limit`).
2. Route different packages to different files: `com.app.api` → `api.log`, `com.app.core` → `core.log`.

## 17. Summary

- JUL is the JDK's built-in logging, usable with zero dependencies.
- Components: Logger → Handler → Formatter → destination.
- Levels: SEVERE, WARNING, INFO, FINE, FINER, FINEST.
- Default root level is INFO; configure both logger and handler levels.
- Next, adopt the industry standard in **SLF4J-and-Logback.md**.