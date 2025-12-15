# GitFlow – Practical Guide

GitFlow is a **branching strategy** that organizes work around clear branch roles.

---

## Main branches

### `main`
```bash
# Think of it like: "Production – what users actually use"
```
- Always stable
- Contains released code only

### `develop`
```bash
# Think of it like: "Integration branch where features come together"
```
- Latest development state
- Base branch for new features

---

## Feature branches

```bash
# Think of it like: "I work on a new idea without breaking others"
git checkout -b feature/user-auth develop
```

- Created from: `develop`
- Merged back into: `develop`
- Naming: `feature/short-description`

Merge example:
```bash
# Think of it like: "I finish my feature and bring it back"
git checkout develop
git merge feature/user-auth
git branch -d feature/user-auth
```

---

## Release branches

```bash
# Think of it like: "I prepare the code for production"
git checkout -b release/1.0.0 develop
```

- Used for final testing, version bump, documentation
- Only bug fixes allowed

Finish a release:
```bash
# Think of it like: "I officially ship the version"
git checkout main
git merge release/1.0.0

git checkout develop
git merge release/1.0.0

git branch -d release/1.0.0
```

---

## Hotfix branches

```bash
# Think of it like: "Production is broken – fix NOW"
git checkout -b hotfix/1.0.1 main
```

- Created from: `main`
- Used for critical production bugs

Finish a hotfix:
```bash
# Think of it like: "I patch production and sync everyone"
git checkout main
git merge hotfix/1.0.1

git checkout develop
git merge hotfix/1.0.1

git branch -d hotfix/1.0.1
```

---

## Visual summary

```text
main
  ↑        ↑
  |        |
release   hotfix
  ↑        ↑
  └── develop ── feature/*
```

---

## When to use GitFlow

```bash
# Think of it like: "My project is structured, versioned, and long-lived"
```

Good fit for:
- Backend systems
- Enterprise projects
- Teams with releases

Not ideal for:
- Very small projects
- Continuous deployment without versions

---

## GitFlow mindset (important)

```bash
# Think of it like:
# main    → stability
# develop → collaboration
# feature → creativity
# release → preparation
# hotfix  → urgency
```

