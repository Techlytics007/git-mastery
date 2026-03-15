# Remote Commands

> Populated during Module 03 — Remotes

## git remote

```bash
git remote -v                           # list remotes with URLs
git remote add <name> <url>             # add a remote
git remote remove <name>                # remove a remote
git remote rename <old> <new>           # rename a remote
git remote set-url <name> <url>         # change URL of existing remote
git remote show <name>                  # detailed info about a remote
```

**Convention**: `origin` = your primary remote (fork or personal repo), `upstream` = original repo you forked from.

---

## git fetch

```bash
git fetch                   # fetch all remotes
git fetch <remote>          # fetch from specific remote
git fetch --all             # fetch all remotes explicitly
git fetch --prune           # remove remote-tracking branches that no longer exist
git fetch origin main       # fetch just one branch
```

**Key insight**: `fetch` is always safe — it never modifies your working tree or local branches. It updates remote-tracking refs (`origin/main` etc.) only.

---

## git pull

```bash
git pull                    # fetch + merge (default)
git pull --rebase           # fetch + rebase (cleaner history)
git pull --ff-only          # fail if not fast-forward (safest)
git pull origin main        # pull specific branch
```

**Recommendation**: Prefer `git pull --rebase` or `git fetch` + manual merge/rebase. Plain `git pull` can create surprise merge commits.

Configure default:
```bash
git config --global pull.rebase true     # always rebase on pull
git config --global pull.ff only         # only fast-forward pulls
```

---

## git push

```bash
git push                            # push current branch to its upstream
git push -u origin <branch>         # push + set upstream (first push)
git push origin <branch>            # push to specific remote/branch
git push --all                      # push all branches
git push --tags                     # push all tags
git push origin --delete <branch>   # delete remote branch
git push --force-with-lease         # force push safely (checks for upstream changes)
git push --force                    # force push (DANGEROUS — can lose others' work)
```

**Golden rule**: Never use `--force` on shared branches. Use `--force-with-lease` instead — it refuses if someone else has pushed since your last fetch.

---

## Fork Workflow

```bash
# Initial setup
git clone <your-fork-url>
git remote add upstream <original-repo-url>

# Keeping fork in sync
git fetch upstream
git switch main
git merge upstream/main           # or: git rebase upstream/main
git push origin main

# Working on a feature
git switch -c feature/my-feature
# ... make changes ...
git push -u origin feature/my-feature
# Then open PR on GitHub from your fork
```

---

## Notes & Gotchas

<!-- Add your own notes here during Module 03 -->
