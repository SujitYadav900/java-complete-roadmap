# Logback Configuration

## 1. Introduction

Logback's real power is its declarative `logback.xml` configuration: you define **appenders** (destinations), **patterns** (format), **levels** per package, and **rolling policies** for files — all without touching code.

## 2. Why This Topic Is Important

- Production systems need file logging with rotation, per-package verbosity, and clean formats.
- Being able to read and tweak `logback.xml` is an everyday, job-level skill.
- Misconfiguration is a common cause of "logs missing" or "disk full" incidents.

## 3. Real-World Analogy

`logback.xml` is like the **control panel of a recording studio**: you decide which microphones (appenders) are active, how loud each source (logger) can be, and whether recordings go to tape, disk, or a master archive (rolling files).

## 4. Theory

### Core elements

| Element | Purpose |
|---------|---------|
| `<configuration>` | Root of the file |
| `<appender>` | A destination (console, file, rolling file) |
| `<encoder>`/`<pattern>` | Format of each log line |
| `<logger>` | Set level/appender for a package or class |
| `<root>` | The top-level logger (level + appenders) |

### Appenders covered

- **ConsoleAppender** — writes to stdout/stderr.
- **FileAppender** — writes to a single file.
- **RollingFileAppender** — rotates files by size or date, keeping a fixed number.

### The default pattern

```
%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n
```

- `%d` timestamp, `%thread` thread name, `%-5level` level padded, `%logger{36}` abbreviated logger name, `%msg%n` message + newline.

## 5. Syntax

```xml
<configuration>
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <root level="INFO">
        <appender-ref ref="CONSOLE"/>
    </root>
</configuration>
```

## 6. Flow Explanation

1. Logback reads `logback.xml` from the classpath at startup.
2. Appenders are created from `<appender>` declarations.
3. The `<root>` logger receives all messages not matched by a child logger.
4. A `<logger name="com.app" level="DEBUG">` overrides the level for that package and below.
5. For each log event, Logback finds matching appenders and writes formatted lines.

## 7. Memory Representation

```
root (INFO) ────────────────┐
   └─ appender CONSOLE ─────┤ Console
com.app (DEBUG) ────────────┤
   └─ appender FILE ────────┤ app.log
```

Events at `com.app` go to both console and file; other packages go to console only.

## 8. Simple Example

```xml
<configuration>
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} %-5level %logger{20} - %msg%n</pattern>
        </encoder>
    </appender>
    <root level="INFO">
        <appender-ref ref="CONSOLE"/>
    </root>
</configuration>
```

## 9. Intermediate Example

```xml
<configuration>
    <appender name="FILE" class="ch.qos.logback.core.FileAppender">
        <file>logs/app.log</file>
        <append>true</append>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <logger name="com.example.dao" level="DEBUG"/>
    <logger name="org.hibernate.SQL" level="WARN"/>

    <root level="INFO">
        <appender-ref ref="FILE"/>
    </root>
</configuration>
```

## 10. Advanced Example

```xml
<configuration>
    <appender name="ROLLING" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>logs/app.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>logs/app.%d{yyyy-MM-dd}.log</fileNamePattern>
            <maxHistory>14</maxHistory>
        </rollingPolicy>
        <encoder>
            <pattern>%d{ISO8601} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <root level="INFO">
        <appender-ref ref="ROLLING"/>
    </root>
</configuration>
```

## 11. Output

```
2026-08-14 11:32:07 [main] INFO  com.app.Main - Application started
2026-08-14 11:32:08 [main] DEBUG com.example.dao.UserDao - SELECT ... returned 3 rows
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| File path with no parent dir | Startup error | Create the dir or use `${user.dir}/logs` |
| Not closing `</configuration>` | Silent fallback to defaults | Validate XML |
| Setting level too low (TRACE) | Disk full / noise | Default INFO, DEBUG for dev |
| No rolling policy | Unlimited file growth | Use TimeBased/SizeBased rolling |
| Root level ERROR hides INFO | Missing "server started" | Keep root at INFO |

## 13. Best Practices

- Put `logback.xml` on the classpath under `src/main/resources`.
- Use rolling files with `maxHistory` in production.
- Set library loggers (Hibernate, Spring) to WARN to reduce noise.
- Keep the console appender for dev, file/rolling for prod.
- Use environment-specific configs (`logback-spring.xml` in Spring Boot) to toggle profiles.

## 14. Interview Questions

**Q: What is a rolling file policy?**
It rotates the active log file (by date, size, or both) so logs don't grow forever; `maxHistory` keeps a bounded set of old files.

**Q: How do you increase logging for just one package?**
Add `<logger name="com.example.dao" level="DEBUG"/>`.

**Q: What does the pattern `%d [%thread] %-5level %logger{36} - %msg%n` produce?**
Timestamp, thread name, padded level, abbreviated logger name, then the message.

## 15. Practice Exercises

1. Write a console-only `logback.xml` and confirm the pattern format.
2. Add a file appender and verify `logs/app.log` is created.
3. Set `com.example.dao` to DEBUG and confirm only that package prints DEBUG lines.

## 16. Coding Challenges

1. Configure daily rolling with 7-day history and verify old files rotate.
2. Build separate appenders for ERROR-only to `errors.log` and everything to `app.log`, with different patterns.

## 17. Summary

- `logback.xml` declares appenders, patterns, levels, and rolling policies.
- RollingFileAppender + TimeBasedRollingPolicy keeps logs bounded.
- Package-level `<logger>` overrides control verbosity.
- Root level defaults to INFO; keep prod noise low.
- Next, add structure and context to logs in **Structured-Logging.md**.