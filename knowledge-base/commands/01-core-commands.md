# Core Commands

> Populated during Module 01 — Foundations

## git init

```bash
git init                    # initialize repo in current directory
git init <directory>        # create directory and initialize
git init --initial-branch=main  # set default branch name (git 2.28+)
```

**What it does**: Creates a `.git/` directory with object store, refs, config, and HEAD.

---

## git clone

```bash
git clone <url>                        # clone into directory named after repo
git clone <url> <directory>            # clone into specific directory
git clone --depth 1 <url>              # shallow clone (latest snapshot only)
git clone --filter=blob:none <url>     # partial clone (no blobs until needed)
git clone --branch <name> <url>        # clone and check out specific branch
```

---

## git add

```bash
git add <file>              # stage a specific file
git add .                   # stage all changes in current directory
git add -p                  # interactive: stage hunks one at a time (VERY useful)
git add -u                  # stage modifications + deletions (not new files)
```

**Key insight**: `git add -p` lets you commit only part of a file's changes. Use it constantly.

---

## git commit

```bash
git commit -m "message"     # commit with inline message
git commit                  # open editor for message
git commit -am "message"    # stage tracked files + commit (skips add for modified)
git commit --amend          # rewrite the last commit (message or content)
git commit --amend --no-edit  # amend without changing message
```

**Amend warning**: Only amend commits that haven't been pushed. Rewrites history.

---

## git status

```bash
git status                  # full status output
git status -s               # short format (M=modified, A=added, ??=untracked)
git status -sb              # short format + branch info
```

---

## git log

```bash
git log                     # full log
git log --oneline           # one line per commit
git log --oneline --graph --all  # visual branch graph (alias: lg)
git log -n 5                # last 5 commits
git log --since="2 weeks ago"
git log --author="name"
git log --grep="keyword"    # search commit messages
git log -S "string"         # pickaxe: commits that added/removed string
git log -- <file>           # history of a specific file
git log <branch1>..<branch2>  # commits in branch2 not in branch1
```

---

## git diff

```bash
git diff                    # working tree vs staging area
git diff --staged           # staging area vs last commit (alias: --cached)
git diff HEAD               # working tree vs last commit
git diff <commit1> <commit2>  # between two commits
git diff <branch1>..<branch2>  # between branch tips
git diff --name-only        # just filenames changed
git diff --stat             # summary with line counts
```

---

## Notes & Gotchas

<!-- Add your own notes here during Module 01 -->
