# Logging Exercises

> Practice the logging concepts from Module 17. Add these to a Maven project with `logback-classic`.

## 1. Beginner Exercises

1. Create a class with a `private static final` SLF4J logger.
2. Log a message at each of the five levels.
3. Run it and confirm output appears on the console.
4. Log a message with one `{}` placeholder and an integer argument.

## 2. Level Exercises

1. Add a `logback.xml` that sets root to WARN and observe which messages disappear.
2. Set a specific logger to DEBUG and confirm only it prints DEBUG lines.
3. Restore root to INFO and confirm expected output.

## 3. File Logging Exercises

1. Configure a `FileAppender` writing to `logs/app.log`.
2. Run the app several times and confirm append behavior.
3. Add a rolling policy that rotates by date with `maxHistory` of 7.

## 4. MDC Exercises

1. Put `userId` and `orderId` in the MDC and log three events.
2. Remove them afterward and confirm they no longer appear.
3. Use the `LogstashEncoder` to produce JSON output with the MDC fields.

## 5. Exception Exercises

1. Catch an exception and log it at ERROR as the last argument.
2. Log a WARN with context but without the stack trace.
3. Guard an expensive debug call with `log.isDebugEnabled()`.

## 6. Challenge: Correlation ID

1. Write a helper that generates a `UUID` when no `X-Request-Id` header exists.
2. Set it in the MDC, log a full "request start → processing → response" flow.
3. Ensure it is removed in a `finally` block.

## 7. Challenge: Redact secrets

1. Create a `User` object with a `password` field.
2. Log the user at DEBUG without ever printing the password (build a safe-to-string).
3. Add a warning-level log that reports a failed login with the username only.

> **You've mastered logging.** Move to **[18 — SQL & Databases](../18-SQL-and-Databases/README.md)** — the language of data.