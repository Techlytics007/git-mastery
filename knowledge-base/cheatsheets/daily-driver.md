# Daily Driver Cheatsheet

The ~20 commands used 90% of the time.

## Starting Work

```bash
git switch main              # go to main
git pull                     # get latest
git switch -c feat/my-thing  # new feature branch
```

## During Work

```bash
git status -sb               # what's changed? (short)
git diff                     # what exactly changed (unstaged)
git diff --staged            # what's staged for commit

git add -p                   # stage interactively (hunk by hunk)
git add <file>               # stage a file

git commit -m "feat: description"    # commit

git stash                    # save WIP quickly
git stash pop                # restore WIP
```

## Syncing

```bash
git fetch origin             # update remote refs (safe)
git pull --rebase            # pull + rebase (cleaner than merge)
git push -u origin HEAD      # push new branch + set upstream
git push                     # push (after upstream set)
git push --force-with-lease  # push after rebase (safe force)
```

## Reviewing History

```bash
git log --oneline -10        # last 10 commits
git log --oneline --graph --all  # full branch graph
git log -- <file>            # history of one file
git show <sha>               # show a commit's diff
```

## Branches

```bash
git branch -a                # all branches (local + remote)
git switch <branch>          # switch
git branch -d <name>         # delete (safe)
git branch --merged main     # branches already in main
```

## Fixing Mistakes

```bash
git restore <file>           # discard changes to file
git restore --staged <file>  # unstage a file
git commit --amend --no-edit # add staged changes to last commit
git reset --soft HEAD~1      # undo last commit, keep changes staged
git reflog                   # find anything you've lost
```

## GitHub

```bash
gh pr create --draft         # open PR as draft
gh pr view --web             # open PR in browser
gh pr merge --squash --auto  # auto-merge when CI passes
gh run list                  # see CI status
gh run watch                 # watch CI in real-time
```
