# Application Properties

## 1. Introduction

**application.properties** (or YAML) centralizes configuration: ports, datasources, feature flags, and profiles — all without touching code.

---

## 2. Why This Topic Is Important

- Config changes shouldn't need recompiles.
- Profiles (dev/prod) switch environments safely.
- `@Value` and `@ConfigurationProperties` read values cleanly.
- Secrets and environment-specific values live here.

---

## 3. Real-World Analogy

Think of a **control panel** in a cockpit: the pilot (developer) flips switches (properties) to configure the flight (app) — altitude (port), destination (database), mode (profile) — without rebuilding the plane.

---

## 4. Theory

### File Formats
| Format | File | Notes |
|--------|------|-------|
| Properties | application.properties | `key=value` |
| YAML | application.yml | Nested, profiles in one file |

### Key Properties
| Property | Purpose |
|----------|---------|
| `server.port` | HTTP port |
| `spring.datasource.url` | DB connection |
| `spring.profiles.active` | Active profile |
| `logging.level.root` | Log level |
| `management.endpoints...` | Actuator exposure |

### Reading Values
| Way | Annotation |
|-----|------------|
| Single value | `@Value("${app.name}")` |
| Grouped | `@ConfigurationProperties(prefix="app")` |

### Profiles
| File | Used When |
|------|-----------|
| application.properties | Default |
| application-dev.properties | profile=dev |
| application-prod.properties | profile=prod |

---

## 5. Syntax

```properties
server.port=9090
spring.profiles.active=dev
app.name=MyService
app.version=1.2.0
```

---

## 6. Flow Explanation

```mermaid
flowchart LR
    A[Startup] --> B[Load default properties]
    B --> C[Load active profile]
    C --> D[Bind to beans]
    D --> E[App configured]
```

---

## 7. Memory Representation

Skip — config topic.

---

## 8. Simple Example — Basic Properties

```properties
# application.properties
server.port=9090
app.name=PaymentService
app.description=Handles payments
```

```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.*;

@RestController
public class InfoController {

    @Value("${app.name}")
    private String appName;

    @Value("${app.description}")
    private String description;

    @GetMapping("/info")
    public String info() {
        return appName + " — " + description;
    }
}
```

---

## 9. Intermediate Example — Profiles

```properties
# application.properties
spring.profiles.active=dev
```

```properties
# application-dev.properties
server.port=8081
app.message=DEV environment
```

```properties
# application-prod.properties
server.port=80
app.message=PROD environment
```

```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.*;

@RestController
public class EnvController {

    @Value("${app.message}")
    private String message;

    @GetMapping("/env")
    public String env() {
        return message;
    }
}
```

---

## 10. Advanced Example — @ConfigurationProperties

```java
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Component
@ConfigurationProperties(prefix = "app")
public class AppConfig {
    private String name;
    private String version;
    private boolean featureX = false;

    // getters and setters
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    public String getVersion() { return version; }
    public void setVersion(String version) { this.version = version; }

    public boolean isFeatureX() { return featureX; }
    public void setFeatureX(boolean featureX) { this.featureX = featureX; }

    public String info() {
        return name + " v" + version + " (featureX=" + featureX + ")";
    }
}
```

```properties
app.name=BankingApi
app.version=3.1.0
app.feature-x=true
```

---

## 11. Output

For the **Simple Example** (`GET /info`):

```
PaymentService — Handles payments
```

For the **Intermediate Example** (dev):

```
DEV environment
```

For the **Advanced Example** (injected bean):

```
BankingApi v3.1.0 (featureX=true)
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| Hardcoding config in code | Move to properties |
| Secrets committed | Use env vars / secret manager |
| Missing profile files | Defaults must exist |
| Wrong prefix in @ConfigurationProperties | Match the property prefix |
| Property not found | Provide defaults `:value` |

---

## 13. Best Practices

- Use YAML for nested config; properties for flat.
- Keep secrets in env vars, not files.
- Use profiles for environments.
- Group configs with @ConfigurationProperties.
- Set defaults in @Value.

---

## 14. Interview Questions

**Q1. application.properties vs yml?**
*Both work; YAML supports nesting and multi-profile in one file.*

**Q2. How do you use profiles?**
*spring.profiles.active + application-{profile}.properties.*

**Q3. How to read a property?**
*@Value or @ConfigurationProperties.*

**Q4. How do you change the port?**
*server.port in properties (or --server.port on CLI).*

**Q5. Secrets handling?**
*Environment variables / secret managers, not committed files.*

---

## 15. Practice Exercises

1. Set a custom port and verify.
2. Read a value with @Value.
3. Create dev/prod profiles and switch.
4. Group config with @ConfigurationProperties.

---

## 16. Coding Challenges

1. **Easy:** Custom port + info endpoint.
2. **Medium:** Two profiles with distinct messages.
3. **Hard:** ConfigurationProperties bean for a feature-flag subsystem.

---

## 17. Summary

- Properties centralize config; profiles switch environments.
- @Value for singles; @ConfigurationProperties for groups.
- Secrets belong in env vars.
- Config changes without code changes.
- Defaults keep apps resilient.

➡️ Continue to: [Spring Boot Annotations](Spring-Boot-Annotations.md)