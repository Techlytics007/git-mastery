# Module 07: Investigation

**Time estimate**: 1-2 hours
**Goal**: Find bugs, trace history, and answer "who did this and when?"

---

## Concepts

- [Investigation commands](../../knowledge-base/commands/07-investigation.md)

---

## Exercises

### 1. git blame

```bash
cd sandbox && mkdir investigation-practice && cd investigation-practice
git init

# Create a file with multiple commits
echo "line 1: setup" > app.txt && git add . && git commit -m "feat: initial setup"
echo "line 2: feature" >> app.txt && git add . && git commit -m "feat: add feature"
echo "line 3: bugfix" >> app.txt && git add . && git commit -m "fix: correct bug"
echo "line 4: refactor" >> app.txt && git add . && git commit -m "refactor: clean up"

git blame app.txt
git blame -L 2,3 app.txt      # blame lines 2-3 only
```

### 2. git log archaeology

```bash
# History of a specific file
git log --oneline -- app.txt
git log --oneline -p -- app.txt   # with diffs
git log --oneline --follow -- app.txt  # across renames

# Search commit messages
git log --oneline --grep="feature"
git log --oneline --grep="fix"

# Filter by author/date
git log --oneline --author="$(git config user.name)"
git log --oneline --since="1 hour ago"
```

### 3. Pickaxe — find when a string was added/removed

```bash
# Add and then remove some "secret" content across commits
echo "TODO: remove this API key: abc123" >> app.txt && git add . && git commit -m "chore: add note"
sed -i '' '/TODO/d' app.txt && git add . && git commit -m "chore: remove TODO"

# Find commits where "abc123" count changed
git log --oneline -S "abc123"        # shows both commits
git log --oneline -S "abc123" -p     # with the actual diff
git log --oneline -G "TODO.*key"     # regex search
```

### 4. git bisect — find the commit that introduced a bug

```bash
# Create a history where one commit "introduced a bug"
for i in 1 2 3 4 5 6 7 8; do
  echo "version $i" > version.txt
  if [ $i -eq 5 ]; then
    echo "BUG" >> version.txt   # commit 5 introduces the bug
  fi
  git add . && git commit -m "chore: version $i"
done

git log --oneline

# Now bisect
git bisect start
git bisect bad           # current commit is bad
git bisect good HEAD~7   # 7 commits ago was good

# Git checks out midpoint — check if bug exists:
cat version.txt          # does it contain "BUG"?
git bisect bad           # or: git bisect good
# Repeat until git says "X is the first bad commit"

git bisect reset         # return to original HEAD
```

### 5. Automated bisect

```bash
# Create a test script
cat > test.sh << 'EOF'
#!/bin/bash
grep -q "BUG" version.txt
# returns 0 (success) if BUG found — which means BAD commit
EOF
chmod +x test.sh

git bisect start HEAD HEAD~7
git bisect run ./test.sh     # auto-bisects!
git bisect reset
```

---

## Challenge

1. Use `git log -S` to find every commit in this repo that touched the word "rebase". Paste the output.
2. Create a 20-commit history with the "bug" at commit 11. Use `git bisect run` with a test script to find it automatically. How many commits did bisect check?
3. Use `git shortlog -sn` on this repo — what does it show?

---

## KB Entry After This Module

Update [`knowledge-base/commands/07-investigation.md`](../../knowledge-base/commands/07-investigation.md) with useful log flags and bisect patterns.
