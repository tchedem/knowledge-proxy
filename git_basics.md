# Git Basics (CLI Knowledge Base)

> Make sure your CLI is positioned **inside the project folder** before running Git commands.
>
> **OS**: Linux (commands are identical on macOS; Windows via WSL/Git Bash)
>
> **Project folder**: `/home/johndoe/projects/my_incredible_application`

---

## Move into the project directory

```bash
# Think of it like: "I enter the folder where my project lives"
cd /home/johndoe/projects/my_incredible_application
```

---

## Initialize Git in a project

```bash
# Think of it like: "I tell Git to start tracking this folder"
git init
```

---

## Rename the default branch

```bash
# Think of it like: "I rename the main working line of code"
git branch -m main
```

---

## Commit changes in a specific file

```bash
# Think of it like: "I select one file to save its changes"
git add file_changed_or_added.txt

# Think of it like: "I take a snapshot with a message explaining why"
git commit -m "I added the description of the new receipt"
```

---

## Commit all changed or added files

```bash
# Think of it like: "I select EVERYTHING that changed"
git add -A

# Think of it like: "I save a global snapshot of my work"
git commit -m "I added a bunch of receipts"
```

---

## Check tracked files

```bash
# Think of it like: "What files does Git currently know about (index + history)?"
git ls-files

# Think of it like: "What files exist in the latest commit on this branch?"
git ls-tree --full-tree --name-only -r HEAD

# Think of it like: "Same as above, but with more technical details"
git ls-tree -r HEAD
```

---

## Check untracked files

```bash
# Think of it like: "What changed since the last commit?"
git status
```

Example output:
```
On branch new_branch
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        untracked_file.java

nothing added to commit but untracked files present
```

---

## Create a new branch from the current one

```bash
# Think of it like: "I create a parallel timeline to work safely"
git checkout -b new_branch
```

---

## Delete a branch

```bash
# Think of it like: "I remove a branch I no longer need"
git branch -d new_branch
```

---

## Create a new GitHub repository (from scratch)

```bash
# Think of it like: "I create my first file"
echo "# knowledge-proxy" >> README.md

# Think of it like: "I initialize Git"
git init

# Think of it like: "I stage the README"
git add README.md

# Think of it like: "I create the first snapshot"
git commit -m "first commit"

# Think of it like: "I standardize the main branch name"
git branch -M main

# Think of it like: "I link my local repo to GitHub"
git remote add origin git@github.com:tchedem/knowledge-proxy.git

# Think of it like: "I upload my code to GitHub"
git push -u origin main
```

---

## Push an existing repository to GitHub

```bash
# Think of it like: "I connect my local repo to a remote"
git remote add origin git@github.com:tchedem/knowledge-proxy.git

# Think of it like: "I ensure the branch name is main"
git branch -M main

# Think of it like: "I send my commits to GitHub"
git push -u origin main
```

