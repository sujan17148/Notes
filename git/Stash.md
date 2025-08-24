# Git Stash – Temporary Storage for Changes

## 🔹 What is `git stash`?
- Saves your **uncommitted changes** (staged + unstaged) into a temporary stack.
- Useful when you want to:
  - Switch branches without committing.
  - Pull new changes without pushing incomplete work.
  - Test something quickly without losing progress.

---

## 🔹 Basic Commands

### Save changes
```bash
git stash
```
Stores uncommitted changes and reverts working directory to clean state.

## List stashes
```
git stash list
```
## Apply stash
```
git stash pop
```
---
## 🔹 Practical Workflow Example
You are working on a new feature but need to switch branches.
```
git stash
git checkout main
```
Do some work or pull updates on main.

Return to your feature branch and get your code back:
```
git checkout feature-branch
git stash pop
```
🔹 Tips

Use stash instead of temporary commits to avoid messy history.
```
git stash save "WIP: login form UI"
//You can stash with a message
```
