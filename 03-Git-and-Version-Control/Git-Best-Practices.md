# Git Best Practices

## 1. Introduction

Beyond commands, professional teams follow **conventions** that keep repositories clean, history readable, and collaboration smooth. This topic consolidates those habits.

## 2. Why This Topic Is Important

- Your Git history is a **log of decisions**; a clean history is a debugging and review superpower.
- Code reviews, blame, and releases all depend on disciplined Git use.
- Teams have style guides for Git too — knowing them makes you professional.

## 3. Real-World Analogy

A messy commit history is like a **diary written in pencil with random dates**. A clean history is a well-organized journal where each entry explains what happened and why, so future readers (including future you) can understand it instantly.

## 4. Theory

### The pillars of good Git hygiene

| Pillar | Rule |
|--------|------|
| **Small commits** | One logical change per commit |
| **Clear messages** | Imperative, specific, descriptive |
| **Ignore junk** | `.gitignore` for secrets/build output |
| **Protect main** | Merge only via reviewed pull requests |
| **Stay current** | Pull frequently, resolve early |

### A strong commit message

```text
Add password reset flow

- Email endpoint with token generation
- Password update controller
- Tests for expiry and reuse

Closes #42
```

Summary line ≤ 50 characters, blank line, optional bullet body, optional issue reference.

## 5. Syntax

```bash
git commit -m "Add password reset flow"
git add -p                       # stage parts of a file, not everything
git diff --check                 # catch whitespace errors before committing
```

## 6. Flow Explanation

1. Pull latest `main` (`git pull`).
2. Create a branch named after the task: `feature/reset-password`.
3. Make small, tested commits as you progress.
4. Push the branch and open a pull request.
5. After review and approval, merge and delete the branch.

## 7. Memory Representation

A healthy repo history:

```
main:        A --- B --- C ---------- E
              \           \           /
feature1:      D           F ------- G
feature2:                  
```

Linear-ish main, short-lived feature branches, no force-pushes.

## 8. Simple Example

```bash
git add -p                  # selectively stage hunks
git diff --cached           # review exactly what will be committed
git commit -m "Fix null check in UserService"
```

## 9. Intermediate Example

```bash
cat .gitignore
# output:
*.class
target/
.idea/
*.env
```

```bash
git add .
git status                  # confirm .gitignore excluded junk
git commit -m "Add build config for Maven"
```

## 10. Advanced Example

```bash
# Commit signing for security
git config --global user.signingkey <key>
git commit -S -m "Signed commit"

# Conventional commits
git commit -m "feat(auth): add JWT refresh endpoint"
git commit -m "fix(cart): total now includes tax"
```

## 11. Output

```text
$ git commit -m "fix(cart): total now includes tax"
[feature/cart 9c4e21b] fix(cart): total now includes tax
 1 file changed, 3 insertions(+), 1 deletion(-)
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Committing generated files | Huge noisy repos | `.gitignore` |
| `git add .` without checking | Secrets committed | `git status`/`git diff --cached` first |
| Committing broken code | Broken `main` | Run tests before commit |
| Force-pushing shared branches | Lost team history | Use PRs and `revert` |

## 13. Best Practices

- Write the commit message as an instruction: "Add", "Fix", "Refactor".
- Commit as soon as the code compiles — even a little progress.
- Keep `main` always deployable.
- Delete branches after merging (`git branch -d`).
- Use a team `.gitignore` and keep it in the repo root.

## 14. Interview Questions

**Q: What makes a good commit message?**
A short imperative summary, a blank line, and optionally a body explaining the why, plus issue references.

**Q: Why keep `main` protected?**
So no one pushes broken or unreviewed code; merges go through pull requests with CI checks.

**Q: What should go in `.gitignore`?**
Build output (`target/`, `build/`), IDE settings, secrets (`.env`), OS junk (`.DS_Store`), and dependency caches.

## 15. Practice Exercises

1. Write three commits with conventional style (`feat:`, `fix:`, `refactor:`).
2. Create a `.gitignore` that excludes `.class`, `target/`, and `.env`, and verify with `git status`.
3. Use `git add -p` to stage only part of a file.

## 16. Coding Challenges

1. Refactor an existing 5-commit repo so each commit is a clean, single-purpose unit (use `reset`/`rebase` — locally only).
2. Simulate a code review: open a PR branch, review the diff, and merge via a merge commit.

## 17. Summary

- Small, well-messaged commits keep history readable.
- `.gitignore` keeps junk and secrets out of the repo.
- Protect `main`; merge through pull requests.
- Stay current and resolve conflicts early.
- Git is now part of your daily workflow — next, start writing Java programs in **Module 04 — Java Basics**.