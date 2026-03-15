# The Three Areas

> Populated during Module 01 — Foundations

## Overview

Every file in your project exists in up to three distinct areas:

```
Working Tree  →  Staging Area (Index)  →  Repository (.git)
              git add                  git commit

              ←                        ←
              git restore --staged     git reset --soft
              git restore              git reset --mixed
                                       git reset --hard
```

---

## 1. Working Tree

- The actual files on disk that you see and edit
- Changes here are **not tracked** until you stage them
- `git status` shows these as "Changes not staged for commit"

---

## 2. Staging Area (Index)

- A snapshot of what will go into the **next commit**
- Lives in `.git/index`
- `git add` moves changes from working tree → index
- `git status` shows staged changes as "Changes to be committed"

**Power move**: Stage only part of a file with `git add -p` (interactive hunk selection)

---

## 3. Repository

- The permanent object database in `.git/objects/`
- `git commit` takes the current index and creates a commit object
- History here is immutable (rewrites create new objects)

---

## Practical Implications

| Goal | Command |
|------|---------|
| Discard working tree changes | `git restore <file>` |
| Unstage (keep working tree) | `git restore --staged <file>` |
| Unstage + discard | `git restore --staged --worktree <file>` |
| See working tree vs index | `git diff` |
| See index vs last commit | `git diff --staged` |
| See working tree vs last commit | `git diff HEAD` |

---

## Notes & Gotchas

- New/untracked files exist in the working tree but are completely invisible to Git until staged
- `git commit -a` stages ALL tracked modified files automatically — bypasses index intentionally
- The index is sometimes called the "cache" (hence `--cached` as alias for `--staged`)

<!-- Add your own notes here during Module 01 -->
