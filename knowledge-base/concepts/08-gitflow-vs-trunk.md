# GitFlow vs Trunk-Based Development

> Populated during Module 13 — Workflows & Conventions

## GitFlow

Defined by Vincent Driessen (2010). Uses multiple long-lived branches:

```
main         ─────────────────────────────────────────────
             ↑ (releases only)                ↑
develop      ──────────────────────────────────────────────
               ↑           ↑
feature/A    ──────        │
feature/B              ────┘
```

**Branches**:
- `main` — production code only, tagged releases
- `develop` — integration branch, staging
- `feature/*` — branches off develop, merges back to develop
- `release/*` — branches off develop, merges to main + develop
- `hotfix/*` — branches off main, merges to main + develop

**Good for**:
- Versioned software (libraries, mobile apps) with scheduled releases
- Teams that need clear separation between release trains
- Strict staging → production pipelines

**Bad for**:
- Web services with continuous deployment
- Small teams — too much ceremony
- Any team deploying more than once a week

---

## Trunk-Based Development

Everyone commits to `main` (the "trunk") frequently. Feature branches are short-lived (hours to 1-2 days):

```
main   A ← B ← C ← D ← E ← F
           ↗       ↗       ↗
feature  B1       D1       F1
(short-lived, merged same day)
```

**Rules**:
- Feature branches max 1-2 days old
- `main` always deployable
- Feature flags hide incomplete features in production
- CI must pass before merge

**Good for**:
- Web services with CI/CD pipelines
- Teams deploying daily or continuously
- Teams practicing DevOps / continuous delivery

**Bad for**:
- Large features that take weeks (use feature flags)
- Teams without good CI coverage

---

## GitHub Flow

A simplified version commonly used with GitHub:
1. Branch off `main`
2. Commit to branch
3. Open PR
4. Review + CI passes
5. Merge to `main`
6. Deploy from `main`

No `develop` branch, no release branches. Simple and effective for most web teams.

---

## Recommendation for This Project

Trunk-based / GitHub Flow — simple, modern, good habits.

---

## Notes & Gotchas

<!-- Add your own notes here during Module 13 -->
