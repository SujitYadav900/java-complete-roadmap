# Git Exercises

> Practice the Git concepts from Module 03. Run every command yourself.

## 1. Beginner Exercises

1. Initialize a repository called `git-practice`.
2. Create `hello.txt`, stage it, and commit with the message `Add hello.txt`.
3. Modify `hello.txt`, view `git diff`, stage, and commit `Update greeting`.
4. View the full history with `git log --oneline`.

## 2. Branching Exercises

1. Create and switch to `feature/welcome`.
2. Add `welcome.txt` and commit it.
3. Switch back to `main` and confirm `welcome.txt` is not present.
4. Merge `feature/welcome` into `main` and delete the branch.

## 3. Conflict Exercises

1. On `main`, set `line1 = "main"` in `config.txt` and commit.
2. Create `feature/change`, change the same line to `"feature"`, and commit.
3. Switch to `main` and merge `feature/change` — observe the conflict.
4. Resolve it by keeping both values, then commit the merge.

## 4. Remote Exercises

1. Create a public GitHub repository and push your local repo to it.
2. Clone it into a second folder and add a file there.
3. In the original folder, pull the change and confirm it appears.

## 5. Undo Exercises

1. Make a bad edit and discard it with `git checkout --`.
2. Create a "WIP" commit, then undo it with `git reset --soft HEAD~1`.
3. Stash a half-finished change, switch branches, return, and `git stash pop`.

## 6. Challenge: Recover a lost commit

1. Make two commits, then run `git reset --hard HEAD~1`.
2. Use `git reflog` to find the "lost" commit hash.
3. Recover it with `git reset --hard <hash>` and verify with `git log`.

## 7. Challenge: Clean history with a rebase

1. Create three commits where the middle one is a small typo fix.
2. Use `git rebase -i HEAD~3` to squash the typo fix into the commit before it.
3. Verify a cleaner log with `git log --oneline`.

> **You've mastered Git.** Move to **[04 — Java Basics](../04-Java-Basics/README.md)** — where the real Java journey begins.