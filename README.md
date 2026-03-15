# Git Mastery

A structured, hands-on curriculum for rebuilding deep Git and GitHub proficiency — covering everything from the object model to GitHub Actions, including all major features added since 2019.

## How to Use This Repo

1. **Work through modules sequentially** — each builds on the last
2. **Update the knowledge base** after each session with what you learned
3. **Use the cheatsheets** as quick references during and after sessions
4. **Use `sandbox/`** for free-form experimentation

---

## Curriculum Overview

| # | Module | Key Topics | Est. Time |
|---|--------|-----------|-----------|
| [00](modules/00-orientation/README.md) | Orientation | Mac git setup, .gitconfig, Homebrew, gh CLI | 30 min |
| [01](modules/01-foundations/README.md) | Foundations | Three areas, add/commit, log, diff, conventional commits | 2-3 hrs |
| [02](modules/02-branching/README.md) | Branching | switch, merge strategies, conflict resolution | 2-3 hrs |
| [03](modules/03-remotes/README.md) | Remotes | fetch vs pull, push -u, force-with-lease, fork workflow | 1-2 hrs |
| [04](modules/04-undoing-recovery/README.md) | Undoing & Recovery | restore, reset levels, revert, reflog rescue | 2-3 hrs |
| [05](modules/05-stash/README.md) | Stash | stash push/pop, untracked files, stash branch | 1-2 hrs |
| [06](modules/06-rebase-mastery/README.md) | Rebase Mastery | interactive rebase, fixup/autosquash, rerere, rebase --onto | 3-4 hrs |
| [07](modules/07-investigation/README.md) | Investigation | bisect, blame, log archaeology, pickaxe (-S) | 1-2 hrs |
| [08](modules/08-modern-git/README.md) | Modern Git | worktrees, sparse-checkout, partial clone, switch/restore | 2-3 hrs |
| [09](modules/09-signing-lfs/README.md) | Signing & LFS | SSH signing (2021+), git-lfs for binaries | 1-2 hrs |
| [10](modules/10-github-core/README.md) | GitHub Core | gh CLI, PR lifecycle, branch protection, rulesets (2023) | 2-3 hrs |
| [11](modules/11-github-actions/README.md) | GitHub Actions | CI workflow, matrix builds, secrets, reusable workflows | 3-4 hrs |
| [12](modules/12-github-advanced/README.md) | GitHub Advanced | Merge Queue, Codespaces, Dependabot, Code Scanning, Projects v2 | 2-3 hrs |
| [13](modules/13-workflows-conventions/README.md) | Workflows & Conventions | Conventional commits + hooks, semver, GitFlow vs trunk | 2-3 hrs |

**Total**: ~28-40 hours across 6-8 weeks

---

## Knowledge Base

Reference material organized by topic:

- **[commands/](knowledge-base/commands/)** — Every command with flags, examples, gotchas
- **[concepts/](knowledge-base/concepts/)** — Mental models and deep explanations
- **[github/](knowledge-base/github/)** — GitHub-specific features and workflows
- **[cheatsheets/](knowledge-base/cheatsheets/)** — Quick references for daily use

**Start here**: [What Changed Since 2010](knowledge-base/cheatsheets/new-commands-2020-2025.md)

---

## Quick Links

- [Daily Driver Cheatsheet](knowledge-base/cheatsheets/daily-driver.md)
- [Emergency Recovery](knowledge-base/cheatsheets/emergency-recovery.md)
- [Merge vs Rebase](knowledge-base/concepts/04-merge-vs-rebase.md)
- [Sandbox](sandbox/README.md)
