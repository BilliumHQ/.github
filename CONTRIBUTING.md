# Contributing to Billium

Thanks for contributing. This document covers the conventions every repository under [BilliumHQ](https://github.com/BilliumHQ) follows for branching, commits, pull requests, and releases.

This file lives in [`BilliumHQ/.github`](https://github.com/BilliumHQ/.github) and is automatically inherited as the default `CONTRIBUTING.md` for any BilliumHQ repository that doesn't have its own. Individual repos can override any section by adding a local `CONTRIBUTING.md`.

---

## Branch naming

All Billium repositories use the same prefix scheme for short-lived branches — the kind that branch off `main`, get a pull request, and are deleted on merge. The prefix tells you the *kind* of change at a glance, which matters once you have more than a handful of open PRs.

| Prefix | Use for |
|---|---|
| `feature/<short-desc>` | New user-facing functionality |
| `fix/<short-desc>` | Bug fixes — no new behavior, just correcting existing |
| `chore/<short-desc>` | Refactors, dependency bumps, lint cleanups, code reorganization, anything that doesn't change runtime behavior |
| `ci/<short-desc>` | Build pipelines, GitHub Actions workflows, release scripts, anything in `.github/` |
| `docs/<short-desc>` | Documentation-only changes (READMEs, JSDoc, CHANGELOG entries, MDX content) |
| `release/v<X.Y.Z>` | **Long-lived** release branches, only created when a project ships a new major version and we still need to back-port security fixes to the previous major. Most repos don't have any. |

`<short-desc>` is lowercase, kebab-cased, and short enough to fit comfortably in a terminal column. Examples:

- `feature/multi-currency-invoices`
- `fix/webhook-timeout-on-large-bodies`
- `ci/drop-npm-token`
- `chore/update-prisma-6.2`
- `docs/clarify-pk-sk-distinction`

When in doubt about which prefix fits, `chore/` is the safe fallback. The prefix exists to make `gh pr list` and `git branch -a` scannable, not to be policed.

---

## Commit messages

We follow [Conventional Commits](https://www.conventionalcommits.org/) for the subject line:

```
<type>(<scope>): <subject>
```

| Type | Use for |
|---|---|
| `feat` | New feature |
| `fix` | Bug fix |
| `chore` | Refactor / deps / lint / cleanup (no behavior change) |
| `ci` | Build / pipeline / release infrastructure |
| `docs` | Documentation |
| `refactor` | Code restructure with no behavior change and no bug fix |
| `test` | Adding or updating tests only |
| `perf` | Performance improvement |

**Scope** identifies the affected sub-system (`invoice`, `webhook`, `auth`, `release`, `prisma`, etc.). Optional but encouraged for anything non-trivial.

**Subject** is lowercase, imperative mood ("add" not "adds" or "added"), with no trailing period, and ≤ 72 characters.

Examples:

- `feat(invoice): add Idempotency-Key support to POST /invoices`
- `fix(invoice): cancel returns full DTO with relations`
- `ci(release): drop NPM_TOKEN, rely on trusted publishing only`
- `chore(prisma): regenerate client after schema changes`
- `docs(sdk): align README key prefix examples with backend reality`

**Breaking changes** get a `!` after the type and a `BREAKING CHANGE:` paragraph in the body explaining the migration:

```
feat(api)!: rename merchantId to accountId

BREAKING CHANGE: The `merchantId` field on Invoice is now `accountId`.
Update any code that reads `invoice.merchantId` to `invoice.accountId`.
```

The body of the commit (anything after the subject) is optional but encouraged for non-trivial changes. Explain *why*, not *what* — the diff already shows the *what*.

For PRs that bundle multiple commits, the **squash merge subject** should follow the same Conventional Commits convention. That subject is the entry that lands on `main`.

---

## Pull requests

1. **One logical change per PR.** If you find yourself writing "and also" in the description, split the PR.
2. **Open the PR early** — draft PRs are fine and encouraged. They keep collaborators informed of in-flight work.
3. **Title** matches the squash merge subject (Conventional Commits format).
4. **Body** explains the *why* in 1-3 sentences, lists *what* changed in bullets, and documents the *test plan* even if it's just "ran lint+tests locally, will verify CI".
5. **Reviewers** are auto-assigned by the `.github/CODEOWNERS` file in each repo. No need to manually request review for paths covered by `CODEOWNERS`.
6. **Status checks must pass** before merge. Branch protection enforces this on every Billium repo.

---

## Merge strategy

- **Squash and merge** is the default for short-lived feature/fix/chore branches. Keeps `main` history linear and PR-grained: one PR = one commit on `main`.
- **Rebase and merge** is allowed when a branch has multiple meaningful commits worth preserving (rare).
- **Merge commits** are disabled in branch protection across all repos. They bloat history with noise that doesn't help anyone reviewing later.
- Branch protection enforces **linear history** on `main` everywhere.

---

## Branch lifecycle

- A branch is created off `main`, the PR is opened and merged, the branch is deleted.
- **Auto-delete head branches** is enabled in each repo's GitHub settings, so the remote branch disappears automatically the moment the PR merges. Cleaning up your local copy is your responsibility:

  ```bash
  git checkout main
  git pull
  git branch -d <branch-name>
  ```

- The **only long-lived branch is `main`** in every repo. There are no `develop`, `staging`, or `production` branches — releases are tag-driven from `main` (`v*.*.*` triggers the corresponding `release.yml` workflow).

---

## Releases

The release process depends on the type of project:

- **Libraries published to a registry** (npm, PyPI, etc.): tag-driven via GitHub Actions. Bump the package version in source control, push a `vX.Y.Z` tag, the workflow publishes. Each repo documents its specific tag format and workflow in its README's "Releasing" section.
- **Services with their own deployment pipeline** (web apps, APIs): deployed through the project's regular pipeline (Docker → infra). Tag-driven release notes are optional but encouraged.

Every release should have a matching entry in that project's `CHANGELOG.md`, written for the user-facing audience of the project — developers integrating an SDK, end-users of a dashboard, etc.

---

## Per-repository specifics

Each repository can document additional conventions in its own `README.md` or `CONTRIBUTING.md`. When in doubt, **check the repository you're working in first** — local conventions override these defaults.

---

## Questions

Open a Discussion in the relevant repository, or reach out to the maintainers listed in that repo's `CODEOWNERS`.
