# What Changed Since You Last Used Git (2019–2024)

The "you definitely didn't know about these" reference.

---

## Git: New Commands & Features

### git switch (v2.23, Aug 2019)
**Replaces**: `git checkout <branch>` and `git checkout -b`

```bash
git switch main              # was: git checkout main
git switch -c feature/foo    # was: git checkout -b feature/foo
git switch -                 # previous branch (same as cd -)
git switch --detach <sha>    # was: git checkout <sha>
```

Why it exists: `checkout` did too many different things (switch branches AND restore files). Now each has a dedicated command.

---

### git restore (v2.23, Aug 2019)
**Replaces**: `git checkout -- <file>` and `git reset HEAD <file>`

```bash
git restore <file>                    # was: git checkout -- <file>
git restore --staged <file>           # was: git reset HEAD <file>
git restore --source=HEAD~2 <file>    # restore file from 2 commits ago
```

---

### git worktree (v2.5, 2015 — widely adopted ~2020)
Work on two branches simultaneously without stashing:

```bash
git worktree add ../hotfix hotfix/critical-bug
# Now you have two working trees — your main dir + ../hotfix
# Each can be on a different branch
git worktree list
git worktree remove ../hotfix
```

**Use case**: You're mid-feature and need to review/test a PR. Add a worktree, no stashing needed.

---

### Sparse Checkout — Cone Mode (v2.26-2.27, 2020)
Check out only part of a large repo:

```bash
git clone --no-checkout <url>
git sparse-checkout init --cone
git sparse-checkout set src/myservice docs/
```

**Use case**: Monorepos where you only work in one service area.

---

### Partial Clone (v2.22+, 2019–2020)

```bash
git clone --filter=blob:none <url>    # skip file contents until accessed
git clone --filter=tree:0 <url>       # skip trees too (commits only)
```

**Use case**: Huge repos with years of large file history. 10x faster clone.

---

### SSH Commit Signing (v2.34, Nov 2021)
Sign commits with SSH instead of GPG — vastly simpler:

```bash
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519.pub
git config --global commit.gpgsign true
```

No GPG keyring, no key servers, no expiry management.

---

### git maintenance (v2.29, Oct 2020)
Background repo optimization (replaces manual `git gc`):

```bash
git maintenance start    # register + schedule background jobs
```

---

### git bisect run (automated bisect)
```bash
git bisect start HEAD <good-sha>
git bisect run npm test    # auto-runs until first bad commit found
```

---

## GitHub: New Features

### GitHub Actions (GA: Nov 2019)
Full CI/CD in `.github/workflows/`. Replaced Travis CI / CircleCI for most teams.
→ See [02-actions-ci-cd.md](../github/02-actions-ci-cd.md)

---

### GitHub CLI — `gh` (GA: Sep 2020)
Full GitHub from the terminal:
```bash
gh pr create --draft
gh pr merge --squash --auto
gh run watch
```
→ See [01-gh-cli.md](../github/01-gh-cli.md)

---

### Codespaces (GA: Nov 2021)
Cloud dev environments. Press `.` on any repo for instant browser editor.
→ See [05-codespaces.md](../github/05-codespaces.md)

---

### GitHub Projects v2 (2022)
Real PM tool with custom fields, roadmap view, automation.
→ See [08-github-projects-v2.md](../github/08-github-projects-v2.md)

---

### Code Scanning / CodeQL (GA: 2022)
Security analysis in PRs. Auto-setup in Settings → Security.
→ See [07-code-scanning.md](../github/07-code-scanning.md)

---

### Repository Rulesets (GA: 2023)
Org-wide branch protection that replaces classic rules:
- Apply to multiple branches with glob patterns
- Works at org level across all repos
→ See [03-branch-protection.md](../github/03-branch-protection.md)

---

### Merge Queue (GA: May 2023)
Eliminates the "merge race condition" — tests PRs in queue order:
→ See [04-merge-queue.md](../github/04-merge-queue.md)

---

### Fine-grained Personal Access Tokens (2022)
Scoped to specific repos and permissions — replaces classic PATs for most automation.

---

## Git Config Defaults Worth Knowing

```bash
# Set once — make modern defaults permanent
git config --global init.defaultBranch main          # new repos default to main
git config --global pull.rebase true                 # pull = fetch + rebase
git config --global rebase.autosquash true           # honor fixup! commits
git config --global push.autoSetupRemote true        # push -u automatically
git config --global rerere.enabled true              # remember conflict resolutions
git config --global fetch.prune true                 # auto-prune stale remote refs
```
