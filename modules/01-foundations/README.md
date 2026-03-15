# Module 01: Foundations

**Time estimate**: 2-3 hours
**Goal**: Deep understanding of the three areas, core commands, and conventional commits.

---

## Concepts

- [The three areas](../../knowledge-base/concepts/02-three-areas.md) — working tree, staging area, repository
- [Git object model](../../knowledge-base/concepts/01-git-object-model.md) — what git actually stores
- [Core commands](../../knowledge-base/commands/01-core-commands.md) — add, commit, status, log, diff

---

## Exercises

### 1. Create a practice project

```bash
cd /Users/ajay/dev/ajay-learn/git-mastery/sandbox
mkdir foundations-practice && cd foundations-practice
git init
```

### 2. Make your first commit

```bash
echo "# Practice Project" > README.md
git status
git add README.md
git status           # notice: README.md is now "staged"
git commit -m "docs: initial README"
git log --oneline
```

**Observe**: What changed in `git status` at each step?

### 3. Explore the three areas

```bash
# Modify, but don't stage
echo "Some content" >> README.md
git status          # "Changes not staged for commit"
git diff            # shows working tree vs staging area

# Stage it
git add README.md
git status          # "Changes to be committed"
git diff            # nothing (working tree == staging area)
git diff --staged   # shows staging area vs last commit

# Commit
git commit -m "docs: add content to README"
git diff --staged   # nothing
```

### 4. Stage interactively (git add -p)

```bash
# Make several changes to one file
cat >> README.md << 'EOF'

## Section A
Content for section A.

## Section B
Content for section B.
EOF

git add -p README.md
# You'll see hunks — press:
#   y = stage this hunk
#   n = skip this hunk
#   s = split hunk further
#   q = quit
```

**Goal**: Commit Section A and Section B in two separate commits.

### 5. Explore git log

```bash
# Add a few more commits first
echo "line 1" > file.txt && git add . && git commit -m "feat: add file.txt"
echo "line 2" >> file.txt && git add . && git commit -m "feat: add second line"
echo "line 3" >> file.txt && git add . && git commit -m "feat: add third line"

git log
git log --oneline
git log --oneline --stat
git log -p              # full diff for each commit (press q to quit)
git log --follow -- file.txt
```

### 6. Explore git diff

```bash
echo "changed" >> file.txt

git diff                         # working tree vs index
git diff HEAD                    # working tree vs last commit
git diff HEAD~1 HEAD             # between two commits
git diff HEAD~2..HEAD -- file.txt  # specific file, range of commits
```

### 7. Inspect objects directly

```bash
git cat-file -t HEAD             # type: commit
git cat-file -p HEAD             # commit contents
git cat-file -p HEAD^{tree}      # tree contents
```

---

## Conventional Commits Practice

For the rest of this module, use conventional commit messages:
```
feat: ...       (new capability)
fix: ...        (bug fix)
docs: ...       (documentation)
chore: ...      (maintenance)
refactor: ...   (no behavior change)
```

---

## Challenge

1. Make 3 changes to `file.txt` — in separate hunks. Commit only the first and third hunks in one commit, and the second hunk in another.
2. Use `git log -S "specific text"` to find which commit added a specific word.
3. Run `git log --oneline --graph --all` (or your `lg` alias) and explain what you see.

---

## KB Entry After This Module

- Add any `git log` flags you find useful to [`knowledge-base/commands/01-core-commands.md`](../../knowledge-base/commands/01-core-commands.md)
- Add any insight about the three areas to [`knowledge-base/concepts/02-three-areas.md`](../../knowledge-base/concepts/02-three-areas.md)
