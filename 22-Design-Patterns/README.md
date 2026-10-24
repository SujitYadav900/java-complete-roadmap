# 22 — Design Patterns

> **Experience level: 🟣 Advanced (2 – 4 years)**

**Design patterns** are proven, reusable solutions to common software problems. This module covers the creational, structural, and behavioral patterns you'll actually use — especially the ones Spring relies on.

---

## 📂 Files in This Module

| File | What You'll Learn |
|------|-------------------|
| [What-are-Design-Patterns.md](What-are-Design-Patterns.md) | Why patterns matter, the three categories |
| [Singleton.md](Singleton.md) | One instance, global access |
| [Factory.md](Factory.md) | Creating objects without specifying classes |
| [Builder.md](Builder.md) | Step-by-step object construction |
| [Observer.md](Observer.md) | Event notification between objects |
| [Strategy.md](Strategy.md) | Swappable algorithms |
| [Exercises.md](Exercises.md) | Practice tasks for this module |

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:

1. ✅ Categorize patterns (creational/structural/behavioral).
2. ✅ Implement Singleton correctly (thread-safe).
3. ✅ Use Factory and Builder patterns.
4. ✅ Apply Observer for notifications.
5. ✅ Swap behavior with Strategy.

---

## 🧭 Module Map

```mermaid
flowchart LR
    A[What are Patterns] --> B[Singleton]
    B --> C[Factory]
    C --> D[Builder]
    D --> E[Observer]
    E --> F[Strategy]
    F --> G[Exercises]
```

---

## 📌 Quick Preview

```java
// Singleton
class Config {
    private static final Config INSTANCE = new Config();
    private Config() {}
    static Config get() { return INSTANCE; }
}
```

---

## Next Module

➡️ [23 — DSA in Java](../23-DSA-in-Java/README.md) — data structures and algorithms.