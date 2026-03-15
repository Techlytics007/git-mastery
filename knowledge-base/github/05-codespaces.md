# GitHub Codespaces

> Populated during Module 12 — GitHub Advanced
> GA: 2021. Cloud dev environments, zero local setup.

## What It Is

A VS Code instance (or JetBrains, or browser) backed by a cloud VM. The repo is cloned, tools installed, and you're coding in seconds.

---

## Starting a Codespace

```bash
gh codespace create                     # interactive create
gh codespace create --repo owner/repo   # specific repo
gh codespace list                       # list your codespaces
gh codespace code                       # open most recent in VS Code
gh codespace ssh                        # SSH into codespace
gh codespace stop                       # stop (billing stops)
gh codespace delete                     # delete
```

Or: Press `.` on any GitHub repo to open in browser-based editor (no codespace, just github.dev).

---

## devcontainer.json

The configuration file that defines the environment. Lives at `.devcontainer/devcontainer.json`:

```json
{
  "name": "My Project",
  "image": "mcr.microsoft.com/devcontainers/node:20",
  "features": {
    "ghcr.io/devcontainers/features/github-cli:1": {}
  },
  "postCreateCommand": "npm install",
  "forwardPorts": [3000],
  "customizations": {
    "vscode": {
      "extensions": ["dbaeumer.vscode-eslint"],
      "settings": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
      }
    }
  }
}
```

---

## Use Cases

- **Onboarding**: New engineers productive on day one, no local setup
- **PR Review**: Spin up a codespace on a PR to test it, not just read it
- **Cross-platform testing**: Test on Linux even if you develop on Mac
- **Sandboxed experiments**: No risk to local environment

---

## Prebuilds

Pre-build codespaces so they start in < 30 seconds (instead of running setup each time):

Settings → Codespaces → Prebuild configuration → Add prebuild

---

## Notes & Gotchas

- Billed by machine type + usage hours. Free tier: 120 core-hours/month (2-core = 60 hours)
- Codespaces are deleted after 30 days of inactivity by default
- Secrets set in GitHub "Codespaces secrets" are auto-injected as env vars

<!-- Add your own notes here during Module 12 -->
