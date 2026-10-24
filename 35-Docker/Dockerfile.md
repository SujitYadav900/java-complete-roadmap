# Dockerfile

## 1. Introduction

A **Dockerfile** is the recipe that builds an image. Each instruction creates a layer, ending in `ENTRYPOINT`/`CMD` — the command containers run.

---

## 2. Why This Topic Is Important

- Dockerfiles define how apps are packaged.
- Efficiency matters (caching, size).
- Multi-stage builds are the professional standard.
- Interviewers ask about layers and caching.

---

## 3. Real-World Analogy

A Dockerfile is a **step-by-step cooking recipe**: prep ingredients (COPY), combine them (RUN), and finally serve (ENTRYPOINT). Reusing a dish (cached layer) when one ingredient changes is faster than remaking everything — that's layer caching.

---

## 4. Theory

### Core Instructions
| Instruction | Purpose |
|-------------|---------|
| `FROM` | Base image (must be first) |
| `WORKDIR` | Working directory |
| `COPY` | Copy files into the image |
| `RUN` | Run commands at build time |
| `ENV` | Set environment variables |
| `EXPOSE` | Document the port |
| `CMD` | Default command (overridable) |
| `ENTRYPOINT` | Fixed entry command |
| `ARG` | Build-time variable |

### CMD vs ENTRYPOINT
| Instruction | Behavior |
|-------------|----------|
| `CMD ["java", "-jar", "app.jar"]` | Overridable at run |
| `ENTRYPOINT ["java"]` + `CMD ["-jar", "app.jar"]` | Entry fixed, args overridable |

### Layer Caching
- Each instruction = one layer.
- Docker caches layers; only changed layers rebuild.
- Order matters: copy rarely-changing things first.

---

## 5. Syntax

```dockerfile
FROM eclipse-temurin:17-jre
WORKDIR /app
COPY target/app.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

---

## 6. Flow Explanation

```mermaid
flowchart LR
    A[FROM base] --> B[COPY files]
    B --> C[RUN commands]
    C --> D[Set entrypoint]
    D --> E[Image]
```

---

## 7. Memory Representation

Each instruction adds a layer; the final image is base + all layers.

---

## 8. Simple Example — Single Stage

```dockerfile
FROM eclipse-temurin:17-jre
WORKDIR /app
COPY target/my-app-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

Build and run:
```bash
docker build -t my-app:1.0 .
docker run -p 8080:8080 my-app:1.0
```

---

## 9. Intermediate Example — Env & Health

```dockerfile
FROM eclipse-temurin:17-jre
WORKDIR /app

ENV SPRING_PROFILES_ACTIVE=prod
ENV JAVA_OPTS="-Xmx512m"

COPY target/app.jar app.jar
EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost:8080/actuator/health || exit 1

ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar app.jar"]
```

---

## 10. Advanced Example — Multi-Stage Build

```dockerfile
# Stage 1: build with Maven
FROM maven:3.9-eclipse-temurin-17 AS build
WORKDIR /build
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn clean package -DskipTests

# Stage 2: slim runtime
FROM eclipse-temurin:17-jre
WORKDIR /app
COPY --from=build /build/target/app.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

Benefits:
- Build tools stay out of the runtime image.
- Image is much smaller.
- Caching keeps builds fast.

---

## 11. Output

For the **Simple Example** (build):

```
Step 1/5 : FROM eclipse-temurin:17-jre
 ---> 3f5d9e...
Step 2/5 : WORKDIR /app
 ---> Running in ...
Successfully built a1b2c3
Successfully tagged my-app:1.0
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| COPYing target/ into build stage | Multi-stage |
| Rarely-changing files last | Order for caching |
| Hardcoded versions | Use ARG/tags |
| No HEALTHCHECK | Ops can't verify |
| Running as root | Add a non-root user |

---

## 13. Best Practices

- Multi-stage builds.
- Order COPY for caching.
- Slim runtime base.
- HEALTHCHECK for services.
- Pin base image tags.

---

## 14. Interview Questions

**Q1. What is a Dockerfile?*
*The recipe that builds an image.*

**Q2. CMD vs ENTRYPOINT?*
*CMD overridable; ENTRYPOINT fixed (both set the run command).*

**Q3. What is layer caching?*
*Unchanged layers are reused — faster builds.*

**Q4. Why multi-stage?*
*Small runtime image; build deps removed.*

**Q5. What is EXPOSE?*
*Documents the port (doesn't publish it).*

---

## 15. Practice Exercises

1. Write a single-stage Dockerfile for a JAR.
2. Add ENV and EXPOSE.
3. Add a HEALTHCHECK.
4. Convert to a multi-stage build.

---

## 16. Coding Challenges

1. **Easy:** Run a JAR in a container.
2. **Medium:** Multi-stage with caching.
3. **Hard:** Layered Boot JAR + health check + non-root user.

---

## 17. Summary

- Dockerfile = image recipe, layer by layer.
- CMD/ENTRYPOINT define the run command.
- Multi-stage = small, clean images.
- Order instructions for caching.
- HEALTHCHECK for production readiness.

➡️ Continue to: [Docker Commands](Docker-Commands.md)