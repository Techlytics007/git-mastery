# Merge Queue

> Populated during Module 12 — GitHub Advanced
> GA: May 2023. Solves the "merge race condition" problem.

## The Problem It Solves

Classic CI setup:
1. PR #1 is green against `main@A`
2. PR #2 is green against `main@A`
3. Both merge — but `main@B` (after #1) was never tested with #2's changes
4. `main@C` is broken

Merge Queue solution:
1. Both PRs enter queue
2. Queue tests PR #1 against main
3. Queue tests PR #2 against (main + PR #1)
4. Both merge only if green at their queued position

---

## Setup

Settings → General → Merge Queue → Enable

Requirements:
- Branch protection rule must require merge queue
- CI checks must be configured

---

## How Authors Use It

```bash
# Instead of clicking "Merge", click "Merge when ready" (adds to queue)
gh pr merge --auto          # equivalent via CLI
```

The PR enters the queue, gets tested, and merges automatically when green.

---

## Configuration Options

- **Merge method**: merge commit / squash / rebase
- **Minimum/maximum group size**: batch multiple PRs together
- **Wait timer**: collect PRs for N minutes before running CI
- **Status checks**: which CI checks must pass

---

## Notes & Gotchas

- Requires branch protection to be configured with "Require merge queue"
- The queue is per-branch (one queue for `main`)
- If a PR fails in queue, it's kicked out — fix and re-queue
- PRs can still be merged outside the queue by admins (if protection allows)

<!-- Add your own notes here during Module 12 -->
