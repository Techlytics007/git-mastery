# Branching Commands

> Populated during Module 02 — Branching

## git branch

```bash
git branch                  # list local branches
git branch -a               # list local + remote tracking branches
git branch -v               # list with last commit
git branch <name>           # create branch (stay on current)
git branch -d <name>        # delete (safe — refuses if unmerged)
git branch -D <name>        # delete (force — even if unmerged)
git branch -m <old> <new>   # rename branch
git branch --merged         # branches already merged into current
git branch --no-merged      # branches not yet merged
```

---

## git switch (modern — prefer over checkout)

```bash
git switch <branch>         # switch to existing branch
git switch -c <name>        # create + switch (replaces: git checkout -b)
git switch -c <name> <remote>/<branch>  # create tracking branch
git switch -               # switch to previous branch (like cd -)
git switch --detach <commit>  # detached HEAD at specific commit
```

**Added**: Git 2.23 (2019). Use `switch` for branches, `restore` for files.

---

## git merge

```bash
git merge <branch>          # merge branch into current
git merge --no-ff <branch>  # always create merge commit (preserves history)
git merge --ff-only <branch>  # fast-forward only (fail if not possible)
git merge --squash <branch> # squash all commits into one (then commit manually)
git merge --abort           # cancel in-progress merge with conflicts
git merge --continue        # continue after resolving conflicts
```

**Merge strategies**:
- **Fast-forward**: Linear history, no merge commit. Clean but loses branch context.
- **--no-ff**: Always creates merge commit. Shows branch structure in log.
- **--squash**: One clean commit on main. Loses individual commit history from branch.

---

## git rebase (basic)

```bash
git rebase <branch>         # rebase current branch onto <branch>
git rebase --abort          # cancel in-progress rebase
git rebase --continue       # continue after resolving conflict
git rebase --skip           # skip current conflicting commit
```

See [06-rebase-advanced.md](06-rebase-advanced.md) for interactive rebase.

---

## Conflict Resolution

When a conflict occurs:
1. `git status` — see which files are conflicted
2. Edit files — look for `<<<<<<<`, `=======`, `>>>>>>>` markers
3. `git add <resolved-file>` — mark as resolved
4. `git merge --continue` or `git commit`

```bash
git mergetool               # open configured merge tool
git checkout --ours <file>  # accept our version entirely
git checkout --theirs <file>  # accept their version entirely
```

---

## Notes & Gotchas

<!-- Add your own notes here during Module 02 -->
