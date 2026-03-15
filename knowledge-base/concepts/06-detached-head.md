# Detached HEAD

> Populated during Module 04 — Undoing & Recovery

## What It Means

Normally, HEAD points to a branch (which points to a commit):
```
HEAD → main → commit-sha
```

In detached HEAD state, HEAD points **directly to a commit**:
```
HEAD → commit-sha  (no branch in between)
```

---

## How You Get There

```bash
git switch --detach <sha>       # intentional
git checkout <sha>              # legacy way
git checkout v1.2.0             # checking out a tag
git bisect start                # during bisect, git detaches HEAD
```

---

## The Risk

If you make commits in detached HEAD state, those commits belong to no branch:
```
HEAD → new-commit ← your work
```

If you then `git switch main`, the `new-commit` is now unreferenced. Git's garbage collector will eventually delete it.

---

## Recovery

**If you just detached and haven't committed**: Just switch back to a branch.
```bash
git switch main
```

**If you made commits you want to keep**:
```bash
# Option 1: Create a branch right now
git switch -c recovery-branch

# Option 2: You already left detached HEAD — use reflog
git reflog                        # find the SHA of your commit
git switch -c recovery-branch <sha>
```

---

## Reflog (Your Safety Net)

The reflog records every position HEAD has been at, even after "losing" commits:

```bash
git reflog                        # full history of HEAD movements
git reflog show main              # reflog for a specific branch
git reflog --relative-date        # show times ("2 minutes ago")
```

Reflog entries expire after 90 days (default). Within that window, almost nothing is truly lost.

---

## Notes & Gotchas

<!-- Add your own notes here during Module 04 -->
