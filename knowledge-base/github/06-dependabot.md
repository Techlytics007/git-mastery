# Dependabot

> Populated during Module 12 — GitHub Advanced

## What It Does

Automatically opens PRs to update dependencies when new versions are released (or security vulnerabilities are found).

---

## dependabot.yml

`.github/dependabot.yml`:

```yaml
version: 2
updates:
  # npm packages
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"          # daily, weekly, monthly
      day: "monday"
    target-branch: "main"
    labels:
      - "dependencies"
    reviewers:
      - "myorg/core-team"
    open-pull-requests-limit: 5   # max open PRs at once

  # GitHub Actions
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"

  # Docker
  - package-ecosystem: "docker"
    directory: "/docker"
    schedule:
      interval: "weekly"
```

Supported ecosystems: `npm`, `pip`, `bundler`, `maven`, `gradle`, `cargo`, `go`, `nuget`, `composer`, `docker`, `github-actions`, `terraform`, and more.

---

## Security Alerts vs Version Updates

| Feature | Trigger | Configuration |
|---------|---------|---------------|
| **Security alerts** | CVE published | Auto-enabled for public repos |
| **Version updates** | New package release | Requires `dependabot.yml` |

---

## Auto-merge

Auto-merge patch/minor updates that pass CI:

```yaml
# In .github/workflows/dependabot-auto-merge.yml
name: Auto-merge Dependabot PRs
on: pull_request

jobs:
  auto-merge:
    if: github.actor == 'dependabot[bot]'
    runs-on: ubuntu-latest
    permissions:
      pull-requests: write
      contents: write
    steps:
      - uses: actions/checkout@v4
      - name: Fetch Dependabot metadata
        id: metadata
        uses: dependabot/fetch-metadata@v2
      - name: Auto-merge patch/minor
        if: steps.metadata.outputs.update-type == 'version-update:semver-patch' ||
            steps.metadata.outputs.update-type == 'version-update:semver-minor'
        run: gh pr merge --auto --squash "$PR_URL"
        env:
          PR_URL: ${{ github.event.pull_request.html_url }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

---

## Notes & Gotchas

<!-- Add your own notes here during Module 12 -->
