# PR Best Practices

> Populated during Module 10 — GitHub Core

## PR Size

**Rule of thumb**: A PR should be reviewable in one sitting (20-30 minutes).

- **Ideal**: < 400 lines changed
- **Acceptable**: 400–800 lines
- **Too large**: > 800 lines — split it

Why it matters:
- Large PRs get rubber-stamped reviews
- Smaller PRs merge faster, get better feedback
- Easier to revert if something goes wrong

---

## Draft PRs

```bash
gh pr create --draft        # open as draft
gh pr ready                 # mark ready for review
```

Use draft PRs for:
- WIP code you want CI to run on
- Early feedback on approach before finishing
- Cross-team visibility on in-progress work

---

## PR Templates

Create `.github/PULL_REQUEST_TEMPLATE.md`:

```markdown
## What

Brief description of the change.

## Why

Context and motivation — what problem does this solve?

## How

Approach taken, any tradeoffs made.

## Testing

How to verify this works (manual steps or test output).

## Checklist

- [ ] Tests added/updated
- [ ] Docs updated if needed
- [ ] No new security vulnerabilities introduced
```

---

## The PR Review Loop

1. Author opens PR (with description, tests, linked issue)
2. CI runs automatically
3. Reviewer leaves comments — prefer "suggestion" blocks for small changes
4. Author addresses feedback (commits or replies)
5. Reviewer approves
6. Merge (squash, merge commit, or rebase — per team convention)

---

## Merge Strategies on GitHub

| Strategy | Result | When to use |
|----------|--------|------------|
| **Merge commit** | Preserves all commits + adds merge commit | Default; full history |
| **Squash and merge** | One commit per PR on main | Clean main history |
| **Rebase and merge** | All PR commits replayed onto main (no merge commit) | Linear history without squashing |

---

## Notes & Gotchas

<!-- Add your own notes here during Module 10 -->
