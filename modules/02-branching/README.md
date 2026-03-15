# Module 02: Branching

**Time estimate**: 2-3 hours
**Goal**: Master git switch, merge strategies, and conflict resolution.

---

## Concepts

- [Branching model](../../knowledge-base/concepts/03-branching-model.md) — branches as pointers, HEAD
- [Merge vs rebase](../../knowledge-base/concepts/04-merge-vs-rebase.md) — when to use each
- [Branching commands](../../knowledge-base/commands/02-branching.md)

---

## Exercises

### 1. Create and switch branches (use git switch, not checkout)

```bash
cd sandbox && mkdir branching-practice && cd branching-practice
git init
echo "main line" > main.txt && git add . && git commit -m "feat: initial commit"

git switch -c feature/add-login
echo "login form" > login.txt && git add . && git commit -m "feat: add login form"
echo "validation" >> login.txt && git add . && git commit -m "feat: add validation"

git switch main
git log --oneline --graph --all   # see the diverged history
```

### 2. Fast-forward merge

```bash
# feature/add-login is ahead of main with no divergence
git merge feature/add-login       # fast-forward by default
git log --oneline --graph --all   # notice: no merge commit
```

### 3. Merge commit (--no-ff)

```bash
# Reset and try again with --no-ff
git reset --hard HEAD~2           # undo the fast-forward
git merge --no-ff feature/add-login -m "feat: merge login feature"
git log --oneline --graph --all   # notice: merge commit preserved branch history
```

### 4. Create a conflict

```bash
git switch -c feature/update-main
echo "feature says: hello world" > main.txt
git add . && git commit -m "feat: update greeting"

git switch main
echo "main says: goodbye world" > main.txt
git add . && git commit -m "feat: change greeting on main"

git merge feature/update-main     # CONFLICT!
git status                        # see conflicted file
cat main.txt                      # see conflict markers
```

### 5. Resolve the conflict

```bash
# Edit main.txt — remove markers, keep the version you want
# Then:
git add main.txt
git merge --continue
git log --oneline --graph --all
```

### 6. Squash merge

```bash
git switch -c feature/multiple-commits
echo "step 1" > work.txt && git add . && git commit -m "wip: step 1"
echo "step 2" >> work.txt && git add . && git commit -m "wip: step 2"
echo "step 3" >> work.txt && git add . && git commit -m "wip: step 3"

git switch main
git merge --squash feature/multiple-commits
git status                # staged but not committed
git commit -m "feat: add complete work (3 steps)"
git log --oneline         # one clean commit on main
```

### 7. Branch cleanup

```bash
git branch --merged main          # branches already in main
git branch -d feature/add-login   # delete (safe)
git branch -d feature/multiple-commits
git branch -a                     # confirm cleanup
```

---

## Challenge

1. Create two branches that both modify the same file differently. Merge both into main, resolving the conflict with a combined result (not just picking one side).
2. Use `git log --first-parent main` — what does it show vs `git log --graph --all`?
3. What's the difference between `git branch -d` and `git branch -D`? When would you need `-D`?

---

## KB Entry After This Module

- Document your conflict resolution process in [`knowledge-base/commands/02-branching.md`](../../knowledge-base/commands/02-branching.md)
- Add your mental model for merge strategies to [`knowledge-base/concepts/04-merge-vs-rebase.md`](../../knowledge-base/concepts/04-merge-vs-rebase.md)
