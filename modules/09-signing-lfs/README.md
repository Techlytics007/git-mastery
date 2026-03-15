# Module 09: Signing & LFS

**Time estimate**: 1-2 hours
**Goal**: Sign commits with SSH (simpler than GPG), and use git-lfs for binaries.

---

## Concepts

- [Signing and LFS commands](../../knowledge-base/commands/09-signing-lfs.md)

---

## Exercises

### 1. SSH Commit Signing Setup

```bash
# Check your git version — need 2.34+
git --version

# Check you have an SSH key
ls ~/.ssh/id_ed25519.pub   # if missing: ssh-keygen -t ed25519 -C "you@example.com"

# Configure SSH signing
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519.pub
git config --global commit.gpgsign true

# Create the allowed signers file (needed for local verification)
echo "$(git config user.email) $(cat ~/.ssh/id_ed25519.pub)" > ~/.ssh/allowed_signers
git config --global gpg.ssh.allowedSignersFile ~/.ssh/allowed_signers
```

### 2. Make a signed commit

```bash
cd sandbox && mkdir signing-practice && cd signing-practice
git init
echo "signed content" > file.txt && git add .
git commit -m "feat: first signed commit"

# Verify
git log --show-signature
git verify-commit HEAD
```

### 3. Add SSH key to GitHub for signature verification

```bash
# Add your key as a signing key on GitHub (separate from auth key)
gh ssh-key add ~/.ssh/id_ed25519.pub --type signing --title "MacBook signing key"
# Or: github.com/settings/keys → "Signing keys" → Add new

# Push a signed commit — GitHub will show "Verified" badge
git push -u origin HEAD  # (from a branch you've set up)
```

### 4. git-lfs Setup

```bash
# Verify installation
git lfs version

# Create a test repo with LFS
cd sandbox && mkdir lfs-practice && cd lfs-practice
git init
git lfs install  # enable in this repo

# Track a file type
git lfs track "*.bin"
git add .gitattributes
git commit -m "chore: configure LFS tracking for .bin files"

# Create a "large" binary file
dd if=/dev/urandom bs=1024 count=100 > large-file.bin 2>/dev/null
git add large-file.bin
git status             # shows as LFS object

git commit -m "chore: add large binary file"
git lfs ls-files       # shows LFS-tracked files

# See the pointer file
cat large-file.bin     # actual binary — LFS downloads it
git cat-file blob HEAD:large-file.bin  # shows the pointer text
```

---

## Challenge

1. Verify that a commit shows "Verified" on GitHub after enabling SSH signing and adding the key.
2. What's the difference between adding an SSH key as an "authentication key" vs a "signing key" on GitHub?
3. Create a repo with LFS tracking for `*.png`. Add an image, commit, and use `git lfs status` to inspect it. What does the pointer file look like?

---

## KB Entry After This Module

Complete [`knowledge-base/commands/09-signing-lfs.md`](../../knowledge-base/commands/09-signing-lfs.md) with your setup notes.
