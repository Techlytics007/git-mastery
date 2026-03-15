# GitHub CLI Quick Reference

```bash
# Auth
gh auth login                   # authenticate
gh auth status                  # check auth

# Repos
gh repo create <name> --public  # create repo
gh repo clone <owner>/<repo>    # clone
gh repo view --web              # open in browser

# Pull Requests
gh pr create                    # create PR (interactive)
gh pr create --draft            # create as draft
gh pr list                      # list open PRs
gh pr view                      # view current branch's PR
gh pr view --web                # open PR in browser
gh pr checkout 42               # check out PR locally
gh pr diff                      # show PR diff
gh pr review --approve          # approve
gh pr review --request-changes -b "reason"
gh pr merge                     # merge (interactive)
gh pr merge --squash --auto     # auto-merge when CI passes
gh pr close 42                  # close PR
gh pr status                    # your PRs at a glance
gh pr ready                     # mark draft PR as ready

# Issues
gh issue create                 # create issue
gh issue list                   # list open issues
gh issue view 42                # view issue
gh issue close 42
gh issue comment 42 -b "text"

# GitHub Actions
gh run list                     # recent workflow runs
gh run view                     # view latest run
gh run watch                    # watch run in real-time
gh run rerun --failed           # re-run failed jobs only
gh workflow list                # list workflows
gh workflow run <name>          # trigger manually

# Codespaces
gh codespace create             # create codespace
gh codespace list               # list codespaces
gh codespace code               # open in VS Code
gh codespace ssh                # SSH in
gh codespace stop               # stop (saves $)

# Releases
gh release create v1.2.0        # create release
gh release list                 # list releases
gh release view v1.2.0

# Secrets
gh secret set MY_SECRET         # set repo secret
gh secret list                  # list secrets (names only)

# Aliases
gh alias set prc 'pr create --draft'
gh alias list
```
