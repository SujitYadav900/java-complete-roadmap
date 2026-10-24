# Deployment

## 1. Introduction

Spring Boot apps deploy as **executable JARs** (or WARs) with an embedded server. One `java -jar` command runs anywhere a JVM exists — a huge ops win.

---

## 2. Why This Topic Is Important

- Executable JARs simplify deployments to VMs, k8s, and clouds.
- Docker layers cache Boot JARs efficiently.
- Environment config via CLI/profiles is standard.
- Deployment skills are required for production work.

---

## 3. Real-World Analogy

The Boot JAR is a **pre-packaged food truck**: everything is inside (kitchen + ingredients = embedded Tomcat + app). You park it anywhere (deploy) and start cooking with a single switch (java -jar). No outside restaurant building needed.

---

## 4. Theory

### Packaging Options
| Type | Use | Embedded Server |
|------|-----|-----------------|
| JAR (default) | Microservices, standalone | Yes |
| WAR | Traditional app servers | Optional |

### Build & Run
```bash
./mvnw clean package
java -jar target/app-0.0.1-SNAPSHOT.jar
```

### Deployment Targets
| Target | Notes |
|--------|-------|
| VM / server | java -jar + systemd |
| Docker | Multi-stage build, layered JAR |
| Kubernetes | Pod with /actuator/health probe |
| Cloud (AWS/Azure/GCP) | Managed or containerized |

### Config at Deploy Time
| Way | Example |
|-----|---------|
| CLI args | `--server.port=8081` |
| Env vars | `SPRING_PROFILES_ACTIVE=prod` |
| External file | `--spring.config.location=...` |

---

## 5. Syntax

```bash
java -jar app.jar --server.port=8081
SPRING_PROFILES_ACTIVE=prod java -jar app.jar
```

---

## 6. Flow Explanation

```mermaid
flowchart LR
    A[Package JAR] --> B[Layered JAR]
    B --> C[Push to registry]
    C --> D[Deploy]
    D --> E[Health check]
    E --> F[Traffic]
```

---

## 7. Memory Representation

Skip — deployment topic.

---

## 8. Simple Example — Package & Run

```bash
# package
./mvnw clean package

# run locally
java -jar target/demo-0.0.1-SNAPSHOT.jar

# verify
curl http://localhost:8080/status
```

---

## 9. Intermediate Example — Dockerfile

```dockerfile
# Build stage
FROM maven:3.9-eclipse-temurin-17 AS build
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn clean package -DskipTests

# Runtime stage
FROM eclipse-temurin:17-jre
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

```bash
docker build -t my-app .
docker run -p 8080:8080 my-app
```

---

## 10. Advanced Example — Kubernetes Deployment

```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: demo-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: demo
  template:
    metadata:
      labels:
        app: demo
    spec:
      containers:
        - name: app
          image: my-registry/demo:1.0.0
          ports:
            - containerPort: 8080
          env:
            - name: SPRING_PROFILES_ACTIVE
              value: prod
          readinessProbe:
            httpGet:
              path: /actuator/health
              port: 8080
            initialDelaySeconds: 10
```

---

## 11. Output

For the **Simple Example**:

```
Started DemoApplication in 1.2 seconds
curl http://localhost:8080/status → Service is up
```

For the **Docker Example**:

```
docker run -p 8080:8080 my-app
  Started DemoApplication ...
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| Forgetting -DskipTests in build image | Slow/fragile builds |
| Copying target/ to runtime image | Multi-stage build |
| No health check | k8s can't route reliably |
| Hardcoded config in JAR | Use env vars / profiles |
| Building fat JAR for libraries | Boot is apps, not libs |

---

## 13. Best Practices

- Use multi-stage Docker builds with layered JARs.
- Configure via env vars at deploy time.
- Always expose /actuator/health.
- Pin image tags; avoid :latest.
- Prefer the JAR packaging for microservices.

---

## 14. Interview Questions

**Q1. How do you deploy a Spring Boot app?**
*Package to an executable JAR; run with java -jar or containerize.*

**Q2. JAR vs WAR?**
*JAR = standalone embedded server; WAR = external server.*

**Q3. Why multi-stage Dockerfile?**
*Smaller runtime image; build deps stay in the build stage.*

**Q4. How to change config at deploy time?**
*Env vars, CLI args, or external config file.*

**Q5. What's a readiness probe?**
*k8s checks /actuator/health before sending traffic.*

---

## 15. Practice Exercises

1. Package and run a JAR standalone.
2. Pass server.port via CLI.
3. Write a Dockerfile and run a container.
4. Set SPRING_PROFILES_ACTIVE and verify.

---

## 16. Coding Challenges

1. **Easy:** Executable JAR run.
2. **Medium:** Dockerfile with a health check.
3. **Hard:** Kubernetes deployment manifest with probes + env.

---

## 17. Summary

- Executable JAR + embedded Tomcat = easy deploy.
- Multi-stage Docker keeps images small.
- Config via env vars at deploy time.
- Health probes enable k8s automation.
- Boot deployment is a production superpower.

➡️ Continue to: [Exercises](Exercises.md)