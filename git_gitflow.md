# GitFlow – Practical Guide

GitFlow is a **branching strategy** that organizes work around clear branch roles.

---

## Main branches

### `main`

- Always stable
- Contains released code only
- Usually associated with version tags (`v1.0.0`, `v1.0.1`, …)

```bash
# Think of it like: "Production snapshots, each one named"
```

### `develop`
```bash
```
- Latest development state
- Base branch for new features

- Latest integration state
- Base branch for all new features
- Can be unstable

```bash
# Think of it like: "Integration branch where features come together" or most simply, "The kitchen during service — messy but productive"
```

---

## Feature branches

- Created from: `develop`
- Merged back into: `develop`
- Short-lived
- Naming: `feature/short-description`

```bash
git checkout -b feature/user-auth develop
```

```bash
# Think of it like: "I borrow tools, build fast, and return them"
```

---

## Release branches

- Created from: `develop`
- Used for stabilization, bug fixes, versioning, documentation
- No new features

```bash
git checkout -b release/1.0.0 develop
```

Tagging a release:
```bash
git tag -a v1.0.0 -m "Release 1.0.0"
```

```bash
# Think of it like: "Final dress rehearsal before the show"
```

Finish a release:
```bash
git checkout main
git merge release/1.0.0

git checkout develop
git merge release/1.0.0

git branch -d release/1.0.0
```

---

## Hotfix branches


- Created from: `main`
- Used for critical production issues
- Must be merged back into `main` and `develop`

```bash
git checkout -b hotfix/1.0.1 main
```

```bash
# Think of it like: "Emergency room surgery — no redesign, just save the patient"
```

Finish a hotfix:
```bash
git checkout main
git merge hotfix/1.0.1

git checkout develop
git merge hotfix/1.0.1

git branch -d hotfix/1.0.1
```

---

## GitFlow tool (CLI)

GitFlow is available as a command-line helper.

Install:
```bash
sudo apt install git-flow
```

Initialize:
```bash
git flow init
```

Default branches:
- `main`
- `develop`

```bash
# Think of it like: "I define the rules once and follow them"
```

---

## GitFlow commands

### Feature
```bash
git flow feature start user-auth
git flow feature finish user-auth
```

```bash
# Think of it like: "Git opens the branch, closes it, and cleans up"
```

---

### Release
```bash
git flow release start 1.0.0
git flow release finish 1.0.0
```

```bash
# Think of it like: "Guided launch procedure"
```

---

### Hotfix
```bash
git flow hotfix start 1.0.1
git flow hotfix finish 1.0.1
```

```bash
# Think of it like: "Apply the patch everywhere consistently"
```

---

## Tags

```bash
git tag -a v1.0.0 -m "Stable release"
git push origin v1.0.0
```

```bash
# Think of it like: "This commit is frozen in time"
```


---

## Tags

```bash
git tag -a v1.0.0 -m "Stable release"
git push origin v1.0.0
```

```bash
# Think of it like: "This commit is frozen in time"
```

---

## GitFlow vs Trunk-Based

| GitFlow | Trunk-Based |
|--------|-------------|
| Structured | Fast |
| Versioned releases | Continuous deployment |
| Multiple branches | Minimal branching |
| Strong process | Lightweight process |

```bash
# Think of it like:
# GitFlow = planned missions
# Trunk   = constant motion
```

---

## Mental model

```bash
main     → stability
develop  → collaboration
feature  → experimentation
release  → preparation
hotfix   → urgency
```

---

## When to use GitFlow

Recommended:
- Long-lived backend systems
- Enterprise or regulated environments
- Versioned APIs

Not recommended:
- Solo projects
- Rapid continuous deployment setups
