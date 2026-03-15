# Emergency Recovery

"I broke something" — flows for the most common disasters.

---

## "I accidentally committed to main"

```bash
# Move last commit to a new branch
git switch -c feat/my-thing   # create branch with the commit
git switch main
git reset --hard HEAD~1       # remove commit from main (local only)
# If already pushed: see "I force-pushed and lost commits" below
```

---

## "I accidentally staged the wrong file"

```bash
git restore --staged <file>   # unstage (keeps your changes in working tree)
```

---

## "I accidentally deleted a file"

```bash
git restore <file>            # recover from last commit
git restore --source=HEAD~2 <file>   # recover older version
```

---

## "I did git reset --hard and lost my work"

```bash
git reflog                    # find the commit SHA before the reset
# Look for "HEAD@{1}: commit: your message"
git reset --hard <sha>        # restore to that point
# Or: create a new branch at that point
git switch -c recovery <sha>
```

---

## "I rebased and now my branch is a mess"

```bash
git reflog show <branch-name>  # find where branch was before rebase
# Look for "checkout: moving from ... to <branch>"
git reset --hard <branch-name>@{1}  # restore branch to pre-rebase state
```

---

## "I can't find a commit I know I made"

```bash
git reflog                    # all HEAD movements for 90 days
git log --all --oneline       # all commits on all branches
git fsck --unreachable        # find dangling commits
```

---

## "I have a merge conflict I want to abort"

```bash
git merge --abort             # cancel merge, return to pre-merge state
git rebase --abort            # cancel rebase
git cherry-pick --abort       # cancel cherry-pick
```

---

## "I pushed a commit with a secret/password"

**This is serious.** The secret is in git history — rotation is required.

1. Revoke/rotate the secret immediately (before doing anything else)
2. Remove from history:
   ```bash
   # Using git filter-repo (recommended — install separately)
   git filter-repo --path secrets.txt --invert-paths
   # Force push all branches
   git push origin --force --all
   ```
3. Notify affected parties
4. GitHub has secret scanning — check if it was already detected

---

## "I deleted a branch with unmerged commits"

```bash
git reflog                    # find the branch tip SHA
git switch -c recovered-branch <sha>
```

---

## Reflog Quick Reference

```bash
git reflog                    # HEAD history
git reflog show main          # branch-specific history
git reflog --relative-date    # show "2 hours ago" format

# Reflog expires after: 90 days (reachable) / 30 days (unreachable)
# Configure: git config gc.reflogExpire 180
```

---

## Nuclear Option: Start Over from Remote

```bash
git fetch origin
git reset --hard origin/main  # DISCARDS all local uncommitted changes
```
