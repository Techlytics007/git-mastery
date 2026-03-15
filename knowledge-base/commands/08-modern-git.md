# Modern Git Commands (2019–2024)

> Populated during Module 08 — Modern Git

## git switch (v2.23, 2019)

Replaces the branch-switching part of `git checkout`:

```bash
git switch <branch>             # switch to branch
git switch -c <name>            # create + switch
git switch -c <name> <sha>      # create branch at specific commit
git switch -                    # previous branch
git switch --detach <commit>    # detached HEAD
```

---

## git restore (v2.23, 2019)

Replaces the file-restoration part of `git checkout`:

```bash
git restore <file>              # discard working tree changes
git restore --staged <file>     # unstage (keep working tree)
git restore --source=<commit> <file>  # restore file from specific commit
```

**Why the split?** `git checkout` did two completely different things. Now each has a dedicated, clearer command.

---

## git worktree (v2.5, 2015 — widely adopted ~2020)

Check out multiple branches simultaneously in separate directories:

```bash
git worktree add ../hotfix-branch hotfix/issue-42   # new directory + branch
git worktree add --detach ../review-branch <sha>    # detached HEAD worktree
git worktree list                                   # list all worktrees
git worktree remove ../hotfix-branch                # remove a worktree
git worktree prune                                  # clean up stale worktrees
```

**Use case**: Reviewing a PR while keeping your WIP intact. No stashing, no context switching overhead.

---

## Sparse Checkout (v2.26, 2020 — cone mode v2.27)

Check out only part of a large repository:

```bash
git clone --no-checkout <url>
cd <repo>
git sparse-checkout init --cone          # cone mode: faster, pattern-based
git sparse-checkout set src/api docs/    # check out only these directories
git sparse-checkout add src/utils        # add more directories
git sparse-checkout list                 # show current patterns
git sparse-checkout disable              # restore full checkout
```

**Use case**: Large monorepos where you only work in one area. Dramatically faster checkouts.

---

## Partial Clone (v2.22+, 2019–2020)

Download repo without blobs (file contents) until you access them:

```bash
git clone --filter=blob:none <url>        # blobless clone (metadata + trees only)
git clone --filter=tree:0 <url>           # treeless clone (commits only)
```

**Use case**: Huge repos with large binary history. Only fetch file contents you actually need.

---

## git maintenance (v2.29, 2020)

Background repo optimization:

```bash
git maintenance start           # register repo + start background maintenance
git maintenance stop            # stop background maintenance
git maintenance run             # run manually
git maintenance run --task=gc   # run specific task
```

---

## Notes & Gotchas

<!-- Add your own notes here during Module 08 -->
