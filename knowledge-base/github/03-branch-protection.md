# Branch Protection

> Populated during Module 10 — GitHub Core

## Classic Branch Protection Rules

Settings → Branches → Add rule → Branch name pattern (e.g., `main`)

Common settings:
- **Require a pull request before merging** — no direct pushes to main
- **Require status checks to pass** — CI must be green
- **Require branches to be up to date** — must be current with main before merge
- **Require conversation resolution** — all review comments addressed
- **Require signed commits** — only GPG/SSH-signed commits
- **Include administrators** — rules apply to admins too (recommended)

---

## Repository Rulesets (2023+)

Rulesets replace and extend classic branch protection. Available at org level too.

**Advantages over classic rules**:
- Apply to multiple branches with one ruleset (glob patterns)
- Apply at org level across all repos
- Layered — multiple rulesets can apply to the same branch
- "Evaluate" mode — test rules without enforcing
- Can target tags too (not just branches)

**Via GitHub UI**: Settings → Rules → Rulesets → New branch ruleset

**Via gh CLI**:
```bash
gh api repos/OWNER/REPO/rulesets    # list rulesets
```

**Via API** (example ruleset JSON):
```json
{
  "name": "protect-main",
  "target": "branch",
  "enforcement": "active",
  "conditions": {
    "ref_name": {
      "include": ["refs/heads/main"],
      "exclude": []
    }
  },
  "rules": [
    {"type": "required_status_checks", "parameters": {"required_status_checks": [{"context": "ci"}]}},
    {"type": "pull_request"},
    {"type": "deletion"}
  ]
}
```

---

## CODEOWNERS

File: `.github/CODEOWNERS` (or `CODEOWNERS` in root or `docs/`)

```
# Global owner
*                   @myorg/core-team

# Specific paths
src/auth/           @myorg/security-team
docs/               @myorg/docs-team
*.go                @myorg/backend-team

# Single file
.github/workflows/  @myorg/devops
```

When a PR touches a file, the matching owner is automatically added as a reviewer. Required for "Require review from code owners" protection.

---

## Notes & Gotchas

<!-- Add your own notes here during Module 10 -->
