# Advanced Rebase

> Populated during Module 06 — Rebase Mastery

## Interactive Rebase

```bash
git rebase -i HEAD~3        # interactively edit last 3 commits
git rebase -i <commit>      # edit commits after <commit> (exclusive)
git rebase -i --root        # edit all commits including the first
```

### Interactive Rebase Commands (in the editor)

| Command | Short | What it does |
|---------|-------|--------------|
| `pick` | `p` | Keep commit as-is |
| `reword` | `r` | Keep commit, edit message |
| `edit` | `e` | Pause and amend commit content |
| `squash` | `s` | Merge into previous commit, combine messages |
| `fixup` | `f` | Merge into previous commit, discard this message |
| `drop` | `d` | Delete this commit entirely |
| `exec` | `x` | Run shell command after this commit |
| `break` | `b` | Pause here (use `git rebase --continue` to resume) |

---

## Fixup + Autosquash

Mark a commit as a fixup for an earlier one:
```bash
git commit --fixup=<commit-sha>     # creates commit "fixup! original message"
git commit --fixup=HEAD~2           # fixup for 3rd commit back
```

Then automatically squash fixups:
```bash
git rebase -i --autosquash HEAD~5   # fixup! commits auto-sorted + marked `fixup`
```

Configure to always autosquash:
```bash
git config --global rebase.autosquash true
```

---

## Rebase --onto

Move a branch to a new base:
```bash
git rebase --onto <newbase> <upstream> <branch>
```

Example — you branched off `feature-a` but want your commits on `main`:
```bash
git rebase --onto main feature-a my-feature
```

---

## rerere (Reuse Recorded Resolution)

Record conflict resolutions so you don't re-solve the same conflict twice:
```bash
git config --global rerere.enabled true

git rerere                  # show recorded resolutions
git rerere diff             # show current conflict vs recorded resolution
git rerere forget <file>    # forget a specific resolution
```

Once enabled, rerere automatically replays your resolutions when the same conflict recurs (e.g., long-lived feature branches repeatedly rebased).

---

## Notes & Gotchas

- **Golden rule**: Never rebase commits that have been pushed to a shared branch
- Rebase rewrites SHAs — anyone with the old commits will have a diverged history
- `--force-with-lease` required when pushing a rebased branch

<!-- Add your own notes here during Module 06 -->
