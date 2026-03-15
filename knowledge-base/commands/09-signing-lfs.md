# Signing & Git LFS

> Populated during Module 09 — Signing & LFS

## SSH Commit Signing (v2.34, 2021)

Sign commits with your SSH key instead of GPG — much simpler setup.

### Setup

```bash
# Configure signing key (use your existing SSH public key)
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519.pub

# Sign all commits by default
git config --global commit.gpgsign true
git config --global tag.gpgsign true
```

### Creating the Allowed Signers File

GitHub needs this to verify signatures locally:
```bash
# Create allowed signers file
echo "your@email.com $(cat ~/.ssh/id_ed25519.pub)" > ~/.ssh/allowed_signers
git config --global gpg.ssh.allowedSignersFile ~/.ssh/allowed_signers
```

### Verifying Signatures

```bash
git log --show-signature        # show signature status in log
git verify-commit <sha>         # verify a specific commit
```

On GitHub: verified commits show a green "Verified" badge.

---

## git-lfs (Large File Storage)

Store large binary files outside the git object store, replacing them with small pointer files.

### Setup

```bash
brew install git-lfs            # macOS
git lfs install                 # enable in git config (once per machine)
```

### Per-Repo Setup

```bash
git lfs track "*.psd"           # track PSD files
git lfs track "*.mp4"           # track video files
git lfs track "assets/images/**"  # track directory pattern
git add .gitattributes          # MUST commit .gitattributes
git commit -m "chore: configure LFS tracking"
```

### Commands

```bash
git lfs status                  # show LFS files in staging area
git lfs ls-files                # list tracked LFS files in repo
git lfs pull                    # download LFS files for current commit
git lfs fetch --all             # fetch all LFS objects from remote
git lfs migrate import --include="*.psd"  # migrate existing large files to LFS
```

### How It Works

- `git add large-file.psd` → stores actual file on LFS server
- Repo contains a pointer file (120 bytes) instead
- On `git checkout`, LFS automatically downloads the actual file

---

## Notes & Gotchas

- SSH signing requires Git 2.34+ — check with `git --version`
- LFS has storage/bandwidth limits on GitHub free tier (1GB storage, 1GB/month bandwidth)
- LFS objects are stored separately from the repo — cloning doesn't auto-download them unless you have LFS installed

<!-- Add your own notes here during Module 09 -->
