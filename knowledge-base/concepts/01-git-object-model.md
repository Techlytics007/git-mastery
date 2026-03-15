# Git Object Model

> Populated during Module 01 — Foundations

## The Four Object Types

Everything in Git is stored as one of four object types in `.git/objects/`:

### 1. Blob
- Stores the **contents** of a file (not the filename)
- Two files with identical content share one blob
- SHA determined by content only

### 2. Tree
- Stores a **directory listing** — maps filenames to blob SHAs (and other tree SHAs)
- Represents one directory at one point in time

### 3. Commit
- Points to one **root tree** (the entire repo snapshot)
- Contains: author, committer, timestamp, message, parent commit SHA(s)
- Two parents = merge commit
- Zero parents = initial commit

### 4. Tag (annotated)
- Points to a commit (or any object) with a name, message, and signature
- Lightweight tags are just refs (not objects)

---

## SHAs

Every object is identified by a **40-character SHA-1 hash** (now transitioning to SHA-256 with `--object-format=sha256`):

```bash
git cat-file -t <sha>     # show object type
git cat-file -p <sha>     # pretty-print object contents
git ls-tree HEAD          # show tree for current commit
git rev-parse HEAD        # show SHA of HEAD
```

---

## The DAG (Directed Acyclic Graph)

Commits form a **DAG** — each commit points to its parent(s):

```
A ← B ← C ← D (main)
         ↑
         E ← F (feature)
```

- History is **immutable** — rewriting creates new objects with new SHAs
- Branches are just **named pointers** to a commit SHA
- `HEAD` points to the current branch (or directly to a commit in detached state)

---

## Inspecting Objects

```bash
# Look inside any object
git cat-file -p HEAD                  # show current commit
git cat-file -p HEAD^{tree}           # show root tree
git cat-file -p HEAD:README.md        # show blob for README.md

# Verify integrity
git fsck                              # verify all objects
git count-objects -v                  # show object database stats
```

---

## Notes & Gotchas

<!-- Add your own notes here during Module 01 -->
