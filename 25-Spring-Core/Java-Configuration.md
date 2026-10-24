# Java Configuration

## 1. Introduction

**Java-based configuration** uses `@Configuration` classes with `@Bean` methods — the modern replacement for XML. It's type-safe, refactorable, and the default in Spring Boot.

---

## 2. Why This Topic Is Important

- Spring Boot apps are configured this way.
- @Configuration + @Bean gives full control over wiring.
- XML is legacy; interviews expect Java config fluency.
- It supports conditional, profiled, and imported config.

---

## 3. Real-World Analogy

Java config is like a **detailed recipe card** vs XML's **loose instructions**. The recipe (Java code) is compiled, checked, and refactorable — typos fail at build time, not runtime. You describe beans in code; Spring cooks them.

---

## 4. Theory

### Core Annotations
| Annotation | Purpose |
|------------|---------|
| `@Configuration` | Marks a config class (contains @Bean methods) |
| `@Bean` | Declares a bean created by the method |
| `@Import` | Pull in other config classes |
| `@Profile` | Activate beans per environment |
| `@PropertySource` | Load external properties |
| `@Enable*` | Turn on features (AOP, Caching, etc.) |
| `@ComponentScan` | Scan for stereo annotations |

### @Bean Method Rules
- Method name = bean name.
- Return type = bean type.
- Parameters are auto-wired from other beans.
- Each call typically returns a new instance, but Spring caches for singleton.

### XML vs Java
| Aspect | XML | Java Config |
|--------|-----|-------------|
| Type safety | Runtime errors | Compile-time |
| Refactoring | Fragile | Safe |
| Readability | Verbose | Concise |
| IDE support | Partial | First-class |

---

## 5. Syntax

```java
@Configuration
public class AppConfig {

    @Bean
    public DataSource dataSource() {
        return new DataSource("jdbc:h2:mem:test");
    }
}
```

---

## 6. Flow Explanation

```mermaid
flowchart LR
    A[Config class] --> B[Read @Bean methods]
    B --> C[Create instances]
    C --> D[Wire parameters]
    D --> E[Register in context]
```

---

## 7. Memory Representation

Bean definitions come from @Bean methods; singleton results are cached in the container.

---

## 8. Simple Example — Two Beans

```java
import org.springframework.context.annotation.*;

class Greeting {
    private final String message;
    Greeting(String message) { this.message = message; }
    public String text() { return message; }
}

@Configuration
class AppConfig {
    @Bean
    Greeting greeting() {
        return new Greeting("Welcome to Spring!");
    }
}

public class JavaConfigDemo {
    public static void main(String[] args) {
        org.springframework.context.annotation.AnnotationConfigApplicationContext
                ctx = new org.springframework.context.annotation.AnnotationConfigApplicationContext(AppConfig.class);

        System.out.println(ctx.getBean(Greeting.class).text());
        ctx.close();
    }
}
```

---

## 9. Intermediate Example — Wiring Beans via Parameters

```java
import org.springframework.context.annotation.*;

class NotificationService {
    public void send(String msg) { System.out.println("Sending: " + msg); }
}

class OrderService {
    private final NotificationService notifier;

    OrderService(NotificationService notifier) { this.notifier = notifier; }

    public void place() { notifier.send("Order placed"); }
}

@Configuration
class AppConfig {
    @Bean
    NotificationService notificationService() {
        return new NotificationService();
    }

    @Bean
    OrderService orderService(NotificationService notificationService) {
        return new OrderService(notificationService);   // wired
    }
}

public class WiringDemo {
    public static void main(String[] args) {
        org.springframework.context.annotation.AnnotationConfigApplicationContext
                ctx = new org.springframework.context.annotation.AnnotationConfigApplicationContext(AppConfig.class);

        ctx.getBean(OrderService.class).place();
        ctx.close();
    }
}
```

---

## 10. Advanced Example — Import & Profile

```java
import org.springframework.context.annotation.*;

class DevDataSource {
    public String url() { return "jdbc:h2:mem:dev"; }
}

class ProdDataSource {
    public String url() { return "jdbc:postgresql://db/prod"; }
}

@Configuration
class DataSourceConfig {
    @Bean
    @Profile("dev")
    DevDataSource dev() { return new DevDataSource(); }

    @Bean
    @Profile("prod")
    ProdDataSource prod() { return new ProdDataSource(); }
}

@Configuration
@Import(DataSourceConfig.class)
class AppConfig { }

public class ImportProfileDemo {
    public static void main(String[] args) {
        System.setProperty("spring.profiles.active", "dev");

        org.springframework.context.annotation.AnnotationConfigApplicationContext
                ctx = new org.springframework.context.annotation.AnnotationConfigApplicationContext(AppConfig.class);

        System.out.println("Active datasource: " + ctx.getBean(DevDataSource.class).url());
        ctx.close();
    }
}
```

---

## 11. Output

For the **Simple Example**:

```
Welcome to Spring!
```

For the **Intermediate Example**:

```
Sending: Order placed
```

For the **Advanced Example**:

```
Active datasource: jdbc:h2:mem:dev
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| Forgetting @Configuration | @Bean may not register correctly |
| Heavy logic in @Bean methods | Keep config simple |
| Mixing XML and Java unnecessarily | Pick one modern style |
| Bean methods returning new each time | Singleton caching handles it |
| Not using profiles for envs | Environment-specific beans collide |

---

## 13. Best Practices

- Prefer @Configuration + @Bean over XML.
- Keep config classes focused (data, web, security).
- Use @Import to compose configs.
- Use @Profile for environments.
- Let parameters wire dependencies automatically.

---

## 14. Interview Questions

**Q1. @Configuration vs @Component?**
*@Configuration + @Bean = explicit bean definitions; @Component = self-registered.*

**Q2. What does @Bean do?**
*Declares a bean produced by the annotated method.*

**Q3. How are beans wired?**
*Method parameters are resolved from other beans automatically.*

**Q4. What's @Import for?**
*Combines multiple config classes into one context.*

**Q5. Java config vs XML?**
*Java: type-safe, refactorable, readable. XML: legacy, verbose.*

---

## 15. Practice Exercises

1. Create a @Configuration with two @Bean methods.
2. Wire one bean into another via a method parameter.
3. Use @Import to split configs.
4. Add @Profile to switch datasources.

---

## 16. Coding Challenges

1. **Easy:** Config class with a single @Bean.
2. **Medium:** Bean wiring through parameters.
3. **Hard:** Multi-profile config with @Import and conditional beans.

---

## 17. Summary

- @Configuration + @Bean replaces XML.
- Method params auto-wire dependencies.
- @Import composes configs; @Profile selects environments.
- Java config is compile-time safe.
- Spring Boot uses this style everywhere.

➡️ Continue to: [Exercises](Exercises.md)