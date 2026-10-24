# INSTALLATION.md

> Step-by-step installation guide to get Java running on **Windows, macOS, and Linux**. Follow this before starting [Module 02](02-Environment-Setup/README.md).

---

## 🔍 Quick Summary — What You Need

| Tool | Purpose | Where to Get It |
|------|---------|-----------------|
| **JDK 17 (LTS) or 21 (LTS)** | Compile & run Java programs | [Adoptium Temurin](https://adoptium.net/) or [Oracle](https://www.oracle.com/java/technologies/downloads/) |
| **VS Code** (lightweight) | Write Java code | https://code.visualstudio.com/ |
| **IntelliJ IDEA Community** (recommended for Spring) | Full-featured IDE | https://www.jetbrains.com/idea/download/ |

> **💡 Tip:** In 2026 the industry standard is **Java 17 or 21 (LTS)**. Choose 21 if your team uses it, otherwise 17 — both are fine for learning.

---

## 🪟 Windows Installation

### Step 1 — Install the JDK
1. Go to https://adoptium.net/
2. Click **Latest LTS** (Temurin 21) → choose **Windows** → `.msi` file → **Download**.
3. Run the `.msi` installer.
4. Accept defaults, but **note the install path** — usually:
   `C:\Program Files\Eclipse Adoptium\jdk-21.x.x\`

### Step 2 — Set `JAVA_HOME` (optional but recommended)
1. Open **System Properties** → **Environment Variables** (`Win` + search "environment variables").
2. Under **System variables** → **New**:
   - Variable name: `JAVA_HOME`
   - Variable value: `C:\Program Files\Eclipse Adoptium\jdk-21.x.x\`
3. Edit the `Path` variable → **New** → add: `%JAVA_HOME%\bin`
4. Click **OK** everywhere. Open a **new** terminal and verify:

```powershell
java -version
javac -version
```

You should see output like:

```
openjdk version "21.0.x" ...
```

### Step 3 — Install VS Code (optional)
1. Download from https://code.visualstudio.com/
2. Install the **"Extension Pack for Java"** (by Microsoft) from the Extensions panel (`Ctrl+Shift+X`).
3. Open any `.java` file — IntelliSense and Run buttons appear automatically.

### Step 4 — Install IntelliJ IDEA (recommended for Spring)
1. Download **IntelliJ IDEA Community** from https://www.jetbrains.com/idea/download/
2. Install with default settings. IntelliJ auto-detects your JDK.

---

## 🍎 macOS Installation

### Using Homebrew (recommended)
```bash
# Install Temurin 21 LTS
brew install --cask temurin@21

# Verify
java -version
```

### Using the installer
1. Download the macOS `.pkg` from https://adoptium.net/
2. Run the package installer.
3. Set `JAVA_HOME` in `~/.zshrc`:

```bash
export JAVA_HOME=$(/usr/libexec/java_home -v 21)
export PATH="$JAVA_HOME/bin:$PATH"
```

---

## 🐧 Linux Installation

### Ubuntu / Debian
```bash
# Add Adoptium repository
sudo apt install -y wget apt-transport-https
wget -qO - https://packages.adoptium.net/artifactory/api/gpg/key/public | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/adoptium.gpg
echo "deb https://packages.adoptium.net/artifactory/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/adoptium.list
sudo apt update
sudo apt install temurin-21-jdk
```

### Fedora / RHEL
```bash
sudo dnf install java-21-openjdk-devel
```

### Verify (all distros)
```bash
java -version
javac -version
echo $JAVA_HOME
```

---

## ✅ Verify Your Setup — Write Your First Program

Create a file named `HelloWorld.java`:

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, Java World!");
    }
}
```

### Compile & Run
```powershell
# From the folder containing HelloWorld.java
javac HelloWorld.java      # creates HelloWorld.class
java HelloWorld            # runs the program
```

**Expected output:**
```
Hello, Java World!
```

> **💡 Rule:** The filename **must** match the public class name. `HelloWorld.java` → `public class HelloWorld`.

---

## 🐞 Troubleshooting

| Problem | Fix |
|---------|-----|
| `'java' is not recognized...` (Windows) | Reinstall or add `%JAVA_HOME%\bin` to `Path`, then open a **new** terminal |
| `'javac' is not recognized` but `java` works | JDK not installed — you only have JRE. Install a full JDK |
| `Error: Could not find or load main class HelloWorld` | Wrong class name or you're in the wrong folder. Check `java` matches the class name (not the file) |
| Multiple Java versions installed | Run `where java` (Windows) / `which java` (Linux/mac) and fix `JAVA_HOME` + `Path` order |
| IDE shows red errors but `javac` works | In VS Code, run "Java: Clean Java Language Server Workspace". In IntelliJ, File → Invalidate Caches |

---

## 📌 Next Steps

Your environment is ready. Head to:

- **[Module 01 — Introduction to Java](01-Introduction-to-Java/README.md)** — understand the theory before writing more code
- **[Module 02 — Environment Setup](02-Environment-Setup/README.md)** — deeper IDE setup walkthroughs
- **[Module 04 — Java Basics](04-Java-Basics/README.md)** — variables, data types, and your first real programs