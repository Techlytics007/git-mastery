# GitHub CLI (gh)

> Populated during Module 10 — GitHub Core
> Released: 2020 (GA). Replaces most browser-based GitHub workflows.

## Authentication

```bash
gh auth login               # interactive login (browser or token)
gh auth status              # show current auth state
gh auth logout
gh auth refresh             # re-authenticate
```

---

## Repos

```bash
gh repo create              # interactive repo creation
gh repo create <name> --public --clone
gh repo clone <owner>/<repo>
gh repo view                # view current repo in browser
gh repo view --web
gh repo fork                # fork current repo
gh repo list                # list your repos
```

---

## Pull Requests

```bash
gh pr create                # interactive PR creation
gh pr create --title "feat: add login" --body "..." --draft
gh pr list                  # list open PRs
gh pr view                  # view current branch's PR
gh pr view 42               # view specific PR
gh pr checkout 42           # check out PR locally
gh pr review --approve      # approve current branch's PR
gh pr review 42 --comment -b "Looks good, minor nit below"
gh pr merge                 # interactive merge
gh pr merge --squash --auto # auto-merge when checks pass
gh pr close 42
gh pr diff                  # show PR diff
gh pr status                # show PRs relevant to you
```

---

## Issues

```bash
gh issue create             # interactive issue creation
gh issue list               # list open issues
gh issue view 42
gh issue close 42
gh issue comment 42 -b "Fixed in PR #57"
```

---

## GitHub Actions

```bash
gh run list                 # list recent workflow runs
gh run view                 # view most recent run
gh run view 1234567890      # view specific run
gh run watch                # watch a run in real-time
gh run rerun                # re-run failed run
gh workflow list            # list workflows
gh workflow run <name>      # manually trigger workflow
```

---

## Aliases

```bash
gh alias set prc 'pr create --draft'    # create alias
gh alias list                           # list aliases
```

---

## Notes & Gotchas

<!-- Add your own notes here during Module 10 -->
