# Exercises — Maven

## 1. Introduction

Practice creating projects, running builds, and managing dependencies with Maven. If you don't have Maven installed, see the environment setup module — otherwise, build hands-on.

---

## 2. Why This Topic Is Important

- Maven is used in almost every Java job.
- Interviewers test lifecycle, pom, and dependency knowledge.
- CI/CD runs Maven commands.
- Framework projects (Spring) start as Maven projects.

---

## 3. Real-World Analogy

Practice is like **learning the building's control room**: know which button (command) does what, which pipes (dependencies) feed what, and how the floors (phases) connect. Then any building is manageable.

---

## 4. Theory — Key Commands

```bash
mvn clean
mvn compile
mvn test
mvn package
mvn install
mvn dependency:tree
```

---

## 5. Syntax Reminder

```xml
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example</groupId>
  <artifactId>app</artifactId>
  <version>1.0.0</version>
  <properties>...</properties>
  <dependencies>...</dependencies>
</project>
```

---

## 6. Practice Flow

```mermaid
flowchart LR
    A[Scaffold project] --> B[pom.xml]
    B --> C[Add deps]
    C --> D[Write code + tests]
    D --> E[Run lifecycle]
    E --> F[Inspect target]
```

---

## 7. Memory Representation

Skip — build mechanics.

---

## 8. Simple Exercises

**Ex 1 — Scaffold**
Create the full directory structure by hand (no IDE wizard).

**Ex 2 — Minimal POM**
Write a pom with coordinates and packaging jar.

**Ex 3 — Compile**
`mvn clean compile`; confirm `target/classes`.

---

## 9. Intermediate Exercises

**Ex 4 — Test Dependency**
Add JUnit (test scope); write one test; run `mvn test`.

**Ex 5 — Runtime Dependency**
Add H2 (runtime scope); run a tiny JDBC program.

**Ex 6 — Package**
`mvn package`; inspect the JAR; run it with `java -cp`.

---

## 10. Advanced Exercises

**Ex 7 — Properties**
Centralize Java version + deps versions in `<properties>`.

**Ex 8 — Dependency Tree**
Run `mvn dependency:tree`; explain each line's scope.

**Ex 9 — Exclusion**
Add a starter-like dependency with an exclusion; verify with the tree.

**Ex 10 — Parent Structure**
Create a parent pom (packaging pom) with dependencyManagement; a child inherits versions.

---

## 11. Expected Output Samples

**Ex 4 (`mvn test`):**
```
Tests run: 1, Failures: 0, Errors: 0
[INFO] BUILD SUCCESS
```

**Ex 8 (dependency:tree):**
```
[INFO] +- org.junit.jupiter:junit-jupiter:jar:5.10.2:test
```

---

## 12. Common Mistakes

| Mistake | Fix |
|---------|-----|
| Wrong directory layout | Standard Maven layout |
| `mvn run` | mvn exec:java / java -cp |
| Forgetting scope | Add test/runtime/provided |
| Committing target/ | .gitignore |
| Version typos | Check Maven Central |

---

## 13. Best Practices

- Commit pom.xml and src/ only.
- Use `mvn clean install` to verify.
- Audit deps with the tree.
- Let parent poms manage versions.
- Run `mvn test` early and often.

---

## 14. Interview Practice Questions

**Q1.** What are Maven phases in order?
**Q2.** What are coordinates?
**Q3.** What is the difference between package and install?
**Q4.** Name the dependency scopes.
**Q5.** How do you resolve a version conflict?

---

## 15. Self-Check

- [ ] I can create a Maven project from scratch.
- [ ] I can read and write a pom.xml.
- [ ] I know the standard directory layout.
- [ ] I can run the full lifecycle.
- [ ] I can manage scopes and exclusions.
- [ ] I can debug with dependency:tree.

---

## 16. Next Steps

- Move to [Module 21 — Gradle](../21-Gradle/README.md) and compare.
- Spring Boot modules (25+) are Maven-based.

---

## 17. Summary

- Maven = declarative builds via pom.xml.
- Lifecycle: compile → test → package → install.
- Standard layout, correct scopes, centralized versions.
- Commands run in CI.
- Practice the full flow hands-on.

➡️ Module complete! Continue to [21 — Gradle](../21-Gradle/README.md).