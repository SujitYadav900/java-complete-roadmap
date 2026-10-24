# History of Java

## 1️⃣ Introduction

Java is a **programming language and computing platform** released by Sun Microsystems in 1995. It was designed to be simple, object-oriented, and portable — able to run on any device. Today, Java is one of the most widely used programming languages in the world, powering Android apps, banking systems, and enterprise software.

---

## 2️⃣ Why This Topic Is Important

- Understanding Java's history explains **why Java is platform-independent** — the core reason companies adopt it.
- Interviewers frequently ask: *"Why was Java created?"* and *"Who developed Java?"*
- Knowing the design goals helps you understand modern Java features — they all serve the original "WORA" vision.
- It builds your confidence as a developer by grounding you in the "why" behind the language.

---

## 3️⃣ Real-World Analogy

Think of Java like the **International Driving Permit**. Different countries have different road rules and languages, but the permit is standardized so you can drive anywhere. Java code behaves the same way — write it once, and it runs on Windows, macOS, Linux, or Android because Java provides a standardized "driver" (the JVM) on every platform.

---

## 4️⃣ Theory

### The Birth of Java (1991)
In June 1991, a small team at Sun Microsystems led by **James Gosling**, Mike Sheridan, and Patrick Naughton began the "Green Project." Their goal was to build software for **consumer electronics** — like smart TVs, toasters, and set-top boxes.

