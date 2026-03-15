# Module 08: Modern Git

**Time estimate**: 2-3 hours
**Goal**: Use git worktree, sparse checkout, partial clone, and the new switch/restore commands.

---

## Concepts

- [Modern git commands](../../knowledge-base/commands/08-modern-git.md)
- [New commands 2020-2025](../../knowledge-base/cheatsheets/new-commands-2020-2025.md)

---

## Exercises

### 1. git switch and git restore (vs old checkout)

```bash
cd sandbox && mkdir modern-practice && cd modern-practice
git init
echo "content" > file.txt && git add . && git commit -m "feat: initial"

# OLD WAY (still works, but discouraged):
# git checkout -b feature
# git checkout -- file.txt

# NEW WAY:
git switch -c feature/new-way         # create + switch
echo "feature content" > feature.txt && git add . && git commit -m "feat: add feature"

git switch -                           # back to previous branch
git restore --staged --worktree file.txt  # discard all changes to file
```

**Key insight**: `checkout` did too much — `switch` and `restore` each have a single, clear purpose.

### 2. git worktree

```bash
cd /Users/ajay/dev/ajay-learn/git-mastery

# List current worktrees
git worktree list

# Add a worktree for reviewing a branch without stashing your current work
git worktree add ../git-mastery-review main

# In the new worktree:
ls ../git-mastery-review     # it's a real directory with a full checkout
cd ../git-mastery-review
git log --oneline -5

# Go back to main worktree
cd /Users/ajay/dev/ajay-learn/git-mastery

# Remove the worktree
git worktree remove ../git-mastery-review
git worktree list            # back to just one
```

**Use case**: Code review while keeping your WIP — no stashing, no context switching overhead.

### 3. Explore sparse checkout

```bash
# Create a test repo to demonstrate
mkdir /tmp/sparse-test && cd /tmp/sparse-test
git init
mkdir -p src/api src/ui docs
echo "api code" > src/api/index.js
echo "ui code" > src/ui/index.js
echo "docs" > docs/guide.md
git add . && git commit -m "feat: monorepo structure"

# Enable sparse checkout
git sparse-checkout init --cone
git sparse-checkout set src/api      # only check out src/api
ls                                   # only src/api visible
git sparse-checkout add docs/        # add docs
ls
git sparse-checkout list             # show patterns
git sparse-checkout disable          # restore full checkout
cd /Users/ajay/dev/ajay-learn/git-mastery
```

### 4. git maintenance

```bash
# Register this repo for background maintenance
git maintenance start

# Verify it's scheduled
cat .git/config | grep maintenance

# Run manually
git maintenance run --task=gc
```

---

## Challenge

1. Use `git worktree` to have the `main` branch and a feature branch checked out simultaneously. Make a commit in each. How does `git log --oneline --graph --all` look from each worktree?
2. What problem does `git switch --detach` solve vs the old `git checkout <sha>`?
3. Read `git restore --help`. What does `--overlay` do? When would it matter?

---

## KB Entry After This Module

- Complete [`knowledge-base/commands/08-modern-git.md`](../../knowledge-base/commands/08-modern-git.md)
- Note your worktree use cases in the file
