# Module 11: GitHub Actions

**Time estimate**: 3-4 hours
**Goal**: Write CI workflows, use matrix builds, manage secrets, and create reusable workflows.

---

## Concepts

- [Actions & CI/CD](../../knowledge-base/github/02-actions-ci-cd.md)

---

## Exercises

### 1. Your first workflow

```bash
mkdir -p .github/workflows
```

Create `.github/workflows/ci.yml`:
```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Verify markdown files exist
        run: |
          echo "Checking knowledge base..."
          ls knowledge-base/
          ls knowledge-base/commands/
          ls knowledge-base/concepts/
          echo "All good!"
      - name: Count modules
        run: |
          count=$(ls -d modules/*/ | wc -l)
          echo "Modules found: $count"
```

```bash
git add .github/workflows/ci.yml
git commit -m "ci: add initial CI workflow"
git push

# Watch it run
gh run list
gh run watch
```

### 2. Matrix build

Add to your workflow:
```yaml
  matrix-check:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest]
    steps:
      - uses: actions/checkout@v4
      - name: Show OS info
        run: uname -a
      - name: List files
        run: ls -la
```

```bash
git add .github/workflows/ci.yml
git commit -m "ci: add matrix build across OS"
git push
gh run watch
```

### 3. Secrets and environment variables

Create `.github/workflows/env-demo.yml`:
```yaml
name: Env Demo

on:
  workflow_dispatch:     # manual trigger only

jobs:
  show-env:
    runs-on: ubuntu-latest
    steps:
      - name: Show context info
        run: |
          echo "Branch: ${{ github.ref_name }}"
          echo "Actor: ${{ github.actor }}"
          echo "Event: ${{ github.event_name }}"
          echo "SHA: ${{ github.sha }}"

      - name: Use a secret (safe demo)
        run: echo "Secret is set: ${{ secrets.MY_TEST_SECRET != '' }}"
        # Never echo the actual secret value!
```

Add a test secret:
```bash
gh secret set MY_TEST_SECRET --body "hello-world"
gh secret list

git add .github/workflows/env-demo.yml
git commit -m "ci: add env demo workflow"
git push

gh workflow run env-demo.yml
gh run watch
```

### 4. Reusable workflow

Create `.github/workflows/reusable-lint.yml`:
```yaml
name: Reusable Lint

on:
  workflow_call:
    inputs:
      path:
        required: true
        type: string

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Check files in path
        run: |
          echo "Checking path: ${{ inputs.path }}"
          ls ${{ inputs.path }}
```

Use it in `ci.yml`:
```yaml
  lint-kb:
    uses: ./.github/workflows/reusable-lint.yml
    with:
      path: knowledge-base/
```

---

## Challenge

1. Add a workflow that runs on a schedule (every day at 9am) and posts a reminder comment on any open PR older than 7 days. (Hint: use `gh pr list` with date filtering.)
2. Create a matrix build that tests against 3 different conditions. What happens if one matrix job fails — do the others continue?
3. What is `GITHUB_TOKEN` and how does it differ from a PAT? What can it do by default?

---

## KB Entry After This Module

Complete [`knowledge-base/github/02-actions-ci-cd.md`](../../knowledge-base/github/02-actions-ci-cd.md) with workflow patterns you found useful.
