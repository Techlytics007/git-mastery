# Module 10: GitHub Core

**Time estimate**: 2-3 hours
**Goal**: Master the gh CLI, the full PR lifecycle, branch protection, and rulesets.

---

## Concepts

- [gh CLI](../../knowledge-base/github/01-gh-cli.md)
- [Branch protection](../../knowledge-base/github/03-branch-protection.md)
- [PR best practices](../../knowledge-base/concepts/09-pr-best-practices.md)

---

## Exercises

### 1. gh CLI fluency

```bash
# Verify auth
gh auth status

# Explore this repo via gh
gh repo view                        # summary
gh repo view --web                  # open in browser

# List recent workflow runs
gh run list

# List PRs (if any)
gh pr list
gh pr status
```

### 2. Full PR lifecycle

```bash
# Create a practice branch
git switch -c feat/github-core-practice
echo "github core practice" > github-practice.txt
git add . && git commit -m "feat: add GitHub core practice file"
git push -u origin feat/github-core-practice

# Create a PR
gh pr create \
  --title "feat: add GitHub core practice" \
  --body "Practice PR for Module 10 — GitHub Core" \
  --draft

# View the PR
gh pr view
gh pr view --web

# Mark ready
gh pr ready

# Check CI status
gh run list
gh run watch   # watch in real-time
```

### 3. Review flow

```bash
# Simulate a review comment response — amend or add a commit
echo "addressed review feedback" >> github-practice.txt
git add . && git commit -m "feat: address review feedback"
git push

# Check PR status
gh pr view
gh pr diff    # show what the PR changes
```

### 4. Set up branch protection on GitHub

Via GitHub UI (Settings → Branches → Add rule for `main`):
- [x] Require a pull request before merging
- [x] Require status checks to pass (add any CI check if you have one)
- [x] Include administrators

Then try to push directly to main:
```bash
git switch main
echo "direct push test" >> README.md
git add . && git commit -m "test: direct push to main"
git push    # should be rejected by branch protection
git reset --soft HEAD~1  # undo the test commit
```

### 5. Create a PR Template

```bash
mkdir -p .github
cat > .github/PULL_REQUEST_TEMPLATE.md << 'EOF'
## What

Brief description of the change.

## Why

Context and motivation.

## Testing

How to verify this works.

## Checklist

- [ ] Tests added/updated
- [ ] Docs updated if needed
EOF

git add .github/PULL_REQUEST_TEMPLATE.md
git commit -m "chore: add PR template"
git push
```

### 6. Merge the PR

```bash
gh pr merge --squash    # squash merge (or --merge for merge commit)
git switch main
git pull                # sync local main
git branch -d feat/github-core-practice
```

---

## Challenge

1. Open a PR, then push an additional commit to it. What does `gh pr view` show? Does GitHub update the PR automatically?
2. Set up a `CODEOWNERS` file that requires your own review for changes to `modules/`. Test it by opening a PR that touches that directory.
3. What's the difference between "Require status checks" and "Require branches to be up to date before merging"? When does the latter matter?

---

## KB Entry After This Module

- Complete [`knowledge-base/github/01-gh-cli.md`](../../knowledge-base/github/01-gh-cli.md)
- Update [`knowledge-base/github/03-branch-protection.md`](../../knowledge-base/github/03-branch-protection.md)
- Add PR patterns to [`knowledge-base/concepts/09-pr-best-practices.md`](../../knowledge-base/concepts/09-pr-best-practices.md)
