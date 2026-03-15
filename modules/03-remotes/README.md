# Module 03: Remotes

**Time estimate**: 1-2 hours
**Goal**: Understand fetch vs pull, push safely, and fork workflows.

---

## Concepts

- [Remote commands](../../knowledge-base/commands/03-remotes.md)
- [Remote tracking branches](../../knowledge-base/concepts/05-remote-tracking.md)

---

## Exercises

### 1. Explore remote setup on this repo

```bash
cd /Users/ajay/dev/ajay-learn/git-mastery
git remote -v                     # see origin
git remote show origin            # detailed info
git branch -a                     # local + remote-tracking branches
git branch -vv                    # see tracking relationships
```

### 2. Fetch vs pull

```bash
# Safe: fetch updates remote-tracking refs, nothing local changes
git fetch origin
git log --oneline main..origin/main    # commits on remote, not local (if any)
git log --oneline origin/main..main    # commits local, not on remote

# Inspect before integrating
git diff main origin/main              # see what would change
```

### 3. Practice push

```bash
git switch -c experiment/remotes-practice
echo "remote practice" > remote-test.txt
git add . && git commit -m "test: remotes practice"

# First push: set upstream
git push -u origin experiment/remotes-practice

# Subsequent pushes
echo "more content" >> remote-test.txt
git add . && git commit -m "test: add more content"
git push     # no -u needed after first time

# Verify on GitHub
gh repo view --web
```

### 4. Force push safely after rebase

```bash
# Amend the last commit (rewrites SHA)
git commit --amend -m "test: remotes practice (amended)"

# Regular push fails — remote has old SHA
git push    # rejected!

# Force push with safety check
git push --force-with-lease    # succeeds if no one else pushed
```

### 5. Delete remote branch

```bash
gh pr create --title "test: remotes module" --draft  # optional
git push origin --delete experiment/remotes-practice
git branch -d experiment/remotes-practice
git fetch --prune    # clean up stale remote-tracking refs
```

### 6. Fork workflow simulation

```bash
# Simulate: inspect what upstream vs origin means
git remote -v

# In a real fork workflow you'd have:
# origin = your fork
# upstream = original repo
# git remote add upstream <original-url>
# git fetch upstream
# git rebase upstream/main
```

---

## Challenge

1. Create a branch, push it, then amend its last commit locally. What happens when you try to push? Why? How do you push successfully without risking others' work?
2. What's the difference between `git fetch --prune` and `git remote prune origin`?
3. Explain `origin/main` vs `main` to someone who's never heard of remote-tracking branches.

---

## KB Entry After This Module

- Update [`knowledge-base/commands/03-remotes.md`](../../knowledge-base/commands/03-remotes.md) with gotchas you encountered
- Add insight to [`knowledge-base/concepts/05-remote-tracking.md`](../../knowledge-base/concepts/05-remote-tracking.md)
