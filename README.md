# .github

This repository hosts the **community health files** that apply by default to every public and private repository under the [BilliumHQ](https://github.com/BilliumHQ) organization.

GitHub automatically uses files in this repo as fallbacks for any sibling repo that doesn't define its own version. The current defaults are:

- [`CONTRIBUTING.md`](./CONTRIBUTING.md) — branch naming, commit messages, pull request workflow, and merge strategy used across every Billium repo.

When a specific repository needs to override one of these defaults (for example, a repo with a different release process), it adds its own copy of the file at the same path and that copy wins for that repo only.

## Adding a new default

To add a new community health file (`SECURITY.md`, `CODE_OF_CONDUCT.md`, `SUPPORT.md`, `PULL_REQUEST_TEMPLATE.md`, etc.), open a pull request against this repo. Once merged to `main`, every BilliumHQ repository starts inheriting it immediately — no per-repo action required.

## See also

- [GitHub docs: creating a default community health file](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file) — explains the inheritance mechanism.
- Each individual repository's `CLAUDE.md` (if present) for any repo-specific overrides or conventions that aren't shared org-wide.
