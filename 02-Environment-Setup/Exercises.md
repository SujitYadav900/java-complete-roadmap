# Exercises — Environment Setup

> Practice exercises to confirm your Java environment is **fully working** before moving to Module 04. Do them in order.

---

## 1. Verify Your Installation

Run these in a **terminal** and confirm each returns a result:

```powershell
java -version
javac -version
echo %JAVA_HOME%
```

**Success criteria:** all three commands print something. If `javac` fails, recheck your `Path`.

---

## 2. First Compile & Run

Create `Practice.java`:

```java
public class Practice {
    public static void main(String[] args) {
        System.out.println("My environment is ready!");
        System.out.println("Java version: " + System.getProperty("java.version"));
    }
}
```

Compile and run:

```powershell
javac Practice.java
java Practice
```

**Expected:**
```
My environment is ready!
Java version: 21.0.x
```

---

## 3. Working with Arguments

Modify `Practice.java` to greet a person passed as an argument:

```java
public class Practice {
    public static void main(String[] args) {
        System.out.println("Hello, " + args[0] + "!");
        System.out.println("Number of args: " + args.length);
    }
}
```

Run:

```powershell
javac Practice.java
java Practice Kumar
```

**Expected:**
```
Hello, Kumar!
Number of args: 1
```

---

## 4. Try Both IDEs (optional but recommended)

| Task | VS Code | IntelliJ |
|------|---------|----------|
| Create a new file `Welcome.java` | ✔ | ✔ |
| Print "Welcome to Java" | ✔ | ✔ |
| Run using the Run button | ✔ | ✔ |
| Set a breakpoint & debug | ✔ | ✔ |

---

## 5. Compile Into a Separate Folder

```powershell
mkdir build
javac -d build Practice.java
java -cp build Practice Kumar
```

**Question:** Where is `Practice.class` now? Where is your source still?

---

## 6. Run the JDK's JShell REPL

```powershell
jshell
```

Inside JShell type:

```java
int x = 10;
int y = 5;
x * y
```

Press `/exit` to leave.

---

## 7. Self-Assessment Checklist

- [ ] `java -version` works from any folder
- [ ] `javac -version` works from any folder
- [ ] `JAVA_HOME` is set and points to the JDK root
- [ ] I can compile with `javac` and run with `java`
- [ ] I ran a program in **both** an IDE and the terminal
- [ ] I used `jshell` at least once
- [ ] I can run a program with command-line arguments

---

## 8. Challenge

Create `Bio.java` that:

1. Takes **3 arguments**: name, age, city.
2. Prints all three in a formatted block.
3. If fewer than 3 arguments are given, prints "Please pass name, age, and city".

**Run:** `java Bio Ajay 25 Pune`

---

## 🎉 Congratulations!

Your environment is ready. Move to **[Module 04 — Java Basics](../04-Java-Basics/README.md)**.