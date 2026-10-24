# Git Basics

## 1. Introduction

Git Basics covers the **daily command set**: initializing a repository, staging, committing, checking status, and viewing history. With these commands you can safely version any project.

## 2. Why This Topic Is Important

These are the commands you will type hundreds of times per week. Mastering them (and what each one actually does) makes everything else in Git — branching, remotes, undo — intuitive.

## 3. Real-World Analogy

`git add` is like **putting items in a shopping basket**, `git commit` is like **paying at the checkout** (making the purchase permanent), and `git status` is like glancing at your receipt to see what is still in the basket.

## 4. Theory

### Installing Git

- **Windows:** download from git-scm.com or `winget install --id Git.Git`.
- **macOS:** `brew install git`.
- **Linux:** `sudo apt install git`.

Verify with `git --version`.

### The commands and their roles

| Command | Role |
|---------|------|
| `git init` | Creates a hidden `.git` folder and starts tracking |
| `git status` | Shows modified, staged, and untracked files |
| `git add <file>` | Stages a change |
| `git add .` | Stages all changes |
| `git commit -m "msg"` | Records a snapshot of staged changes |
| `git log` | Shows commit history |
| `git diff` | Shows unstaged changes |

### The staging area

You can stage part of your work and commit in logical pieces. This is why Git separates `add` from `commit`.

## 5. Syntax

```bash
git init
git add hello.txt
git commit -m "Add hello.txt"
git status
git log --oneline
```

## 6. Flow Explanation

1. `git init` creates the repository.
2. Edit or create files → Git reports them as **untracked/modified**.
3. `git add` moves them into the **staging area**.
4. `git commit` freezes the staged snapshot into history.
5. `git log` lets you review the timeline.

## 7. Memory Representation

```
Working Dir --add--> Staging --commit--> History
   hello.txt         hello.txt      commit abc1234
```

Each commit stores a tree of file snapshots plus author, timestamp, and message.

## 8. Simple Example

```bash
git init myapp
cd myapp
echo "public class Hello {}" > Hello.java
git add Hello.java
git commit -m "Add Hello.java"
git status
```

## 9. Intermediate Example

```bash
git add Hello.java Main.java
git status
git commit -m "Add main application classes"
git log --oneline
git diff
```

## 10. Advanced Example

```bash
git commit --amend -m "Better message"     # edit the last commit message
git log --oneline --graph --all
git show HEAD                              # inspect the latest commit
git diff HEAD                              # all changes vs last commit
```

## 11. Output

```text
$ git status
On branch main
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   Hello.java

$ git log --oneline
a1b2c3d Add main application classes
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Committing without checking `git status` | Unwanted files in history | Always review first |
| Huge `git add .` before a commit | Secrets/binaries committed | Use `.gitignore` |
| Bad commit messages (`update`) | Useless history | Be specific |
| Amending pushed commits | Team history corrupted | Never amend shared commits |

## 13. Best Practices

- Commit small, logical units of work.
- Check `git status` and `git diff` before every commit.
- Start messages with a capital verb: `Add`, `Fix`, `Refactor`.
- Commit when the code compiles/runs, not mid-broken-state.

## 14. Interview Questions

**Q: What is the difference between `git add` and `git commit`?**
`git add` moves changes into the staging area; `git commit` permanently records those staged changes in history.

**Q: What does `git status` show?**
The current branch, staged changes, unstaged changes, and untracked files.

**Q: How do you see only the last three commits?**
`git log -3` or `git log --oneline -3`.

## 15. Practice Exercises

1. Initialize a repo and commit a single `.java` file.
2. Modify it, view `git diff`, stage it, and commit with a clear message.
3. Create a second file, stage only one file, commit, and note which is staged.

## 16. Coding Challenges

1. Without looking, run the full cycle for 5 files and end with `git log --oneline` showing exactly 5 commits.
2. Use `git commit --amend` to fix a typo in the last message, then verify with `git log -1`.

## 17. Summary

- `init`, `add`, `commit`, `status`, `log`, `diff` form the core Git loop.
- The staging area lets you craft precise commits.
- Always review before committing.
- Small, well-messaged commits make history readable and revertable.
- Next, learn **Branching-and-Merging.md** to work on features safely.