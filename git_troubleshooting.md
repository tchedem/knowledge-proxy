# Git Troubleshooting – Practical Fixes

This file is a **problem → diagnosis → solution** cheat sheet.
All commands follow the same mental model style used in `git.md`.

---

## Detached HEAD state

### Symptom
```bash
# You see something like:
HEAD detached at abc1234
```

### Meaning
```bash
# Think of it like: "I am looking at a commit, not standing on a branch"
```

### Fix (keep changes)
```bash
# Think of it like: "I create a branch from where I am"
git checkout -b fix-detached-head
```

### Fix (discard changes)
```bash
# Think of it like: "I go back to a safe branch"
git checkout main
```

---

## Undo last commit (not pushed)

### Keep changes (edit commit)
```bash
# Think of it like: "I remove the commit but keep my work"
git reset --soft HEAD~1
```

### Discard changes completely
```bash
# Think of it like: "I erase the commit and the code"
git reset --hard HEAD~1
```

---

## Undo last commit (already pushed)

```bash
# Think of it like: "I create a new commit that cancels the previous one"
git revert HEAD
```

> Safe for shared branches (`main`, `develop`)

---

## Reset vs Revert (important)

```bash
# Think of it like:
# reset  → rewrite history (dangerous if pushed)
# revert → add history (safe for teams)
```

---

## Discard local changes in a file

```bash
# Think of it like: "I reload the file from last commit"
git checkout -- file.txt
```

---

## Discard ALL local changes

```bash
# Think of it like: "I go back to a clean working tree"
git reset --hard
```

---

## Remove untracked files

```bash
# Think of it like: "I delete files Git never tracked"
git clean -f

# Think of it like: "I also remove untracked directories"
git clean -fd
```

---

## Fix: committed to the wrong branch

```bash
# Think of it like: "I move my commit to the correct branch"
git branch temp-branch
git checkout correct-branch
git cherry-pick temp-branch
git branch -d temp-branch
```

---

## Fix merge conflict

### Identify conflicts
```bash
# Think of it like: "Git tells me what is broken"
git status
```

### After manual resolution
```bash
# Think of it like: "I confirm conflicts are fixed"
git add conflicted_file.txt

# Think of it like: "I finish the merge"
git commit
```

---

## Abort a merge

```bash
# Think of it like: "This merge is going wrong, stop everything"
git merge --abort
```

---

## Recover a lost commit

```bash
# Think of it like: "I search Git’s memory"
git reflog
```

```bash
# Think of it like: "I go back to a specific moment"
git checkout abc1234
```

---

## Fix: branch refuses to delete

```bash
# Think of it like: "I force delete a local branch"
git branch -D branch_name
```

---

## Check who changed what

```bash
# Think of it like: "Who touched this line and when"
git blame file.txt
```

---

## Emergency checklist

```bash
# Think of it like:
# 1. git status   → understand state
# 2. git log      → understand history
# 3. git reflog   → recover mistakes
# 4. reset/revert → fix safely
```

---

## Golden rules

```bash
# Think of it like:
# If not pushed → reset is OK
# If pushed     → revert only
# If confused   → STOP and check status
```

