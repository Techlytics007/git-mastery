# Module 06: Rebase Mastery

**Time estimate**: 3-4 hours
**Goal**: Interactive rebase, fixup/autosquash, rerere, rebase --onto.

---

## Concepts

- [Advanced rebase](../../knowledge-base/commands/06-rebase-advanced.md)
- [Merge vs rebase](../../knowledge-base/concepts/04-merge-vs-rebase.md)

---

## Exercises

### 1. Basic rebase — update feature branch

```bash
cd sandbox && mkdir rebase-practice && cd rebase-practice
git init
echo "main v1" > main.txt && git add . && git commit -m "feat: initial"

git switch -c feature/work
echo "feature work" > feature.txt && git add . && git commit -m "feat: feature work"
echo "more feature" >> feature.txt && git add . && git commit -m "feat: more work"

# Simulate main moving forward
git switch main
echo "main v2" >> main.txt && git add . && git commit -m "feat: main progressed"

git log --oneline --graph --all   # see diverged history

# Rebase feature onto main
git switch feature/work
git rebase main
git log --oneline --graph --all   # linear history!
```

### 2. Interactive rebase — clean up WIP commits

```bash
git switch -c feature/messy
echo "start" > work.txt && git add . && git commit -m "wip: starting"
echo "progress" >> work.txt && git add . && git commit -m "wip: making progress"
echo "oops typo" >> work.txt && git add . && git commit -m "fix typo"
echo "done" >> work.txt && git add . && git commit -m "wip: almost done"
echo "final" >> work.txt && git add . && git commit -m "feat: finished the thing"

git log --oneline    # messy WIP history

# Clean it up
git rebase -i HEAD~5
# In the editor, use: pick/squash/fixup/reword to make one clean commit
```

**In the editor**, change to:
```
pick <sha> wip: starting
squash <sha> wip: making progress
fixup <sha> fix typo
squash <sha> wip: almost done
reword <sha> feat: finished the thing
```

### 3. Fixup + autosquash

```bash
git switch -c feature/autosquash
echo "main feature code" > feature.txt && git add . && git commit -m "feat: add feature"
echo "unrelated change" > other.txt && git add . && git commit -m "chore: unrelated"
echo "feature fix" >> feature.txt && git add . && git commit -m "feat: add more"

# Oops — I need to add something to the first "feat: add feature" commit
echo "forgot this" >> feature.txt
git add feature.txt
git log --oneline       # find the SHA of "feat: add feature"
git commit --fixup=HEAD~2   # creates "fixup! feat: add feature"

git log --oneline       # see the fixup! commit

# Autosquash squashes fixup! commits automatically
git rebase -i --autosquash HEAD~4
# The fixup commit is auto-sorted and marked `fixup`
```

### 4. rerere (reuse recorded resolution)

```bash
git config rerere.enabled true   # enable if not already set globally

git switch -c feature/rerere
echo "conflict-prone line" > shared.txt && git add . && git commit -m "feat: add shared"

git switch main
echo "main version" > shared.txt && git add . && git commit -m "feat: main changes shared"

git switch feature/rerere
git rebase main       # hit a conflict
# Resolve the conflict in shared.txt
git add shared.txt
git rebase --continue  # rerere records the resolution

# Now if you need to rebase again:
git switch main
echo "main moved again" >> main.txt && git add . && git commit -m "feat: another main change"

git switch feature/rerere
git rebase main       # rerere auto-applies your saved resolution!
```

### 5. rebase --onto

```bash
# Scenario: you branched off feature-a instead of main by mistake
git switch main
echo "main" > base.txt && git add . && git commit -m "feat: base"

git switch -c feature-a
echo "feature A" > a.txt && git add . && git commit -m "feat: feature A"

git switch -c my-feature feature-a   # branched off feature-a
echo "my work" > mine.txt && git add . && git commit -m "feat: my work"

git log --oneline --graph --all

# Transplant my-feature's commits to main (excluding feature-a commits)
git rebase --onto main feature-a my-feature
git log --oneline --graph --all   # my-feature now based on main
```

---

## Challenge

1. Create 6 commits with some typos in messages and some small fixup commits scattered throughout. Use interactive rebase to produce 2 clean, well-messaged commits.
2. Configure `rebase.autosquash true` globally. Create a `--fixup` commit and verify it auto-sorts in interactive rebase.
3. Explain in your own words: what does "Never rebase pushed commits" actually prevent from going wrong?

---

## KB Entry After This Module

- Complete [`knowledge-base/commands/06-rebase-advanced.md`](../../knowledge-base/commands/06-rebase-advanced.md)
- Update [`knowledge-base/concepts/04-merge-vs-rebase.md`](../../knowledge-base/concepts/04-merge-vs-rebase.md) with your practical experience
