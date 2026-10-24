# What is Maven

## 1. Introduction

**Maven** is a build automation tool for Java. It standardizes how projects are compiled, tested, packaged, and deployed using a declarative XML file (`pom.xml`) and a set of build **lifecycles**.

---

## 2. Why This Topic Is Important

- Nearly every Java job uses Maven or Gradle.
- Interviewers test the build lifecycle and goals.
- Maven resolves dependencies automatically.
- Frameworks (Spring Boot) are bootstrapped via Maven.

---

## 3. Real-World Analogy

Think of Maven as a **kitchen brigade system** with fixed stations. Instead of each chef improvising, there's a standard flow: prep (compile) → taste (test) → plate (package) → store (install). Same stations, same order, every dish — predictable and repeatable.

---

## 4. Theory

### The Build Lifecycles
Maven defines three lifecycles:
| Lifecycle | Phases (main) | Purpose |
|-----------|---------------|---------|
| `default` | validate → compile → test → package → install → deploy | Core build |
| `clean` | clean | Delete target/ |
| `site` | site | Generate docs |

### Default Lifecycle Phases
| Phase | What Happens |
|-------|--------------|
| validate | Project is correct |
| compile | Compile source → target/classes |
| test | Run unit tests |
| package | Build JAR/WAR |
| verify | Integration checks |
| install | Copy to local repository |
| deploy | Upload to remote repo |

### Key Concepts
| Concept | Meaning |
|---------|---------|
| `pom.xml` | The project's brain (POM = Project Object Model) |
| Plugin | Does the work (compiler, surefire) |
| Dependency | External library |
| Local repository | `~/.m2/repository` — cached deps |
| Coordinates | groupId:artifactId:version |

---

## 5. Syntax (command)

```bash
mvn clean compile
mvn test
mvn package
mvn install
```

---

## 6. Flow Explanation

```mermaid
flowchart LR
    A[pom.xml] --> B[validate]
    B --> C[compile]
    C --> D[test]
    D --> E[package]
    E --> F[install]
    F --> G[local repo]
```

---

## 7. Memory Representation

Build artifacts live under `target/`; downloaded dependencies under `~/.m2/repository`.

---

## 8. Simple Example — A Minimal Project

`pom.xml`:
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>hello-app</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>
</project>
```

`src/main/java/com/example/Hello.java`:
```java
package com.example;

public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, Maven!");
    }
}
```

Build:
```bash
mvn package
java -cp target/hello-app-1.0.0.jar com.example.Hello
```

---

## 9. Intermediate Example — Adding JUnit

`pom.xml` (excerpt):
```xml
<dependencies>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>5.10.2</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

`src/test/java/com/example/HelloTest.java`:
```java
package com.example;

import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class HelloTest {
    @Test
    void helloWorks() {
        assertTrue("Hello, Maven!".startsWith("Hello"));
    }
}
```

Run: `mvn test`

---

## 10. Advanced Example — Lifecycle Output

```bash
mvn clean install -DskipTests
```
This runs: clean → validate → compile → test (skipped) → package → install. The JAR lands in `target/` and your local repo.

---

## 11. Output

For the **Simple Example** (`mvn package` on success):

```
[INFO] --- maven-jar-plugin ... ---
[INFO] Building jar: target/hello-app-1.0.0.jar
[INFO] BUILD SUCCESS
```

For the **Intermediate Example** (`mvn test`):

```
[INFO] Tests run: 1, Failures: 0, Errors: 0
[INFO] BUILD SUCCESS
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| Wrong coordinates | groupId/artifactId/version must be unique |
| Missing `src/main/java` | Default layout required |
| Forgetting to run `mvn` goals | `mvn compile` before `mvn package` |
| Dependency version typos | Check Maven Central |
| Stale local repo | `mvn -U` forces updates |

---

## 13. Best Practices

- Use the standard directory layout.
- Keep the version at the top of pom.xml.
- Commit pom.xml, never `target/`.
- Use properties for versions.
- Use `-DskipTests` only when intentional.

---

## 14. Interview Questions

**Q1. What is Maven?**
*A build automation tool managing compilation, testing, packaging, and dependencies via pom.xml.*

**Q2. What is a lifecycle?**
*A sequence of phases (validate → compile → test → package → install → deploy).*

**Q3. What are Maven coordinates?**
*groupId:artifactId:version — uniquely identify a project/artifact.*

**Q4. Where are dependencies stored locally?**
*~/.m2/repository.*

**Q5. What is the difference between package and install?**
*package builds the JAR; install copies it into the local repository.*

---

## 15. Practice Exercises

1. Create a minimal Maven project from scratch.
2. Add a dependency and use it.
3. Write a JUnit test and run `mvn test`.
4. Run the full lifecycle and inspect `target/`.

---

## 16. Coding Challenges

1. **Easy:** Build and run a hello-app JAR.
2. **Medium:** Add a library (e.g., H2) and use it in a test.
3. **Hard:** Multi-module parent/child POM structure.

---

## 17. Summary

- Maven = declarative build tool.
- Lifecycle: compile → test → package → install.
- pom.xml drives everything; dependencies resolve automatically.
- Standard layout matters.
- Commit POMs, not build output.

➡️ Continue to: [POM XML](POM-XML.md)