# Undoing Changes

## 1. Introduction

Mistakes are normal — Git's superpower is that almost everything can be undone. This topic covers `checkout`, `reset`, `revert`, and `stash`: the safe ways to discard, rewind, or shelve changes.

## 2. Why This Topic Is Important

- Fear of breaking code disappears when you know how to undo.
- Interviews ask which command to use to "undo a commit" — and why `revert` is safer than `reset` on shared branches.
- You will use `git stash` constantly to switch branches mid-work.

## 3. Real-World Analogy

- **checkout** — flip the file back to its last saved version.
- **reset** — rewind the timeline to an earlier saved moment (discards later moments).
- **revert** — append a new change that undoes an old one, keeping the timeline intact.
- **stash** — put your messy desk contents into a drawer so you can work clean, then take them out later.

## 4. Theory

### Command roles

| Command | Scope | Effect |
|---------|-------|--------|
| `git checkout -- <file>` | Working tree | Discard unstaged changes in that file |
| `git restore <file>` | Working tree | Modern equivalent of checkout |
| `git reset --soft HEAD~1` | History | Undo commit, keep changes staged |
| `git reset --mixed HEAD~1` | History | Undo commit, keep changes unstaged (default) |
| `git reset --hard HEAD~1` | History | Undo commit AND discard changes (dangerous) |
| `git revert <commit>` | History | New commit that inverts the given commit |
| `git stash` | Shelve | Save uncommitted work aside |

### `reset` vs `revert`

- **reset** moves the branch pointer backward — it rewrites history. Only use on unshared commits.
- **revert** creates a NEW commit that cancels an old one — safe to use on pushed/shared history.

## 5. Syntax

```bash
git checkout -- hello.txt
git reset --hard HEAD~1
git revert a1b2c3d
git stash
git stash pop
```

## 6. Flow Explanation

1. **Discard a bad edit:** `git checkout -- <file>` restores the last committed version.
2. **Rewind local history:** `git reset --hard HEAD~2` moves the branch back two commits and drops those changes.
3. **Undo a pushed commit:** `git revert <hash>` creates an inverse commit that you push normally.
4. **Pause mid-work:** `git stash` hides uncommitted changes; `git stash pop` brings them back.

## 7. Memory Representation

```
Before reset --hard HEAD~1:
main: A --- B --- C(HEAD)

After:
main: A --- B(HEAD)        C is gone (unless reflog)
```

`git reflog` records every pointer move and is your last-resort recovery tool.

## 8. Simple Example

```bash
echo "bad change" >> hello.txt
git checkout -- hello.txt      # change discarded
```

## 9. Intermediate Example

```bash
git add hello.txt
git commit -m "WIP"            # accidental commit
git reset --soft HEAD~1        # undo commit, keep staged
git commit -m "Real message"   # re-commit properly
```

## 10. Advanced Example

```bash
git stash -u                    # stash including untracked files
git stash list
git checkout -b feature/fix
# ... work ...
git checkout main
git stash pop                   # restore the stashed work

# Recover a "lost" commit
git reflog
git reset --hard <hash-from-reflog>
```

## 11. Output

```text
$ git revert a1b2c3d
[main 8e7f6a5] Revert "Add broken config"
 1 file changed, 1 deletion(-)
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| `reset --hard` on pushed commits | Rewrites shared history | Use `revert` instead |
| Using `reset --hard` blindly | Loses stashes/commits | Check `git status` first; use reflog |
| Forgetting to `stash pop` | Work "disappears" | It is in the stash; `git stash list` |
| Reverting someone else's commit carelessly | Team conflict | Communicate before reverting |

## 13. Best Practices

- Prefer `revert` over `reset --hard` on any shared branch.
- Use `git checkout --` (or `git restore`) only for files you really want to discard.
- `git stash` with a message: `git stash push -m "WIP login"`.
- Learn `git reflog` — it can recover almost anything.

## 14. Interview Questions

**Q: How do you undo the last commit without losing the changes?**
`git reset --soft HEAD~1` keeps the changes staged; `git reset --mixed HEAD~1` keeps them unstaged.

**Q: Why is `git revert` preferred on shared branches?**
It adds a new commit instead of rewriting history, so teammates' clones stay consistent.

**Q: What is `git stash` used for?**
To temporarily shelve uncommitted changes so you can switch branches or apply a hotfix.

## 15. Practice Exercises

1. Make a file, commit it, edit it badly, and discard the edit with `git checkout --`.
2. Create a "WIP" commit and undo it with `git reset --soft`, then re-commit properly.
3. Stash some work, switch branches, switch back, and `stash pop`.

## 16. Coding Challenges

1. Recover a commit that you "deleted" with `reset --hard` using `git reflog`.
2. Revert a pushed commit and confirm the timeline stays linear and shared-safe.

## 17. Summary

- `checkout/restore` discards uncommitted changes in files.
- `reset` rewinds history — only for local, unshared commits.
- `revert` undoes commits by adding a new commit — safe for shared branches.
- `stash` shelves work temporarily; `reflog` can recover almost anything.
- Next, learn team-wide good habits in **Git-Best-Practices.md**.