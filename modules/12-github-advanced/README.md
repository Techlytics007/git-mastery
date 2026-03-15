# Module 12: GitHub Advanced

**Time estimate**: 2-3 hours
**Goal**: Merge Queue, Codespaces, Dependabot, Code Scanning, Projects v2.

---

## Concepts

- [Merge Queue](../../knowledge-base/github/04-merge-queue.md)
- [Codespaces](../../knowledge-base/github/05-codespaces.md)
- [Dependabot](../../knowledge-base/github/06-dependabot.md)
- [Code Scanning](../../knowledge-base/github/07-code-scanning.md)
- [GitHub Projects v2](../../knowledge-base/github/08-github-projects-v2.md)

---

## Exercises

### 1. Set up Dependabot

```bash
mkdir -p .github
```

Create `.github/dependabot.yml`:
```yaml
version: 2
updates:
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
    labels:
      - "dependencies"
```

```bash
git add .github/dependabot.yml
git commit -m "chore: add Dependabot for GitHub Actions"
git push
```

Check in a few days (or immediately via Security tab): Settings → Security → Dependabot alerts.

### 2. Enable Code Scanning

Via GitHub UI: Security tab → Code scanning → Set up → Default

This auto-creates a CodeQL workflow. You can also:
```bash
# View the auto-created workflow
gh run list
```

For manual setup, create `.github/workflows/codeql.yml` — see [`knowledge-base/github/07-code-scanning.md`](../../knowledge-base/github/07-code-scanning.md).

### 3. Explore Codespaces

```bash
# Open this repo in a Codespace
gh codespace create --repo $(gh repo view --json nameWithOwner -q .nameWithOwner)
gh codespace list

# Or just press '.' on the GitHub repo page for instant browser editor
```

Create a devcontainer config:
```bash
mkdir -p .devcontainer
```

Create `.devcontainer/devcontainer.json`:
```json
{
  "name": "Git Mastery",
  "image": "mcr.microsoft.com/devcontainers/base:ubuntu",
  "features": {
    "ghcr.io/devcontainers/features/github-cli:1": {}
  },
  "postCreateCommand": "echo 'Welcome to Git Mastery!'",
  "customizations": {
    "vscode": {
      "extensions": [
        "mhutchie.git-graph",
        "eamodio.gitlens"
      ]
    }
  }
}
```

```bash
git add .devcontainer/
git commit -m "chore: add devcontainer config for Codespaces"
git push
```

### 4. GitHub Projects v2

Via GitHub UI (or `gh project create`):
1. Go to your profile → Projects → New project
2. Choose "Table" template
3. Add custom fields: Priority (single-select: High/Medium/Low), Module (number)
4. Add the modules as items
5. Switch to Board view
6. Switch to Roadmap view

```bash
gh project list
```

### 5. Merge Queue (read + plan)

Review [04-merge-queue.md](../../knowledge-base/github/04-merge-queue.md).

To enable (requires branch protection):
- Settings → General → Merge Queue
- Update branch protection for `main` to require merge queue
- Test: `gh pr merge --auto` adds PR to queue instead of merging immediately

---

## Challenge

1. Set up Dependabot for `github-actions` and trigger it manually (or wait for Monday). What does the resulting PR look like?
2. Enable Code Scanning's default setup. What languages does it detect? Does it find any issues in this repo?
3. Create a GitHub Project with a "Module" number field. Add all 14 modules as items. Create a filtered view showing only modules 01-05.

---

## KB Entry After This Module

Update all five files in [`knowledge-base/github/`](../../knowledge-base/github/) (04 through 08) with your hands-on experience.
