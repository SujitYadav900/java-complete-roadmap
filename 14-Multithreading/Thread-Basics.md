# Thread Basics

## 1. Introduction

A **thread** is an independent path of execution within a program. The main method runs in the `main` thread; you can spawn more threads to do work **concurrently** — downloading while typing, for example.

---

## 2. Why This Topic Is Important

- UIs freeze without threads (no background work).
- Servers handle many users via threads.
- Interviewers start with "what is a thread?"
- Understanding threads is prerequisite for concurrency at work.

---

## 3. Real-World Analogy

Think of a **restaurant kitchen**. One chef cooking one dish at a time (single thread) is slow. A kitchen with many chefs (threads) prepares many dishes at once. But too many chefs bump into each other — which is why we coordinate them (synchronization, later topics).

---

## 4. Theory

### Creating a Thread (way 1 — extend Thread)
```java
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Running in " + Thread.currentThread().getName());
    }
}
new MyThread().start();
```

### Creating a Thread (way 2 — Runnable)
```java
Thread t = new Thread(() -> System.out.println("Hello from thread"));
t.start();
```

### Key Points
| Concept | Detail |
|---------|--------|
| `start()` | Creates a new OS thread and calls `run()` |
| `run()` | The work to do — don't call directly |
| `Thread.currentThread()` | Get the running thread |
| `getName()` / `getId()` | Identity |
| `sleep(ms)` | Pause (throws InterruptedException) |
| `join()` | Wait for this thread to finish |

### Concurrency vs Parallelism
- **Concurrency**: multiple tasks making progress interleaved.
- **Parallelism**: multiple tasks running truly simultaneously (multi-core).

---

## 5. Syntax

```java
Thread t = new Thread(() -> {
    // work
});
t.start();
```

---

## 6. Flow Explanation

```mermaid
flowchart TD
    A[main thread] --> B[start() on new thread]
    B --> C[New OS thread scheduled]
    C --> D[run() executes]
    D --> E[Terminated]
    A --> F[main continues]
    F --> G[join waits for thread]
```

---

## 7. Memory Representation

Each thread has its **own stack** (local variables, call frames) but **shares the heap** (objects). Sharing is powerful — and dangerous (next topics).

---

## 8. Simple Example

```java
public class ThreadDemo {
    public static void main(String[] args) {
        System.out.println("Main: " + Thread.currentThread().getName());

        Thread worker = new Thread(() -> {
            System.out.println("Worker: " + Thread.currentThread().getName());
            for (int i = 1; i <= 3; i++) {
                System.out.println("Working " + i);
                try { Thread.sleep(500); } catch (InterruptedException e) { }
            }
        });

        worker.start();
        System.out.println("Main thread done (worker continues)");
    }
}
```

---

## 9. Intermediate Example — Join to Wait

```java
public class JoinDemo {
    public static void main(String[] args) throws InterruptedException {
        Runnable task = () -> {
            for (int i = 1; i <= 5; i++) {
                System.out.println(Thread.currentThread().getName()
                        + " step " + i);
                try { Thread.sleep(200); } catch (InterruptedException e) { }
            }
        };

        Thread t1 = new Thread(task, "T1");
        Thread t2 = new Thread(task, "T2");

        t1.start();
        t1.join();          // main waits for t1
        t2.start();
        t2.join();          // main waits for t2

        System.out.println("All threads finished.");
    }
}
```

---

## 10. Advanced Example — Interrupt Handling

```java
public class InterruptDemo {
    public static void main(String[] args) throws InterruptedException {
        Thread longTask = new Thread(() -> {
            try {
                Thread.sleep(10000);
                System.out.println("Task completed naturally");
            } catch (InterruptedException e) {
                System.out.println("Task interrupted! " + e.getMessage());
            }
        });

        longTask.start();
        Thread.sleep(1000);
        longTask.interrupt();      // request the thread to stop sleeping
        System.out.println("Main sent interrupt.");
    }
}
```

---

## 11. Output

For the **Simple Example** (order may vary):

```
Main: main
Main thread done (worker continues)
Worker: Thread-0
Working 1
Working 2
Working 3
```

For the **Advanced Example**:

```
Main sent interrupt.
Task interrupted! sleep interrupted
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| Calling `run()` instead of `start()` | Runs on the same thread — no concurrency |
| Starting a thread twice | `IllegalThreadStateException` |
| Throwing checked exceptions in run() | Can't — wrap them |
| Forgetting sleep throws InterruptedException | Catch or propagate |
| Relying on thread order | Order is NOT guaranteed — use join/coordination |

---

## 13. Best Practices

- Prefer **Runnable** (or lambdas) over extending Thread.
- Use `start()`, never `run()` directly.
- Name your threads for debugging (`new Thread(task, "worker-1")`).
- Handle `InterruptedException` — respect the interrupt flag.
- Prefer `ExecutorService` (later topic) over raw threads.

---

## 14. Interview Questions

**Q1. What is a thread?**
*An independent path of execution; each has its own stack but shares the heap.*

**Q2. start() vs run()?**
*start() creates a new thread; run() just executes on the current thread.*

**Q3. What does join() do?**
*Blocks the calling thread until the target thread finishes.*

**Q4. Concurrency vs parallelism?**
*Concurrency = interleaved progress; parallelism = true simultaneous execution.*

**Q5. What is interrupt()?**
*A cooperative signal asking the thread to stop; the thread must check/handle it.*

---

## 15. Practice Exercises

1. Print "Main" and "Worker" from two threads.
2. Create 3 threads with names; observe interleaving.
3. Use join to guarantee completion order.
4. Interrupt a sleeping thread and handle it.

---

## 16. Coding Challenges

1. **Easy:** A thread that counts 1–10 with delays.
2. **Medium:** Parallel greetings: 5 threads print their names.
3. **Hard:** A race where join guarantees all download-simulations finish before "All done".

---

## 17. Summary

- Thread = independent execution path (own stack, shared heap).
- `start()` launches; `run()` is the work.
- `sleep`, `join`, `interrupt` control threads.
- Order is never guaranteed — coordinate explicitly.
- Prefer Runnable/lambdas; ExecutorService later.

➡️ Continue to: [Runnable vs Thread](Runnable-vs-Thread.md)