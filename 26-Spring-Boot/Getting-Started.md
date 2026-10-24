# Getting Started

## 1. Introduction

Spin up a working Spring Boot project in minutes using **Spring Initializr**, then run your first endpoint. This is the fastest path from zero to running app.

---

## 2. Why This Topic Is Important

- Every Spring Boot job starts with scaffolding.
- Spring Initializr is the industry-standard generator.
- The wrapper (`mvnw`) makes builds reproducible.
- Setup skills are used every single day.

---

## 3. Real-World Analogy

Spring Initializr is like **ordering a pre-built house kit**: you pick the floor plan (web? data? security?), and it arrives with everything assembled. You just move in and decorate (add your code).

---

## 4. Theory

### Setup Checklist
| Step | Detail |
|------|--------|
| Java 17+ | Install JDK |
| Build tool | Maven or Gradle |
| Initializr | start.spring.io |
| Dependencies | web, jpa, devtools, etc. |
| IDE | IntelliJ / VS Code |

### Project Structure (Generated)
```
demo/
├── src/main/java/com/example/demo/
│   ├── DemoApplication.java
├── src/main/resources/
│   ├── application.properties
├── pom.xml
└── mvnw / mvnw.cmd
```

### Build Tools
| Tool | File | Command |
|------|------|---------|
| Maven | pom.xml | `./mvnw spring-boot:run` |
| Gradle | build.gradle | `./gradlew bootRun` |

---

## 5. Syntax (Key File)

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

---

## 6. Flow Explanation

```mermaid
flowchart LR
    A[Initializr] --> B[Generate zip]
    B --> C[Open in IDE]
    C --> D[Add dependencies]
    D --> E[Write code]
    E --> F[Run & verify]
```

---

## 7. Memory Representation

Skip — setup topic.

---

## 8. Simple Example — pom.xml (Maven)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.3.0</version>
        <relativePath/>
    </parent>

    <groupId>com.example</groupId>
    <artifactId>demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <properties>
        <java.version>17</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

---

## 9. Intermediate Example — First Controller

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.*;

@SpringBootApplication
@RestController
public class DemoApplication {

    @GetMapping("/status")
    public String status() {
        return "Service is up";
    }

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

---

## 10. Advanced Example — Running & Packaging

```bash
# Run in dev (auto-restart via devtools)
./mvnw spring-boot:run

# Package an executable JAR
./mvnw clean package

# Run the JAR (works anywhere with a JVM)
java -jar target/demo-0.0.1-SNAPSHOT.jar

# Verify
curl http://localhost:8080/status
```

---

## 11. Output

For the **Intermediate Example**:

```
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v3.3.0)

Started DemoApplication in 1.2 seconds
```

`curl http://localhost:8080/status` returns:

```
Service is up
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| JDK version mismatch | Match Java 17+ to the project |
| Forgetting web starter | Endpoints don't respond |
| Package scan misses controllers | Root package placement |
| Running without wrapper | Use ./mvnw for reproducibility |
| Port conflict | Change server.port |

---

## 13. Best Practices

- Always generate from Spring Initializr.
- Use the Maven/Gradle wrapper.
- Keep the main class at the package root.
- Set Java version in properties.
- Test with curl or a REST client early.

---

## 14. Interview Questions

**Q1. How do you start a Spring Boot project?**
*Spring Initializr → generate → open in IDE → add code.*

**Q2. What is the main class?**
*@SpringBootApplication + main that calls SpringApplication.run.*

**Q3. What does the wrapper do?**
*Pins the build tool version for reproducible builds.*

**Q4. Default port?**
*8080; change with server.port.*

**Q5. How do you run the JAR?**
*java -jar target/xxx.jar.*

---

## 15. Practice Exercises

1. Generate a Boot project with web + devtools.
2. Add a /status endpoint.
3. Run with the wrapper; check the console.
4. Package to a JAR and run it standalone.

---

## 16. Coding Challenges

1. **Easy:** Hello World Boot app.
2. **Medium:** App with 3 endpoints (paths + params).
3. **Hard:** JAR deployment with custom server.port via CLI argument.

---

## 17. Summary

- Spring Initializr scaffolds instantly.
- @SpringBootApplication + main = entry point.
- Wrapper = reproducible builds.
- Package to executable JAR; run with java -jar.
- Setup is a daily skill — make it muscle memory.

➡️ Continue to: [Controllers & REST](Controllers-and-REST.md)