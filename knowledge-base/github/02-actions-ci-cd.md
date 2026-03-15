# GitHub Actions

> Populated during Module 11 — GitHub Actions
> Launched 2018, GA 2019. Replaced external CI (Travis, CircleCI, Jenkins) for most teams.

## Basic Workflow Structure

`.github/workflows/ci.yml`:

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm test
```

---

## Triggers (on:)

```yaml
on:
  push:
    branches: [main, 'release/**']
    tags: ['v*']
  pull_request:
    types: [opened, synchronize, reopened]
  schedule:
    - cron: '0 9 * * 1'        # 9am every Monday
  workflow_dispatch:             # manual trigger
    inputs:
      environment:
        type: choice
        options: [staging, production]
  workflow_call:                 # reusable workflow trigger
```

---

## Matrix Builds

```yaml
jobs:
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest, windows-latest]
        node: [18, 20, 22]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node }}
      - run: npm test
```

---

## Secrets & Environment Variables

```yaml
env:
  NODE_ENV: production          # workflow-level env var

jobs:
  deploy:
    environment: production     # GitHub Environment (with protection rules)
    steps:
      - run: deploy.sh
        env:
          API_KEY: ${{ secrets.API_KEY }}       # from repo/org secrets
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}  # auto-provided
```

---

## Caching

```yaml
- uses: actions/cache@v4
  with:
    path: ~/.npm
    key: ${{ runner.os }}-npm-${{ hashFiles('**/package-lock.json') }}
    restore-keys: ${{ runner.os }}-npm-
```

---

## Reusable Workflows

**Caller** (`.github/workflows/deploy.yml`):
```yaml
jobs:
  deploy:
    uses: ./.github/workflows/reusable-deploy.yml
    with:
      environment: production
    secrets: inherit
```

**Reusable** (`.github/workflows/reusable-deploy.yml`):
```yaml
on:
  workflow_call:
    inputs:
      environment:
        required: true
        type: string
```

---

## Notes & Gotchas

<!-- Add your own notes here during Module 11 -->
