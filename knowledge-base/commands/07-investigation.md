# Investigation Commands

> Populated during Module 07 — Investigation

## git blame

```bash
git blame <file>                    # show who last changed each line
git blame -L 10,25 <file>           # blame only lines 10-25
git blame -w <file>                 # ignore whitespace changes
git blame -C <file>                 # detect lines moved/copied from other files
git blame --since="3 months ago" <file>
```

**Output format**: `<sha> (<author> <date> <line-number>) <content>`

In GitHub: click any file → "Blame" button for a visual version.

---

## git bisect

Binary search to find which commit introduced a bug:

```bash
git bisect start
git bisect bad                  # current commit is bad
git bisect good <commit>        # last known good commit

# Git checks out midpoint — test, then:
git bisect good                 # this commit is fine
git bisect bad                  # this commit has the bug
# Repeat until git says "X is the first bad commit"

git bisect reset                # return to original HEAD when done
```

### Automated bisect:

```bash
git bisect start HEAD <good-commit>
git bisect run npm test         # runs test; 0=good, non-zero=bad
```

---

## Advanced git log

```bash
git log --all --oneline --graph --decorate  # full visual graph
git log --follow -- <file>      # history of file across renames
git log -p -- <file>            # patch/diff for each commit touching file
git log --stat                  # files changed per commit
git log --diff-filter=D -- <file>  # commits that deleted the file
git log --merges                # only merge commits
git log --no-merges             # exclude merge commits
git log --first-parent          # only main-line commits (skip merge branch)
```

---

## Pickaxe (-S and -G)

Find when a string was added or removed:
```bash
git log -S "functionName"           # commits where count of "functionName" changed
git log -G "regex.*pattern"         # commits where diff matches regex
git log -S "functionName" -p        # show the actual patch
git log -S "functionName" --all     # search all branches
```

**Use case**: "When was this API call removed?" or "Who deleted this config key?"

---

## git shortlog

```bash
git shortlog -sn                # commit count by author
git shortlog -sn --no-merges    # exclude merge commits
git shortlog --since="1 month ago"
```

---

## Notes & Gotchas

<!-- Add your own notes here during Module 07 -->
