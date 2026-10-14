# Contributing to SmartQueue

This document describes how we work on SmartQueue: branching, commits,
pull requests, and what "Done" means. It implements PRD sections 49
(Git Strategy), 48 (CI/CD) and 50 (Definition of Done).

## 1. Branches

| Branch      | Purpose                                         | Created from | Merged into          |
|-------------|-------------------------------------------------|--------------|----------------------|
| `main`      | Stable, release-ready code                      | -            | -                    |
| `develop`   | Integration branch for completed work           | `main`       | `main`               |
| `feature/*` | New work (features, docs, chores, refactors)    | `develop`    | `develop` (via PR)   |
| `bugfix/*`  | Fixes for bugs found on `develop`               | `develop`    | `develop` (via PR)   |
| `hotfix/*`  | Urgent fixes for `main`                         | `main`       | `main` and `develop` |

Naming: lowercase kebab-case after the prefix, e.g.
`feature/appointment-booking`, `bugfix/duplicate-queue-ticket`.

> The PRD defines the branch names (section 49). The base/target of each
> branch type above, and the hotfix flow, are our working conventions
> because the PRD does not specify them.

Rules:

- No direct commits to `main` or `develop`. Everything goes through a PR.
- One branch = one focused change. Keep PRs small and reviewable.

## 2. Workflow

```bash
git checkout develop
git pull origin develop
git checkout -b feature/<short-description>
# ...work, commit...
git push -u origin feature/<short-description>
# open a Pull Request: base = develop
```

## 3. Commit messages (Conventional Commits)

Format: `<type>: <short imperative description>`

Types we use: `feat`, `fix`, `docs`, `refactor`, `test`, `ci`, `chore`,
`build`, `perf`.

Examples:

```
feat: add appointment booking
fix: prevent duplicate queue tickets
refactor: extract queue policy
test: add booking integration tests
docs: update authentication flow
```

## 4. Pull requests

- Open the PR against `develop` (or `main` for hotfixes).
- The PR title must itself be a Conventional Commit message.
- Fill in the PR template, including the PRD requirements it covers.
- CI must pass before merging.
- The PR must be reviewed before merging.

Merge strategy:

- `feature/*`, `bugfix/*` into `develop`: **Squash and merge**
  (one clean commit per PR, using the PR title as the message).
- `develop` into `main`, and `hotfix/*` into `main`: **Create a merge commit**
  (keeps `main` and `develop` history consistent).

## 5. Definition of Done (PRD section 50)

A feature is NOT complete when the code works locally. It is Done when:

- [ ] Business requirement is satisfied
- [ ] Code follows the architecture (see `docs/architecture`)
- [ ] Validation exists
- [ ] Authorization is implemented
- [ ] Error handling exists
- [ ] Tests exist
- [ ] Logging exists where required
- [ ] Database migration exists if needed
- [ ] API documentation is updated
- [ ] No obvious security issue exists
- [ ] Code is committed
- [ ] Pull Request is created
- [ ] CI passes
- [ ] PR is reviewed
- [ ] Feature is merged

## 6. Architecture rules

Dependencies point inward (ADR-001): `Api` -> `Application` -> `Domain`,
and `Infrastructure` -> `Application`/`Domain`. `Domain` must never
reference Infrastructure or framework-specific packages.
