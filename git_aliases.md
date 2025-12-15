# Git Aliases – Work Faster

Aliases turn long commands into **muscle memory shortcuts**.

---

## List existing aliases

```bash
# Think of it like: "What shortcuts do I already have?"
git config --get-regexp alias
```

---

## Basic productivity aliases

```bash
# Think of it like: "Shorter status"
git config --global alias.st status

# Think of it like: "Compact history"
git config --global alias.lg "log --oneline --graph --decorate --all"

# Think of it like: "Quick commit"
git config --global alias.cm "commit -m"
```

---

## Safer aliases

```bash
# Think of it like: "Undo last commit but keep changes"
git config --global alias.undo "reset --soft HEAD~1"
```

---

## Inspection aliases

```bash
# Think of it like: "Who changed what"
git config --global alias.blameall "blame -w"

# Think of it like: "Show last commit"
git config --global alias.last "log -1 HEAD"
```

---

## Cleanup aliases

```bash
# Think of it like: "Remove untracked files safely"
git config --global alias.cleanall "clean -fd"
```

---

## Usage examples

```bash
# Think of it like: "I use shortcuts instead of full commands"
git st
git lg
git cm "Fix login bug"
git undo
```

---

## Where aliases live

```bash
# Think of it like: "My personal Git config"
~/.gitconfig
```

---

## Golden rule

```bash
# Think of it like:
# Aliases save time
# But must stay readable for your future self
```