### From Oak to Java
- The language was initially called **Oak** (after an oak tree outside Gosling's window).
- It was renamed to **Java** in 1995 because "Oak" was already trademarked by a hardware company.
- "Java" was inspired by the coffee that the team drank, which came from the island of Java, Indonesia.

### The Turning Point — The Web (1994–1995)
- In 1994, the team realized Java could power **web browsers** with "applets" — small programs that run inside web pages.
- Java 1.0 was officially released on **January 23, 1996**, at SunWorld.
- The slogan **"Write Once, Run Anywhere" (WORA)** became the marketing banner of Java.

### Growth Timeline

| Year | Event |
|------|-------|
| 1991 | Green Project starts (Oak language) |
| 1995 | Renamed to Java; announced to the public |
| 1996 | Java 1.0 released |
| 1997 | Java 1.1 — inner classes, JDBC, RMI |
| 1998 | Java 2 (J2SE 1.2) — Collections framework, Swing |
| 2004 | Java 5 (Tiger) — Generics, enums, annotations, enhanced for-loop |
| 2006 | Java 6 (Mustang) |
| 2009 | **Oracle acquires Sun Microsystems** for $7.4 billion |
| 2010 | Java 7 (Dolphin) — try-with-resources, switch on strings |
| 2014 | Java 8 (LTS) — Lambdas, Streams API, Optional |
| 2017 | Java 9 — Modules, JShell |
| 2018 | Java 11 (LTS) — var keyword, HTTP client |
| 2021 | Java 17 (LTS) — sealed classes, pattern matching preview |
| 2023 | Java 21 (LTS) — virtual threads, record patterns |
| 2025 | Java 25 (LTS) — continued annual feature releases |

> **📌 Note:** Since Java 9, a **new version is released every 6 months** (March and September). LTS (Long-Term Support) versions are released every two years and are safe for production.

---

## 5️⃣ Syntax

History has no syntax — but here is how a modern Java program pays tribute to its 1991 roots:

```java
public class HelloJava {
    public static void main(String[] args) {
        System.out.println("Hello from Java, 25+ years later!");
    }
}
```

---

## 6️⃣ Flow Explanation

```mermaid
flowchart LR
    A[Green Project 1991] --> B[Oak language 1992]
    B --> C[Renamed to Java 1995]
    C --> D[Java 1.0 1996]
    D --> E[Web boom / Applets]
    E --> F[Oracle acquires Sun 2009]
    F --> G[Java 8 LTS 2014]
    G --> H[Java 11 LTS 2018]
    H --> I[Java 17 LTS 2021]
    I --> J[Java 21 LTS 2023]
    J --> K[Java 25 LTS 2025]
```

The evolution is a story of adapting to the times — from embedded devices, to the web, to enterprise servers, to cloud and big-data workloads.

---

## 7️⃣ Memory Representation

Not applicable — history is conceptual. However, the key takeaway is that Java's original architecture decision (an intermediate bytecode + a JVM per platform) is what gives Java its portability today.

---

## 8️⃣ Simple Example

```java
public class WhoCreatedJava {
    public static void main(String[] args) {
        System.out.println("James Gosling is known as the father of Java.");
    }
}
```

---

## 9️⃣ Intermediate Example

```java
public class JavaTimeline {
    public static void main(String[] args) {
        String[][] events = {
            {"1991", "Green Project started"},
            {"1995", "Java publicly announced"},
            {"1996", "Java 1.0 released"},
            {"2009", "Oracle acquired Sun"},
            {"2014", "Java 8 (LTS) released"},
            {"2021", "Java 17 (LTS) released"}
        };
        for (String[] event : events) {
            System.out.println(event[0] + "  ->  " + event[1]);
        }
    }
}
```

---

## 🔟 Advanced Example

```java
public class JavaEra {
    static String describeEra(String version) {
        return switch (version) {
            case "1.0"  -> "The pioneer era (1996)";
            case "5"    -> "The generics revolution (2004)";
            case "8"    -> "The functional era (2014)";
            case "17"   -> "The modern LTS (2021)";
            case "21"   -> "The virtual thread era (2023)";
            default     -> "A feature release";
        };
    }

    public static void main(String[] args) {
        for (String v : new String[]{"1.0", "5", "8", "17", "21", "26"}) {
            System.out.println("Java " + v + ": " + describeEra(v));
        }
    }
}
```

---

## 1️⃣1️⃣ Output

For the **Advanced Example**:

```
Java 1.0: The pioneer era (1996)
Java 5: The generics revolution (2004)
Java 8: The functional era (2014)
Java 17: The modern LTS (2021)
Java 21: The virtual thread era (2023)
Java 26: A feature release
```

---

## 1️⃣2️⃣ Common Mistakes

| Mistake | Correction |
|---------|------------|
| Saying "Java was created by Microsoft" | Java was created by **James Gosling at Sun Microsystems** (acquired by Oracle in 2009) |
| Thinking JavaScript = Java | Totally unrelated — JavaScript was created by Netscape for web browsers |
| Believing Java 8 is the "newest" version | Java 8 is old but widely used; Java 21 is the modern LTS |
| Thinking "Oak" was renamed because of copyright | It was renamed because the name **Oak** was already trademarked |
| Forgetting Oracle owns Java | Sun was the original owner; **Oracle** has owned it since 2009 |

---

## 1️⃣3️⃣ Best Practices

- Refer to versions by number (Java 8, 11, 17, 21) not codenames in professional contexts.
- Use an **LTS version** for learning so you can follow tutorials without version headaches.
- When answering interview questions, tell the history **as a story** — start with the problem (embedded devices) not the answer (Java).
- Mention the design goals (simple, portable, object-oriented) rather than just dates.

---

## 1️⃣4️⃣ Interview Questions

**Q1. Who created Java and why?**
*James Gosling at Sun Microsystems, starting 1991. The goal was to build a simple, portable language for consumer electronics that could run on many different devices.*

**Q2. Why was the language called Oak, and why was it renamed?**
*It was named Oak after a tree outside Gosling's window. It was renamed Java because the name "Oak" was already trademarked.*

**Q3. What does "Write Once, Run Anywhere" mean?**
*Java code compiles to bytecode that runs on any device with a JVM — no recompilation needed per operating system.*

**Q4. When was Java 1.0 officially released?**
*January 23, 1996.*

**Q5. Who owns Java today?**
*Oracle Corporation, which acquired Sun Microsystems in 2009.*

---

## 1️⃣5️⃣ Practice Exercises

1. Create a timeline table of 6 Java versions and one feature each introduced.
2. Write a program that prints the year Java 1.0 was released.
3. Research and note the difference between Java and JavaScript in 3 bullet points.
4. Explain in one paragraph (in your own words) why Java became so popular in the 1990s.

---

## 1️⃣6️⃣ Coding Challenges

1. **Easy:** Write a program that prints the names of all Java "flavors" you know (Java SE, Java EE, Java ME) one per line.
2. **Medium:** Create a `String[]` of the major Java release years `{1996, 2004, 2014, 2018, 2021, 2023}` and print the gap in years between consecutive releases.

---

## 1️⃣7️⃣ Summary

- Java was created by **James Gosling** at Sun Microsystems in **1991** and released as Java 1.0 in **1996**.
- The original name was **Oak**, renamed to Java for trademark reasons.
- Its motto **"Write Once, Run Anywhere"** is powered by the JVM.
- Java grew from consumer electronics → web applets → enterprise → cloud computing.
- **Oracle** has owned Java since 2009.
- Java is now released every 6 months, with **LTS versions every 2 years** (8, 11, 17, 21, 25).

➡️ Continue to: [Features of Java](Features-of-Java.md)