# Code Scanning & CodeQL

> Populated during Module 12 — GitHub Advanced
> GA: 2022. Security analysis integrated into PRs.

## What It Does

Scans your code for security vulnerabilities using static analysis. Results appear as PR comments and in the "Security" tab.

---

## Setting Up CodeQL (Default Setup)

Settings → Security → Code scanning → Set up → Default

GitHub automatically:
- Detects languages
- Creates a workflow
- Runs on every push to main and every PR

---

## Advanced Setup (custom workflow)

`.github/workflows/codeql.yml`:

```yaml
name: CodeQL

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  schedule:
    - cron: '30 1 * * 0'    # weekly deep scan

jobs:
  analyze:
    runs-on: ubuntu-latest
    permissions:
      security-events: write
      actions: read
      contents: read

    strategy:
      matrix:
        language: [javascript, python]    # or: go, java, cpp, ruby, swift

    steps:
      - uses: actions/checkout@v4
      - uses: github/codeql-action/init@v3
        with:
          languages: ${{ matrix.language }}
          queries: security-extended    # or: security-and-quality
      - uses: github/codeql-action/autobuild@v3
      - uses: github/codeql-action/analyze@v3
```

---

## SARIF Format

Code scanning results are in SARIF (Static Analysis Results Interchange Format). You can upload results from any scanner:

```yaml
- name: Upload SARIF
  uses: github/codeql-action/upload-sarif@v3
  with:
    sarif_file: results.sarif
```

Integrates: Snyk, Semgrep, ESLint SARIF, Trivy, and many others.

---

## Notes & Gotchas

- Free for public repos; included in GitHub Advanced Security for private repos
- Alerts can be dismissed with a reason (false positive, won't fix)
- Branch protection can require code scanning to pass before merge

<!-- Add your own notes here during Module 12 -->
