# 10 — Exception Handling

> **Experience level: 🟡 Basic+ (6 months – 1 year)**

Programs fail — files get deleted, networks drop, users enter bad data. **Exception handling** lets your code detect, respond to, and recover from failures gracefully instead of crashing.

---

## 📂 Files in This Module

| File | What You'll Learn |
|------|-------------------|
| [Try-Catch.md](Try-Catch.md) | The try/catch block, the heart of handling |
| [Checked-vs-Unchecked.md](Checked-vs-Unchecked.md) | Compile-time vs runtime exceptions |
| [Finally-Block.md](Finally-Block.md) | Code that always runs (cleanup) |
| [Throw-and-Throws.md](Throw-and-Throws.md) | Raising exceptions & declaring them |
| [Custom-Exceptions.md](Custom-Exceptions.md) | Your own exception types |
| [Try-With-Resources.md](Try-With-Resources.md) | Auto-closing resources (Java 7+) |
| [Best-Practices.md](Best-Practices.md) | Handle errors the professional way |
| [Exercises.md](Exercises.md) | Practice tasks for this module |

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:

1. ✅ Wrap risky code in try/catch/finally.
2. ✅ Distinguish checked vs unchecked exceptions.
3. ✅ Throw and declare exceptions correctly.
4. ✅ Create custom exceptions for your domain.
5. ✅ Auto-close resources with try-with-resources.
6. ✅ Follow error-handling best practices.

---

## 🧭 Module Map

```mermaid
flowchart LR
    A[Try-Catch] --> B[Checked vs Unchecked]
    B --> C[Finally]
    C --> D[Throw & Throws]
    D --> E[Custom Exceptions]
    E --> F[Try-With-Resources]
    F --> G[Best Practices]
    G --> H[Exercises]
```

---

## 📌 Quick Preview

```java
import java.io.BufferedReader;
import java.io.FileReader;

public class Preview {
    public static void main(String[] args) {
        try (BufferedReader br = new BufferedReader(new FileReader("data.txt"))) {
            System.out.println(br.readLine());
        } catch (Exception e) {
            System.out.println("Could not read file: " + e.getMessage());
        }
    }
}
```

---

## Next Module

➡️ [11 — Packages & Access Modifiers](../11-Packages-and-Access-Modifiers/README.md) — organize and protect your code.