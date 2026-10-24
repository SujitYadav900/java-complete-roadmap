# Docker Compose

## 1. Introduction

**Docker Compose** defines multi-container apps in a single YAML file — one command starts the whole stack (app + DB + queue).

---

## 2. Why This Topic Is Important

- Real apps have many containers.
- Compose orchestrates them locally and in CI.
- `docker-compose.yml` is a universal pattern.
- It's the bridge to k8s concepts.

---

## 3. Real-World Analogy

Compose is a **theme-park group pass**: instead of buying tickets at each ride (running containers individually), one pass covers the whole park. `docker compose up` opens every ride, with the right order and connections.

---

## 4. Theory

### docker-compose.yml Structure
```yaml
services:
  app:
    build: .
    ports: ["8080:8080"]
    depends_on: [db]
  db:
    image: postgres:16
    environment: { POSTGRES_PASSWORD: secret }
```

### Key Sections
| Section | Purpose |
|---------|---------|
| `services` | Containers to run |
| `image` / `build` | Which image |
| `ports` | Port mapping |
| `environment` | Env vars |
| `volumes` | Persistence |
| `networks` | Service communication |
| `depends_on` | Startup order |
| `healthcheck` | Readiness |

### Common Commands
| Command | Purpose |
|---------|---------|
| `docker compose up` | Start all |
| `docker compose up -d` | Detached |
| `docker compose down` | Stop + remove |
| `docker compose logs -f` | Follow logs |
| `docker compose ps` | Status |
| `docker compose exec svc sh` | Enter a service |

### Internal DNS
Services reach each other by **service name**: app connects to `db:5432`, not localhost.

---

## 5. Syntax

```yaml
services:
  web:
    build: .
    ports:
      - "8080:8080"
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://db:5432/app
```

---

## 6. Flow Explanation

```mermaid
flowchart LR
    A[compose up] --> B[Create network]
    B --> C[Start db]
    C --> D[Start app]
    D --> E[app → db via DNS]
```

---

## 7. Memory Representation

Compose creates a project network; services resolve each other by name.

---

## 8. Simple Example — App + DB

```yaml
services:
  db:
    image: postgres:16
    environment:
      POSTGRES_DB: appdb
      POSTGRES_USER: app
      POSTGRES_PASSWORD: secret
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data

  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://db:5432/appdb
      SPRING_DATASOURCE_USERNAME: app
      SPRING_DATASOURCE_PASSWORD: secret
    depends_on:
      db:
        condition: service_healthy

volumes:
  pgdata:
```

```bash
docker compose up -d
```

---

## 9. Intermediate Example — Health Checks

```yaml
services:
  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: secret
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 3s
      retries: 5

  app:
    build: .
    ports: ["8080:8080"]
    depends_on:
      db:
        condition: service_healthy
```

---

## 10. Advanced Example — Profiles & Networks

```yaml
services:
  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: secret
    networks:
      - backend

  app:
    build: .
    ports: ["8080:8080"]
    networks:
      - backend
    profiles: ["prod"]

  monitoring:
    image: grafana/grafana:latest
    ports: ["3000:3000"]
    networks:
      - backend
    profiles: ["monitoring"]

networks:
  backend:
```

```bash
docker compose --profile prod up -d
docker compose --profile prod --profile monitoring up -d
```

---

## 11. Output

For the **Simple Example**:

```
docker compose up -d
[+] Running 3/3
 ✔ Network myapp_default   Created
 ✔ Container myapp-db-1    Started
 ✔ Container myapp-app-1   Started

docker compose ps
NAME           STATUS      PORTS
myapp-app-1    Up          0.0.0.0:8080->8080/tcp
myapp-db-1     Up (healthy) 0.0.0.0:5432->5432/tcp
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| Using localhost for DB | Use service name `db` |
| Missing depends_on conditions | App starts before DB ready |
| No volumes | Data lost on down |
| Port conflicts | Map distinct host ports |
| Forgetting rebuild | `docker compose build` after changes |

---

## 13. Best Practices

- Health checks before depends_on.
- Volumes for persistent data.
- Service-name connectivity.
- Profiles for env-specific services.
- `up -d` + `logs -f` for workflow.

---

## 14. Interview Questions

**Q1. What is Docker Compose?*
*Defines and runs multi-container apps from one YAML file.*

**Q2. How do services talk?*
*By service name on the compose network.*

**Q3. depends_on with condition?*
*Waits until healthcheck passes.*

**Q4. Compose vs Dockerfile?*
*Dockerfile builds one image; Compose orchestrates many containers.*

**Q5. How do you persist data?*
*Named volumes.*

---

## 15. Practice Exercises

1. Compose an app + DB stack.
2. Connect via service name.
3. Add health checks + depends_on.
4. Add a monitoring profile.

---

## 16. Coding Challenges

1. **Easy:** Two-service stack.
2. **Medium:** Healthy-start ordering.
3. **Hard:** Multi-profile stack with persistence and health.

---

## 17. Summary

- Compose = one YAML for the whole stack.
- Services connect by name.
- depends_on + healthchecks order startup.
- Volumes persist data.
- Profiles switch environments.

➡️ Continue to: [Java & Docker](Java-and-Docker.md)