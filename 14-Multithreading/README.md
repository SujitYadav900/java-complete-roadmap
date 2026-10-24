# 14 — Multithreading

> **Experience level: 🟠 Intermediate (1 – 2 years)**

Modern programs do many things at once. **Multithreading** lets Java run multiple tasks concurrently — essential for responsive apps, servers, and fast processing.

---

## 📂 Files in This Module

| File | What You'll Learn |
|------|-------------------|
| [Thread-Basics.md](Thread-Basics.md) | What threads are, creating and running them |
| [Runnable-vs-Thread.md](Runnable-vs-Thread.md) | Two ways to define work, and why Runnable |
| [Thread-Lifecycle.md](Thread-Lifecycle.md) | NEW, RUNNABLE, BLOCKED, WAITING, TERMINATED |
| [Thread-Synchronization.md](Thread-Synchronization.md) | Race conditions, synchronized, locking |
| [Thread-Communication.md](Thread-Communication.md) | wait, notify, notifyAll between threads |
| [ExecutorService.md](ExecutorService.md) | Thread pools — the modern way |
| [Exercises.md](Exercises.md) | Practice tasks for this module |

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:

1. ✅ Create and start threads two ways.
2. ✅ Explain the thread lifecycle and states.
3. ✅ Fix race conditions with synchronization.
4. ✅ Communicate between threads with wait/notify.
5. ✅ Manage pools with ExecutorService.

---

## 🧭 Module Map

```mermaid
flowchart LR
    A[Thread Basics] --> B[Runnable vs Thread]
    B --> C[Lifecycle]
    C --> D[Synchronization]
    D --> E[Thread Communication]
    E --> F[Executor Service]
    F --> G[Exercises]
```

---

## 📌 Quick Preview

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class Preview {
    public static void main(String[] args) {
        ExecutorService pool = Executors.newFixedThreadPool(2);
        pool.submit(() -> System.out.println("Task A"));
        pool.submit(() -> System.out.println("Task B"));
        pool.shutdown();
    }
}
```

---

## Next Module

➡️ [15 — Generics](../15-Generics/README.md) — type-safe, reusable code.