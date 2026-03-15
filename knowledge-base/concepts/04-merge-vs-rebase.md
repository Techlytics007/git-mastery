# Merge vs Rebase

> One of the most referenced files in this knowledge base.

## The Core Difference

Both integrate changes from one branch into another. They differ in **how they handle history**.

### Merge

Creates a new **merge commit** that ties together two branch histories:

```
Before:
main:    A ← B ← C
feature:          ↑
                  D ← E

After merge:
main:    A ← B ← C ← M   (M = merge commit, has two parents)
                  ↑   ↑
feature:          D ← E
```

**Result**: History is preserved exactly as it happened. You can see when the branch diverged and when it rejoined.

### Rebase

**Replays** feature branch commits on top of the target branch:

```
Before:
main:    A ← B ← C
feature:      ↑
              D ← E

After rebase feature onto main:
main:    A ← B ← C
feature:          ↑
                  D' ← E'   (new commits with new SHAs)
```

**Result**: Linear history — looks like feature was developed after C, not in parallel. Old D and E commits are abandoned.

---

## When to Use Each

### Use Merge When:
- Integrating a completed feature into `main` — preserves branch history
- Working in a team where others have the branch — safe, non-destructive
- The branch history itself is meaningful (parallel work, PR)
- You want a clear record of when branches joined

### Use Rebase When:
- Updating a feature branch with latest `main` — keeps history clean
- Cleaning up messy WIP commits before opening a PR
- You want linear history that's easy to `git bisect`
- **Solo work only** — or on branches you own

---

## The Golden Rule of Rebase

> **Never rebase commits that have been pushed to a shared branch.**

Rebase rewrites SHAs. If others have built on your old commits, they'll have a diverged history that's painful to reconcile.

---

## Fast-Forward Merges

When the feature branch is directly ahead of main (no divergence), merge can "fast-forward" — just move the pointer:

```
Before:
main:    A ← B
feature:      ↑
              C ← D

After fast-forward merge:
main:    A ← B ← C ← D
```

No merge commit created. History is identical to a rebase result.

```bash
git merge --ff-only feature    # succeed only if fast-forward possible
git merge --no-ff feature      # always create a merge commit
```

---

## Practical Workflow (Trunk-Based)

```bash
# On your feature branch, update from main:
git fetch origin
git rebase origin/main        # keep feature branch current

# When feature is done, open a PR
# After PR approval, merge to main (GitHub creates merge commit, or squash-merges)
```

---

## Notes & Gotchas

<!-- Add your own notes here during Module 02 or 06 -->
