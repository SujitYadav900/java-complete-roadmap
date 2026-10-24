# Spring Boot Annotations

## 1. Introduction

Boot's annotations glue configuration, REST, and features together. The most important is **@SpringBootApplication** — a single annotation that bootstraps an entire app.

---

## 2. Why This Topic Is Important

- @SpringBootApplication appears in every Boot project.
- Annotations enable declarative configuration.
- Understanding them explains how Boot "just works."
- Interviewers probe annotation combinations.

---

## 3. Real-World Analogy

@SpringBootApplication is like a **one-button ignition switch** for a car: pressing it (running main) starts the engine (embedded Tomcat), checks all systems (auto-configuration), and picks up every passenger (component scanning) — all from a single press.

---

## 4. Theory

### @SpringBootApplication = 3-in-1
| Annotation | Role |
|------------|------|
| `@Configuration` | Marks the class as a config source |
| `@EnableAutoConfiguration` | Auto-configure from classpath |
| `@ComponentScan` | Scan the package subtree for beans |

### Other Key Annotations
| Annotation | Purpose |
|------------|---------|
| `@RestController` / `@Controller` | Web layer |
| `@Service` | Business logic |
| `@Repository` | Data access |
| `@Component` | Generic bean |
| `@ConfigurationProperties` | Bind config groups |
| `@EnableCaching` | Turn on caching |
| `@EnableScheduling` | Turn on scheduled tasks |
| `@EnableAsync` | Async processing |
| `@ConditionalOnProperty` | Bean only if property set |

### Bean Scope & Wiring (Boot)
| Annotation | Purpose |
|------------|---------|
| `@Autowired` | Inject (optional with single ctor) |
| `@Qualifier` | Pick a bean among many |
| `@Scope` | singleton/prototype/request |
| `@Value` | Inject property |

---

## 5. Syntax

```java
@SpringBootApplication
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}
```

---

## 6. Flow Explanation

```mermaid
flowchart LR
    A[main] --> B[Auto-configuration]
    B --> C[Component scan]
    C --> D[Bean registry]
    D --> E[Embedded server]
    E --> F[Ready]
```

---

## 7. Memory Representation

Skip — annotation explanation.

---

## 8. Simple Example — The 3-in-1

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.*;

@SpringBootApplication
@RestController
public class App {

    @GetMapping("/ping")
    public String ping() {
        return "pong";
    }

    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}
```

`@SpringBootApplication` covers config, auto-config, and scanning all at once.

---

## 9. Intermediate Example — Enable Features

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.*;
import org.springframework.web.bind.annotation.*;

@SpringBootApplication
@EnableScheduling
@RestController
public class SchedulerApp {

    @GetMapping("/schedule")
    public String info() {
        return "Scheduling enabled";
    }

    @Scheduled(fixedDelay = 10000)
    public void job() {
        System.out.println("Scheduled job ran at " + System.currentTimeMillis());
    }

    public static void main(String[] args) {
        SpringApplication.run(SchedulerApp.class, args);
    }
}
```

---

## 10. Advanced Example — Conditional Configuration

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.condition.ConditionalOnProperty;
import org.springframework.context.annotation.*;
import org.springframework.web.bind.annotation.*;

class FeatureService {
    public String run() { return "Feature active"; }
}

@Configuration
class FeatureConfig {
    @Bean
    @ConditionalOnProperty(name = "app.feature-x", havingValue = "true")
    FeatureService featureService() {
        return new FeatureService();
    }
}

@SpringBootApplication
@RestController
public class ConditionalApp {

    @GetMapping("/feature")
    public String feature() {
        try {
            return applicationContext.getBean(FeatureService.class).run();
        } catch (Exception e) {
            return "Feature disabled";
        }
    }

    static org.springframework.context.ApplicationContext applicationContext;

    public static void main(String[] args) {
        applicationContext = SpringApplication.run(ConditionalApp.class, args);
    }
}
```

```properties
app.feature-x=true
```

---

## 11. Output

For the **Simple Example** (`GET /ping`):

```
pong
```

For the **Intermediate Example**:

```
GET /schedule → Scheduling enabled
Scheduled job ran at 1720000000000   (every 10s)
```

For the **Advanced Example** (`GET /feature`):

```
Feature active
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| @SpringBootApplication not at root | Beans get missed |
| Forgetting @EnableX for features | Scheduled/async never runs |
| Conditional property typo | Match exact property + value |
| Multiple main classes | Keep one entry point |
| Static context access | Prefer injection |

---

## 13. Best Practices

- Root package placement for scanning.
- Use @EnableScheduling/@EnableCaching explicitly.
- Conditions for feature flags.
- Prefer injection over static access.
- Know which annotations compose Boot behavior.

---

## 14. Interview Questions

**Q1. What is @SpringBootApplication?**
*@Configuration + @EnableAutoConfiguration + @ComponentScan.*

**Q2. What does @EnableAutoConfiguration do?**
*Configures beans from dependencies on the classpath.*

**Q3. What does component scanning do?**
*Finds @Component/@Service/@Repository beans in the package tree.*

**Q4. @ConditionalOnProperty?**
*Creates a bean only when a property matches.*

**Q5. Where must the main class be?**
*The root package so scanning covers all beans.*

---

## 15. Practice Exercises

1. Examine @SpringBootApplication's three parts.
2. Add @EnableScheduling with a @Scheduled job.
3. Create a conditional bean via a property.
4. Move main class to a sub-package and observe scan failure.

---

## 16. Coding Challenges

1. **Easy:** App with ping endpoint.
2. **Medium:** Scheduled job every 5 seconds.
3. **Hard:** Feature-flagged service with @ConditionalOnProperty.

---

## 17. Summary

- @SpringBootApplication = config + auto-config + scan.
- @Enable* annotations turn on features.
- Conditions gate beans by properties.
- Root package placement is critical.
- Annotations make Boot declarative.

➡️ Continue to: [Actuator](Actuator.md)