# Install IntelliJ IDEA

## 1. Introduction

**IntelliJ IDEA** is the most popular IDE for professional Java development. It is built by JetBrains and understands Java deeply — refactoring, navigation, debugging, and Spring support are its superpowers. The **Community Edition** is free and fully sufficient for this roadmap.

---

## 2. Why This Topic Is Important

- IntelliJ is the **industry standard** — most Java jobs use it.
- Its smart features (auto-import, refactoring, error previews) make you faster.
- Later modules (Spring, Hibernate) are far easier in IntelliJ.
- Knowing it well is a resume bullet.

---

## 3. Real-World Analogy

If VS Code is a notebook, IntelliJ is a **professional architect's studio**. It doesn't just write your code — it understands the whole building (project), suggests improvements (inspections), instantly navigates between floors (classes), and catches design mistakes before you make them.

---

## 4. Theory

### Editions
| Edition | Price | Best For |
|---------|-------|----------|
| **Community Edition** | Free | Core Java, this roadmap |
| **Ultimate Edition** | Paid | Spring, web, database tooling |

> **💡 Use the Community Edition for modules 01–23.** The Spring modules work fine with it too (Ultimate just has extra conveniences).

### Key Concepts in IntelliJ
- **Project** — a folder that IntelliJ indexes as one unit.
- **JDK Configuration** — you tell IntelliJ which JDK to compile with.
- **Run Configuration** — how to run `main` (IntelliJ auto-creates these).

---

## 5. Syntax

IntelliJ is a GUI tool; there is no install syntax, but you can install via terminal:

```powershell
# Windows (winget)
winget install JetBrains.IntelliJIDEA.Community

# macOS (Homebrew)
brew install --cask intellij-idea-ce

# Linux (snap)
sudo snap install intellij-idea-community --classic
```

---

## 6. Flow Explanation

```mermaid
flowchart LR
    A[Install IntelliJ] --> B[Open it]
    B --> C[New Project]
    C --> D[Select JDK]
    D --> E[Create main class]
    E --> F[Run green arrow]
    F --> G[Output in console]
```

---

## 7. Memory Representation

Not applicable.

---

## 8. Simple Example — Installation (GUI)

1. Download **Community** from https://www.jetbrains.com/idea/download/
2. Run the installer. Check **"Add bin folder to PATH"** and **".java" file association**.
3. Launch IntelliJ.
4. On the welcome screen: **New Project**.

---

## 9. Intermediate Example — First Project

1. **New Project** → Name: `MyFirstProject`.
2. **Language:** Java, **Build system:** IntelliJ, **JDK:** pick your installed JDK (21).
3. Click **Create**.
4. In the `src` folder, right-click → **New → Java Class** → name it `Hello`.
5. Write:
   ```java
   public class Hello {
       public static void main(String[] args) {
           System.out.println("Hello from IntelliJ!");
       }
   }
   ```
6. Click the **green play button** next to `main` (or `Shift+F10`).
7. Output shows in the **Run** panel.

---

## 10. Advanced Example — Refactoring & Navigation

1. In `Hello.java`, change the variable name:
   ```java
   public static void main(String[] args) {
       String message = "Hello from IntelliJ!";
       System.out.println(message);
   }
   ```
2. Place the cursor on `message`, press `Shift+F6` (Rename). Type `greeting` → **Enter**. The variable renames everywhere.
3. Press `Ctrl+N` and type `String` — IntelliJ jumps to the JDK source. That's navigation.
4. Right-click the class name → **Refactor → Extract Method** — try wrapping the print into a method.

---

## 11. Output

For the **Intermediate Example**:

```
Hello from IntelliJ!
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| Installing Ultimate thinking it's free | Ultimate needs a license; use Community |
| Forgetting to select the JDK | Project → Project Structure (`Ctrl+Alt+Shift+S`) → SDK |
| Creating files outside `src` | Source code belongs in `src` |
| Not using the green Run arrow | `Shift+F10` runs the current class |
| Ignoring red squiggles | IntelliJ's warnings save you debugging time |

---

## 13. Best Practices

- Let IntelliJ **auto-import** classes (it prompts).
- Use `Alt+Enter` to see the "quick fix" menu on any error.
- Learn `Ctrl+N` (navigate class) and `Shift+F6` (rename) — they're productivity gold.
- Use **Inspect Code** (`Code → Inspect Code`) to review your whole project.

---

## 14. Interview Questions

**Q1. What IDE do most Java developers use?**
*IntelliJ IDEA — it offers deep code intelligence, refactoring, and Spring support.*

**Q2. Is IntelliJ free?**
*The Community Edition is free and open-source; the Ultimate Edition is paid.*

**Q3. How do you set the JDK in IntelliJ?**
*File → Project Structure → Project → SDK → select/Add your JDK.*

---

## 15. Practice Exercises

1. Create a new IntelliJ project and run HelloWorld.
2. Use `Shift+F6` to rename a variable.
3. Use `Ctrl+N` to navigate to `ArrayList` and read its Javadoc.
4. Create a second class and call its static method from `main`.

---

## 16. Coding Challenges

1. **Easy:** Print "I love Java" using `System.out.print` (no newline) then `System.out.println`.
2. **Medium:** Use IntelliJ's Extract Method to refactor a 5-line calculation into a method.
3. **Hard:** Configure the JDK to 17 and recompile the project — verify it still runs, and note the bytecode version.

---

## 17. Summary

- IntelliJ **Community Edition** is free and perfect for this roadmap.
- Projects live in a folder with a `src` directory.
- Run with the green arrow (`Shift+F10`), debug with `F9`.
- Rename refactoring is `Shift+F6`; navigation is `Ctrl+N`.
- Set your SDK via **Project Structure**.

➡️ Next: [Configure JAVA_HOME](Configure-JAVA_HOME.md) or [Compile and Run](Compile-and-Run.md)