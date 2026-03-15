# Module 13: Workflows & Conventions

**Time estimate**: 2-3 hours
**Goal**: Conventional commits, git hooks with commitlint/husky, semver releases, GitFlow vs trunk.

---

## Concepts

- [Conventional commits](../../knowledge-base/concepts/07-conventional-commits.md)
- [GitFlow vs trunk-based](../../knowledge-base/concepts/08-gitflow-vs-trunk.md)

---

## Exercises

### 1. Validate conventional commits with commitlint + husky

```bash
# Initialize npm (needed for husky/commitlint)
cd /Users/ajay/dev/ajay-learn/git-mastery
npm init -y

# Install commitlint + husky
npm install --save-dev @commitlint/{cli,config-conventional} husky

# Configure commitlint
echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js

# Enable husky
npx husky init

# Add commit-msg hook
echo "npx --no -- commitlint --edit \$1" > .husky/commit-msg
chmod +x .husky/commit-msg
```

Test it:
```bash
# Bad commit message — should fail
git commit --allow-empty -m "bad commit message"

# Good commit message — should pass
git commit --allow-empty -m "chore: test commitlint"
```

### 2. Automated releases with release-please

Create `.github/workflows/release-please.yml`:
```yaml
name: Release Please

on:
  push:
    branches: [main]

permissions:
  contents: write
  pull-requests: write

jobs:
  release-please:
    runs-on: ubuntu-latest
    steps:
      - uses: googleapis/release-please-action@v4
        with:
          release-type: simple
```

```bash
git add .github/workflows/release-please.yml
git commit -m "ci: add release-please automation"
git push
```

After pushing several `feat:` and `fix:` commits, release-please will open a PR with an auto-generated CHANGELOG and version bump.

### 3. Pre-push hook to prevent direct main pushes

```bash
cat > .husky/pre-push << 'EOF'
#!/bin/sh
branch=$(git symbolic-ref HEAD | sed -e 's,.*/\(.*\),\1,')
if [ "$branch" = "main" ]; then
  echo "Error: Direct push to main is not allowed. Open a PR instead."
  exit 1
fi
EOF
chmod +x .husky/pre-push
```

```bash
git add .husky/
git commit -m "chore: add pre-push hook to block direct main pushes"
```

### 4. Enforce conventional commits across the team

Add to your `.github/workflows/ci.yml`:
```yaml
  commitlint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npx commitlint --from ${{ github.event.pull_request.base.sha }} --to ${{ github.event.pull_request.head.sha }} --verbose
        if: github.event_name == 'pull_request'
```

### 5. Gitflow vs trunk — reflection exercise

Review [`knowledge-base/concepts/08-gitflow-vs-trunk.md`](../../knowledge-base/concepts/08-gitflow-vs-trunk.md).

In your sandbox, simulate trunk-based development:
```bash
# Short-lived feature branch (< 2 days is the goal)
git switch -c feat/quick-feature
echo "new feature" > quick.txt
git add . && git commit -m "feat: add quick feature"
git push -u origin feat/quick-feature

# Immediately open PR
gh pr create --title "feat: add quick feature" --body "Trunk-based: short-lived branch"

# Merge same day
gh pr merge --squash
git switch main && git pull
git branch -d feat/quick-feature
```

---

## Challenge

1. Configure commitlint to also enforce a scope pattern (e.g., only allow scopes: `core`, `docs`, `ci`, `deps`). Test it.
2. Push 3 `feat:` commits and 2 `fix:` commits. What version bump does release-please propose?
3. Implement a `pre-commit` hook that runs `git diff --check` to block commits with trailing whitespace.

---

## KB Entry After This Module

- Complete [`knowledge-base/concepts/07-conventional-commits.md`](../../knowledge-base/concepts/07-conventional-commits.md)
- Complete [`knowledge-base/concepts/08-gitflow-vs-trunk.md`](../../knowledge-base/concepts/08-gitflow-vs-trunk.md)
