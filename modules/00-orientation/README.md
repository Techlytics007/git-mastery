# Module 00: Orientation

**Time estimate**: 30 minutes
**Goal**: Mac git setup, modern defaults, and navigate this project confidently.

---

## Concepts

- Why Homebrew git vs system git (Apple ships an old version)
- `.gitconfig` — global configuration, aliases, defaults
- `gh` CLI — GitHub from your terminal

---

## Exercises

### 1. Check your current git version

```bash
git --version
```

macOS ships git 2.24 or older. We want 2.43+ for all modern features.

### 2. Install latest git via Homebrew

```bash
brew install git
```

Then verify Homebrew's git is first in PATH:
```bash
which git          # should show /opt/homebrew/bin/git (not /usr/bin/git)
git --version      # should show 2.43+
```

If still showing `/usr/bin/git`, add to your shell config:
```bash
echo 'export PATH="/opt/homebrew/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### 3. Set up global .gitconfig

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --global core.editor "code --wait"   # VS Code
git config --global pull.rebase true
git config --global rebase.autosquash true
git config --global push.autoSetupRemote true
git config --global fetch.prune true
git config --global rerere.enabled true
```

View your config:
```bash
git config --global --list
cat ~/.gitconfig
```

### 4. Set up aliases

```bash
git config --global alias.lg "log --oneline --graph --all --decorate"
git config --global alias.st "status -sb"
git config --global alias.unstage "restore --staged"
git config --global alias.last "log -1 HEAD --stat"
```

Test them:
```bash
git lg
git st
```

### 5. Install and authenticate gh CLI

```bash
brew install gh
gh auth login     # choose GitHub.com → HTTPS → browser
gh auth status    # should show "Logged in to github.com"
```

### 6. Install git-lfs

```bash
brew install git-lfs
git lfs install   # enable globally
git lfs version
```

### 7. Set up SSH key for GitHub

```bash
# Generate ed25519 key (if you don't have one)
ssh-keygen -t ed25519 -C "you@example.com"

# Add to GitHub
gh ssh-key add ~/.ssh/id_ed25519.pub --title "MacBook"
# Or: https://github.com/settings/keys

# Test
ssh -T git@github.com
```

### 8. Navigate this project

```bash
ls                          # repo root
cat README.md               # curriculum overview
ls knowledge-base/          # reference material
ls modules/                 # hands-on exercises
cat knowledge-base/cheatsheets/new-commands-2020-2025.md  # what's new
```

---

## Challenge

Without looking above:
1. What command shows your full git config?
2. What's the alias you set for `git log --oneline --graph --all`?
3. What is `push.autoSetupRemote` doing for you?

---

## KB Entry After This Module

Update [`knowledge-base/commands/01-core-commands.md`](../../knowledge-base/commands/01-core-commands.md) with any config gotchas you hit.
