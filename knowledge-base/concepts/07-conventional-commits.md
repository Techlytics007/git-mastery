# Conventional Commits

> Populated during Module 13 — Workflows & Conventions
> Reference: https://www.conventionalcommits.org

## Format

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

## Types

| Type | When to use |
|------|------------|
| `feat` | A new feature (correlates with MINOR in semver) |
| `fix` | A bug fix (correlates with PATCH in semver) |
| `docs` | Documentation changes only |
| `style` | Formatting, whitespace — no logic change |
| `refactor` | Code change that neither fixes a bug nor adds a feature |
| `perf` | Performance improvement |
| `test` | Adding or fixing tests |
| `chore` | Maintenance tasks (deps, config, CI) |
| `build` | Changes to build system or external dependencies |
| `ci` | Changes to CI configuration files |
| `revert` | Reverts a previous commit |

## Breaking Changes

Two ways to indicate:
```
feat!: change authentication API          # ! after type

feat(auth): update token format

BREAKING CHANGE: tokens are now JWTs      # footer key
```

Either triggers a MAJOR semver bump.

## Examples

```
feat(auth): add OAuth2 login with Google

fix(cart): prevent duplicate items on rapid click

docs: update README with new env vars

chore(deps): bump lodash from 4.17.20 to 4.17.21

feat!: remove deprecated v1 API endpoints

BREAKING CHANGE: /api/v1/* routes have been removed.
Migrate to /api/v2/* — see migration guide in docs/migration.md.
```

## Scopes

Scopes are optional but recommended for larger repos. Use consistent names:
- `(auth)`, `(api)`, `(ui)`, `(db)`, `(deps)`, `(ci)`

---

## Tools That Use Conventional Commits

- **semantic-release** — auto-generates releases and changelogs
- **release-please** (Google) — creates release PRs automatically
- **commitlint** — enforces convention via git hooks
- **standard-version** — local semver bumping + changelog

---

## Notes & Gotchas

<!-- Add your own notes here during Module 13 -->
