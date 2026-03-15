# Branching Model

> Populated during Module 02 — Branching

## Branches Are Just Pointers

A branch is nothing more than a **file containing a 40-character SHA**:

```bash
cat .git/refs/heads/main       # shows the SHA of the tip of main
cat .git/HEAD                  # shows "ref: refs/heads/main" (or a SHA in detached state)
```

- Creating a branch = writing a new file in `.git/refs/heads/`
- Switching branches = updating `.git/HEAD`
- Committing = updating the current branch file to the new commit SHA

**Implication**: Branches are extremely cheap — creating one is essentially free.

---

## HEAD

`HEAD` is a special pointer that indicates your current position:

| State | HEAD contains |
|-------|--------------|
| On a branch | `ref: refs/heads/main` (symbolic ref) |
| Detached HEAD | `a3f8b2c...` (direct SHA) |

```bash
git log --oneline -1        # show current HEAD commit
git rev-parse HEAD          # show HEAD's SHA
git symbolic-ref HEAD       # show what branch HEAD points to
```

---

## What Happens When You Commit

1. Git creates a new commit object pointing to the current index (snapshot) and current HEAD as parent
2. The current branch pointer advances to the new commit
3. HEAD stays pointing to the same branch (which now points to the new commit)

```
Before:  main → C3 ← C2 ← C1
After:   main → C4 ← C3 ← C2 ← C1
                ↑
              (new commit)
```

---

## Detached HEAD

When HEAD points directly to a commit (not a branch):

```bash
git switch --detach main     # or: git checkout <sha>
git log --oneline -1         # "HEAD detached at abc1234"
```

**Risk**: If you commit in detached HEAD state, those commits belong to no branch. If you switch away, they'll be garbage collected.

**Recovery**: `git switch -c new-branch` while still in detached HEAD.

---

## Notes & Gotchas

<!-- Add your own notes here during Module 02 -->
