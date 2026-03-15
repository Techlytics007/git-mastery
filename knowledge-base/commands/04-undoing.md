# Undoing Changes

> Populated during Module 04 — Undoing & Recovery

## git restore (modern — prefer over checkout for files)

```bash
git restore <file>                  # discard working tree changes (dangerous — unrecoverable)
git restore --staged <file>         # unstage file (keep working tree changes)
git restore --staged --worktree <file>  # unstage + discard changes
git restore --source=HEAD~2 <file>  # restore file to version 2 commits ago
git restore .                       # discard ALL working tree changes
```

**Added**: Git 2.23 (2019). Use `restore` for files, `switch` for branches.

---

## git reset

Reset moves the branch pointer (and optionally staging area / working tree).

```bash
git reset --soft HEAD~1     # undo last commit, keep changes staged
git reset --mixed HEAD~1    # undo last commit, keep changes unstaged (DEFAULT)
git reset --hard HEAD~1     # undo last commit, DISCARD changes (dangerous)
git reset HEAD~3            # undo last 3 commits (mixed)
git reset <commit-sha>      # reset to specific commit
```

**The three modes**:
| Mode | Branch pointer | Staging area | Working tree |
|------|---------------|--------------|--------------|
| `--soft` | moves | unchanged | unchanged |
| `--mixed` (default) | moves | cleared | unchanged |
| `--hard` | moves | cleared | cleared |

**Warning**: `--hard` discards working tree changes permanently (unless reflog saves you — see emergency-recovery.md).

---

## git revert

```bash
git revert <commit>         # create new commit that undoes <commit>
git revert HEAD             # revert the last commit
git revert HEAD~3..HEAD     # revert last 3 commits (creates 3 new commits)
git revert --no-commit <commit>  # stage the revert without committing
git revert -m 1 <merge-commit>  # revert a merge commit (1=mainline parent)
```

**Key distinction**:
- `reset` rewrites history (don't use on pushed commits)
- `revert` adds a new commit (safe for shared/pushed history)

---

## git clean

```bash
git clean -n                # dry run — show what would be removed
git clean -f                # remove untracked files
git clean -fd               # remove untracked files AND directories
git clean -fX               # remove only ignored files
git clean -fdx              # remove everything untracked (including ignored)
```

**Always run `-n` first** to preview what will be deleted.

---

## Notes & Gotchas

<!-- Add your own notes here during Module 04 -->
