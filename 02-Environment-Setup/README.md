# 02 — Environment Setup

> Before you can write and run Java, you must install the tools. This module walks you through installing the **JDK**, setting up **VS Code** and **IntelliJ IDEA**, configuring **JAVA_HOME**, and running your first program from both the terminal and an IDE.

---

## 📂 Files in This Module

| File | What You'll Learn |
|------|-------------------|
| [Install-JDK.md](Install-JDK.md) | Download and install the JDK on Windows, macOS, and Linux |
| [Install-VS-Code.md](Install-VS-Code.md) | Set up VS Code with the Java extensions |
| [Install-IntelliJ.md](Install-IntelliJ.md) | Install and configure IntelliJ IDEA |
| [Configure-JAVA_HOME.md](Configure-JAVA_HOME.md) | Set `JAVA_HOME` and `Path` correctly |
| [Compile-and-Run.md](Compile-and-Run.md) | Compile and run programs from the terminal |
| [Exercises.md](Exercises.md) | Hands-on tasks to verify your setup |

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:

1. ✅ Install the JDK (17 or 21 LTS) on your operating system.
2. ✅ Set `JAVA_HOME` and `Path` so `java` and `javac` work anywhere.
3. ✅ Configure both VS Code and IntelliJ IDEA for Java.
4. ✅ Compile a `.java` file and run it from the terminal.
5. ✅ Troubleshoot common environment errors.

---

## 🧭 Recommended Setup Path

```mermaid
flowchart LR
    A[Install JDK] --> B[Set JAVA_HOME + Path]
    B --> C[Verify: java -version]
    C --> D[Install IDE]
    D --> E[Write + Run HelloWorld]
    E --> F[Practice with Exercises]
```

> **💡 Which IDE should you pick?**
>
> - **IntelliJ IDEA Community** — the most popular Java IDE, recommended for the Spring modules later.
> - **VS Code** — lightweight and fast; great for quick practice.
>
> You don't need both — but knowing both is a bonus.

---

## 🚀 Quick-Start Summary

| Task | Command / Action |
|------|------------------|
| Check Java is installed | `java -version` and `javac -version` |
| Compile a program | `javac Hello.java` |
| Run a program | `java Hello` |
| Fix "command not found" | Add `%JAVA_HOME%\bin` to `Path` |

---

## 📌 Prerequisites

- A computer with internet access.
- Administrator rights (Windows) or sudo access (Linux/macOS) for installation.
- Roughly 30 minutes of uninterrupted time.

---

## Next Module

➡️ [04 — Java Basics](../04-Java-Basics/README.md) — variables, data types, operators, and your first real logic.