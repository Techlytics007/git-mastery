# Remote Tracking Branches

> Populated during Module 03 — Remotes

## The Distinction

| Name | What it is |
|------|-----------|
| `main` | Your local branch — you can commit to it |
| `origin/main` | Remote-tracking ref — read-only snapshot of remote |

`origin/main` is updated only when you run `git fetch` (or `git pull`). It does NOT auto-update.

---

## How It Works

```bash
# After fetch, origin/main points to latest remote state
# Your local main may be behind:

git fetch origin
git log --oneline main..origin/main      # commits on remote, not local
git log --oneline origin/main..main      # commits local, not on remote (diverged)

# To see visual status:
git status                               # shows "behind by 2 commits"
git log --oneline --graph main origin/main
```

---

## Tracking Configuration

A tracking relationship tells git which remote branch is the "upstream" of a local branch:

```bash
git branch -vv                           # show tracking info for all branches
git branch --set-upstream-to=origin/main main   # set tracking
git push -u origin feature               # push + set tracking in one step
```

With tracking configured:
- `git pull` knows where to pull from (no need for `git pull origin main`)
- `git status` shows ahead/behind info
- `git push` knows where to push to

---

## fetch vs pull

| Command | What happens |
|---------|-------------|
| `git fetch` | Updates `origin/main` (remote-tracking ref). Your `main` unchanged. |
| `git pull` | `fetch` + `merge` (or `rebase` if configured). Your `main` updated. |

**Best practice**: `fetch` first, inspect, then decide how to integrate:
```bash
git fetch origin
git log --oneline HEAD..origin/main   # what's new on remote
git merge origin/main                 # or: git rebase origin/main
```

---

## Notes & Gotchas

<!-- Add your own notes here during Module 03 -->
