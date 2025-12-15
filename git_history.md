# Git History & Inspection – Understand the Past

This file focuses on **reading and understanding Git history**, not changing it.

---

## View commit history

```bash
# Think of it like: "I list everything that happened"
git log
```

---

## Compact and readable history

```bash
# Think of it like: "I want a clean timeline"
git log --oneline --graph --decorate --all
```

---

## Limit history output

```bash
# Think of it like: "Show me only the last few actions"
git log -5
```

---

## View changes introduced by a commit

```bash
# Think of it like: "What exactly changed here?"
git show HEAD
```

```bash
# Think of it like: "Inspect a specific commit"
git show abc1234
```

---

## Compare changes (diff)

```bash
# Think of it like: "What changed but is not committed yet?"
git diff
```

```bash
# Think of it like: "What changed between two commits?"
git diff abc1234 def5678
```

---

## Compare branches

```bash
# Think of it like: "How far is my branch from main?"
git diff main..feature/my-feature
```

---

## Inspect file history

```bash
# Think of it like: "Show me every change on this file"
git log file.txt
```

---

## Who changed what (line by line)

```bash
# Think of it like: "Who touched this line and when?"
git blame file.txt
```

---

## Search in history

```bash
# Think of it like: "Find commits mentioning a keyword"
git log --grep="login"
```

```bash
# Think of it like: "Find commits that changed this code"
git log -S "password"
```

---

## Golden mindset

```bash
# Think of it like:
# git log   → timeline
# git show  → snapshot
# git diff  → comparison
# git blame → responsibility
```

