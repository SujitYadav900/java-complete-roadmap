# Branching and Merging

## 1. Introduction

A **branch** is an independent line of development. Branches let you build a feature, fix a bug, or experiment in isolation, then **merge** that work back when it is ready. Branching is the heart of team workflows and of feature-driven development.

## 2. Why This Topic Is Important

- Almost every real project uses branches for features, releases, and hotfixes.
- Interviewers ask about branch strategies and conflict resolution.
- Without branches, all teammates edit one fragile shared timeline.

## 3. Real-World Analogy

Branches are like **parallel universes of your document**. You and a teammate each write different chapters in your own universe, then you agree to combine them (merge). If you both rewrote the same sentence differently, you must decide which version survives — that is a conflict.

## 4. Theory

### Core concepts

- **HEAD** — a pointer to the commit you are currently on.
- **Branch pointer** — a label that moves forward as you commit.
- **Merge** — combining the changes of two branches.
- **Conflict** — when the same lines were changed differently on both branches.

### Common branch strategies

| Strategy | Idea |
|----------|------|
| **Feature branches** | One branch per feature, merged when done |
| **Git Flow** | `main` + `develop` + feature/release/hotfix branches |
| **Trunk-based** | Short-lived branches, merged to `main` frequently |

## 5. Syntax

```bash
git branch feature/login        # create
git checkout feature/login      # switch
git checkout -b feature/login   # create + switch (combined)
git merge feature/login         # merge into current branch
```

## 6. Flow Explanation

1. Create a branch from `main`.
2. Make commits on the branch — `main` stays untouched.
3. Switch back to `main`, run `git merge <branch>`.
4. Git fast-forwards or creates a merge commit; resolve conflicts if any.
5. Delete the branch with `git branch -d <branch>`.

## 7. Memory Representation

```
main:    A --- B --- C
                    \
feature:            D --- E
```

After merging `feature` into `main`:

```
main:    A --- B --- C -------- F (merge commit)
                    \         /
feature:            D --- E
```

## 8. Simple Example

```bash
git checkout -b add-readme
echo "# Project" > README.md
git add README.md
git commit -m "Add README"
git checkout main
git merge add-readme
```

## 9. Intermediate Example

```bash
git checkout -b fix/typo
sed -i 's/teh/the/' hello.txt
git add hello.txt
git commit -m "Fix typo"
git checkout main
git merge fix/typo
git branch -d fix/typo
```

## 10. Advanced Example

```bash
# Resolving a conflict
git merge feature/login
# CONFLICT (content): hello.txt
# Edit hello.txt, keep the lines you want, then:
git add hello.txt
git commit -m "Merge feature/login"
```

During a conflict, keep the file open, search for `<<<<<<<`, `=======`, and `>>>>>>>`, and delete the markers.

## 11. Output

A clean merge:

```text
Updating 5a1b2c3..8d9e0f1
Fast-forward
 hello.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Merging before testing | Breaking `main` | Test the branch first |
| Leaving conflicts unresolved | Repository unusable | Finish conflict editing, then commit |
| Deleting branches with unmerged work | Work lost | Use `git branch -d` (lowercase) |
| Long-lived feature branches | Painful merges | Merge frequently, keep branches short |

## 13. Best Practices

- One branch = one logical feature or fix.
- Pull latest `main` into your branch regularly to reduce conflicts.
- Write merge-friendly code: small changes, single responsibility.
- Name branches like `feature/login`, `fix/typo`, `release/v1.2`.

## 14. Interview Questions

**Q: What is a fast-forward merge?**
When the current branch has not diverged, Git simply moves the pointer forward — no merge commit is created.

**Q: How do you resolve a merge conflict?**
Edit the conflicted files, choose the correct content, remove conflict markers, stage them, and commit the merge.

**Q: What is the difference between `git merge` and `git rebase`?**
Merge preserves history as commits joining lines; rebase rewrites history by replaying commits onto a new base (cleaner but can alter shared history).

## 15. Practice Exercises

1. Create `feature/hello` and add a file there; verify `main` is unchanged.
2. Merge it back and delete the branch.
3. Create two branches that edit the same line, merge one into `main`, then merge the other and resolve the conflict.

## 16. Coding Challenges

1. Set up a three-branch workflow (feature → develop → main) and merge two features into develop, then into main.
2. Simulate a conflict, resolve it, and produce a clean `git log --graph` with both branches visible.

## 17. Summary

- Branches isolate work; merges combine it.
- HEAD points to your current branch and commit.
- Conflicts occur when the same lines change differently — resolve them by editing.
- Keep branches short-lived and merge frequently.
- Next, connect your repository to the cloud in **Remote-Repositories.md**.