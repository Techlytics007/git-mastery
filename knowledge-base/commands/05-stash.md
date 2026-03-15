# Stash Commands

> Populated during Module 05 — Stash

## Core Stash Commands

```bash
git stash                   # stash working tree + staging area (tracked files)
git stash push              # same as above (explicit form)
git stash push -m "message" # stash with a descriptive name
git stash push -u           # include untracked files
git stash push -a           # include untracked + ignored files
git stash push -- <file>    # stash only specific file(s)

git stash list              # list all stashes (stash@{0} is most recent)
git stash show              # show summary of most recent stash
git stash show stash@{2}    # show summary of specific stash
git stash show -p           # show full diff of most recent stash

git stash pop               # apply most recent stash + remove from list
git stash apply             # apply most recent stash, keep in list
git stash apply stash@{2}   # apply specific stash
git stash drop stash@{2}    # delete a specific stash
git stash clear             # delete ALL stashes (no undo!)
```

---

## Stash Branch (Advanced)

```bash
git stash branch <branch-name>          # create branch from stash + apply it
git stash branch <branch-name> stash@{2}  # from specific stash
```

Use this when applying a stash causes conflicts — it creates a new branch at the commit where you stashed, then applies the stash cleanly.

---

## Common Patterns

**Quick context switch**:
```bash
git stash          # save WIP
git switch hotfix-branch
# ... fix the bug, commit ...
git switch -       # back to previous branch
git stash pop      # restore WIP
```

**Stash only staged changes**:
```bash
git stash push --staged   # stash only what's in staging area
```

**Partial stash (specific files)**:
```bash
git stash push -- src/feature.js src/feature.css
```

---

## Notes & Gotchas

- Stashes are stored in `refs/stash` — they survive branch switches and even some destructive operations
- `stash pop` = `stash apply` + `stash drop` — if apply fails due to conflicts, the stash is NOT dropped
- Each stash entry records the branch it was created on (shown in `stash list`)

<!-- Add your own notes here during Module 05 -->
