# Module 04: Undoing & Recovery

**Time estimate**: 2-3 hours
**Goal**: Confidently undo anything. Nothing should feel permanently broken.

---

## Concepts

- [Undoing commands](../../knowledge-base/commands/04-undoing.md)
- [Detached HEAD & reflog](../../knowledge-base/concepts/06-detached-head.md)
- [Emergency recovery cheatsheet](../../knowledge-base/cheatsheets/emergency-recovery.md)

---

## Exercises

### 1. git restore — discard working tree changes

```bash
cd sandbox && mkdir undo-practice && cd undo-practice
git init
echo "original" > file.txt && git add . && git commit -m "feat: initial"

echo "unwanted change" >> file.txt
git status
git restore file.txt      # discard change
cat file.txt              # back to original
```

### 2. git restore --staged — unstage

```bash
echo "some work" >> file.txt
git add file.txt
git status                        # staged
git restore --staged file.txt     # unstage (keep changes)
git status                        # unstaged
git diff                          # change still in working tree
```

### 3. git reset — three modes

```bash
# Set up 3 commits
echo "v1" > file.txt && git add . && git commit -m "v1"
echo "v2" > file.txt && git add . && git commit -m "v2"
echo "v3" > file.txt && git add . && git commit -m "v3"
git log --oneline

# --soft: undo commit, keep staged
git reset --soft HEAD~1
git status        # v3 is staged
git log --oneline # v3 commit gone

# --mixed (default): undo commit, unstage
git reset HEAD~1
git status        # v2 is in working tree (unstaged)
git log --oneline # v2 commit gone

# --hard: undo commit + discard changes
git reset --hard HEAD~1
git status        # clean
git log --oneline # back to just v1 commit
cat file.txt      # v1 content
```

### 4. git revert — safe for pushed commits

```bash
echo "feature" > feature.txt && git add . && git commit -m "feat: add feature"
echo "bug" > bug.txt && git add . && git commit -m "fix: but actually introduces bug"
git log --oneline

# Revert the "bug" commit (creates a new commit)
git revert HEAD
git log --oneline   # new revert commit added
ls                  # bug.txt is gone
```

### 5. Reflog — your safety net

```bash
# "Lose" a commit with reset --hard
echo "important work" > important.txt && git add . && git commit -m "feat: important work"
git reset --hard HEAD~1    # "lose" the commit
git log --oneline          # commit appears gone

# Recover it
git reflog                 # find "feat: important work" — note its SHA
git reset --hard <sha>     # or: git switch -c recovery <sha>
git log --oneline          # commit is back!
```

### 6. Detached HEAD practice

```bash
git log --oneline    # note a SHA 3 commits back
git switch --detach HEAD~3
git log --oneline    # you're in the past
echo "experiment" > experiment.txt && git add . && git commit -m "test: experiment"

# Leave detached HEAD
git switch main
git log --oneline --all  # where is your experiment commit?

# Recover it
git reflog               # find the experiment SHA
git switch -c recovered-experiment <sha>
git log --oneline --all  # now it's on a branch and safe
```

---

## Challenge

1. Commit 5 files. Use `git reset --mixed HEAD~5` to undo all 5 commits. Now use `git add -p` to recommit them as 2 thoughtful commits instead.
2. Simulate the "I pushed a bad commit" scenario: commit something, push it, then use `git revert` to undo it (without rewriting history).
3. Use reflog to find a commit from an earlier exercise and restore it.

---

## KB Entry After This Module

- Complete [`knowledge-base/commands/04-undoing.md`](../../knowledge-base/commands/04-undoing.md) with your notes
- Update [`knowledge-base/concepts/06-detached-head.md`](../../knowledge-base/concepts/06-detached-head.md)
- Add any recovery scenarios you discovered to [`knowledge-base/cheatsheets/emergency-recovery.md`](../../knowledge-base/cheatsheets/emergency-recovery.md)
