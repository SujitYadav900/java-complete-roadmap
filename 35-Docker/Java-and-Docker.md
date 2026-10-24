# Java & Docker

## 1. Introduction

Containerizing **Spring Boot** is straightforward: a multi-stage build produces a small image with a fast startup — perfect for cloud deployment.

---

## 2. Why This Topic Is Important

- Every modern Java app deploys in containers.
- Boot's embedded server simplifies packaging.
- Layered JARs enable fast image builds.
- Interviewers expect a Boot Dockerfile.

---

## 3. Real-World Analogy

Packaging Spring Boot for Docker is like **pre-packing a food truck**: the app is self-contained (embedded Tomcat = built-in kitchen), so you just seal it in a container and it runs anywhere. Layered JARs = packing the perishable parts last so restocking is fast.

---

## 4. Theory

### Why Boot + Docker Work Well
| Boot Feature | Docker Benefit |
|--------------|----------------|
| Embedded server | No external Tomcat |
| Executable JAR | Single artifact |
| Layered JAR | Fast incremental builds |
| Actuator health | HEALTHCHECK support |

### Layered JARs (Spring Boot 2.3+)
```dockerfile
FROM eclipse-temurin:17-jre AS builder
WORKDIR /app
COPY target/*.jar app.jar
RUN java -Djarmode=layertools -jar app.jar extract
```
Extracts dependencies, spring-boot-loader, snapshot-deps, application into layers.

### Boot Dockerfile (Modern, 3.x)
```dockerfile
FROM eclipse-temurin:17-jre AS builder
WORKDIR /builder
COPY target/*.jar app.jar
RUN java -Djarmode=layertools -jar app.jar extract

FROM eclipse-temurin:17-jre
WORKDIR /app
COPY --from=builder /builder/dependencies/ ./
COPY --from=builder /builder/spring-boot-loader/ ./
COPY --from=builder /builder/snapshot-dependencies/ ./
COPY --from=builder /builder/application/ ./
EXPOSE 8080
ENTRYPOINT ["java", "org.springframework.boot.loader.launch.JarLauncher"]
```

---

## 5. Syntax

```bash
./mvnw clean package
docker build -t my-app .
docker run -p 8080:8080 my-app
```

---

## 6. Flow Explanation

```mermaid
flowchart LR
    A[JAR] --> B[Extract layers]
    B --> C[Copy layers]
    C --> D[Runtime image]
    D --> E[Run]
```

---

## 7. Memory Representation

Layered images reuse cached layers; only the app layer changes on rebuild.

---

## 8. Simple Example — Minimal Boot Dockerfile

```dockerfile
FROM eclipse-temurin:17-jre
WORKDIR /app
COPY target/my-app-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

```bash
./mvnw clean package
docker build -t my-app:1.0 .
docker run -p 8080:8080 my-app:1.0
curl http://localhost:8080/actuator/health
```

---

## 9. Intermediate Example — Multi-Stage with Maven

```dockerfile
FROM maven:3.9-eclipse-temurin-17 AS build
WORKDIR /build
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn clean package -DskipTests

FROM eclipse-temurin:17-jre
WORKDIR /app
COPY --from=build /build/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

---

## 10. Advanced Example — Layered Boot Build

```dockerfile
FROM eclipse-temurin:17-jre AS builder
WORKDIR /builder
COPY target/*.jar app.jar
RUN java -Djarmode=layertools -jar app.jar extract

FROM eclipse-temurin:17-jre
WORKDIR /app
COPY --from=builder /builder/dependencies/ ./
COPY --from=builder /builder/spring-boot-loader/ ./
COPY --from=builder /builder/snapshot-dependencies/ ./
COPY --from=builder /builder/application/ ./
EXPOSE 8080
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost:8080/actuator/health || exit 1
ENTRYPOINT ["java", "org.springframework.boot.loader.launch.JarLauncher"]
```

---

## 11. Output

For the **Simple Example**:

```
docker build -t my-app:1.0 .
Step 1/4 : FROM eclipse-temurin:17-jre
...
Successfully tagged my-app:1.0

docker run -p 8080:8080 my-app:1.0
Started MyApplication in 1.3 seconds
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| Copying whole target/ | Copy just the JAR |
| Building inside the image | Multi-stage |
| No health check | Add Actuator check |
| Hardcoded profiles | Env vars at runtime |
| Non-reproducible builds | Pin base image tags |

---

## 13. Best Practices

- Multi-stage + layered JARs.
- Copy only the final JAR.
- HEALTHCHECK via Actuator.
- Config via env vars.
- Pin image tags; non-root user.

---

## 14. Interview Questions

**Q1. How do you containerize Spring Boot?*
*Multi-stage build → JAR → runtime image.*

**Q2. What are layered JARs?*
*Boot JARs split into layers for cached builds.*

**Q3. Why multi-stage?*
*Build tools excluded from the runtime image.*

**Q4. Health check for Boot?*
*/actuator/health in the Docker HEALTHCHECK.*

**Q5. Config at runtime?*
*Env vars / SPRING_PROFILES_ACTIVE.*

---

## 15. Practice Exercises

1. Containerize a Boot JAR.
2. Convert to multi-stage.
3. Add a health check.
4. Try the layered build.

---

## 16. Coding Challenges

1. **Easy:** JAR → container → health check.
2. **Medium:** Multi-stage Boot build.
3. **Hard:** Layered build with cached dependency layers.

---

## 17. Summary

- Boot + embedded server = container-friendly.
- Multi-stage keeps images slim.
- Layered JARs make builds fast.
- Health checks via Actuator.
- Env vars for configuration.

➡️ Continue to: [Deployment](Deployment.md)