# Module 05: Stash

**Time estimate**: 1-2 hours
**Goal**: Fluently use stash for context switching without losing work.

---

## Concepts

- [Stash commands](../../knowledge-base/commands/05-stash.md)

---

## Exercises

### 1. Basic stash push/pop

```bash
cd sandbox && mkdir stash-practice && cd stash-practice
git init
echo "main content" > main.txt && git add . && git commit -m "feat: initial"

# Start some work
echo "WIP feature" > feature.txt
echo "changed main" >> main.txt
git status      # 1 modified, 1 untracked

git stash       # stash tracked changes (not untracked by default)
git status      # modified is gone, untracked remains
git stash pop   # restore
```

### 2. Stash with untracked files

```bash
echo "WIP feature" > feature.txt  # untracked
echo "changed main" >> main.txt   # modified

git stash push -u -m "WIP: login feature"   # include untracked
git status      # clean
git stash list  # see your stash

git stash pop   # restore everything
```

### 3. Multiple stashes

```bash
echo "work A" > a.txt && git stash push -m "WIP: work A"
echo "work B" > b.txt && git stash push -m "WIP: work B"
git stash list          # stash@{0}: WIP: work B, stash@{1}: WIP: work A

git stash show stash@{1}          # inspect older stash
git stash apply stash@{1}         # apply specific stash (don't drop)
git stash drop stash@{1}          # remove specific stash
git stash pop                     # apply + drop most recent
```

### 4. Stash a specific file

```bash
echo "change A" >> main.txt
echo "change B" > other.txt

git stash push -- main.txt       # stash only main.txt
git status                        # other.txt still present
git stash pop
```

### 5. Real-world context switch

```bash
# Simulate: you're working on a feature, urgent bug comes in
echo "feature in progress" >> main.txt
git stash push -u -m "WIP: feature - half done"

git switch -c hotfix/critical-bug
echo "bugfix" > fix.txt && git add . && git commit -m "fix: critical bug"
git switch main
git merge --no-ff hotfix/critical-bug -m "fix: merge hotfix"
git branch -d hotfix/critical-bug

# Back to your feature
git stash pop
git status      # WIP restored
```

### 6. Stash branch (when apply conflicts)

```bash
# Make some commits since you stashed
echo "diverged main" >> main.txt && git add . && git commit -m "chore: main moved on"

git stash list   # you have a stash that might conflict

# Instead of apply (which might conflict), create a branch from the stash
git stash branch feature/from-stash stash@{0}
# This creates a branch at the commit where you stashed, then applies cleanly
```

---

## Challenge

1. Create a stash that includes only staged changes (hint: `--staged` flag). What does this let you do?
2. You have 3 stashes. Apply the oldest one without using `stash@{2}` notation (use the message).
3. What happens to the stash if `git stash pop` fails due to a conflict? Is the stash lost?

---

## KB Entry After This Module

Update [`knowledge-base/commands/05-stash.md`](../../knowledge-base/commands/05-stash.md) with patterns you found useful.
