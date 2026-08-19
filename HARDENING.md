<!-- markdownlint-disable -->

# Hardening Report: wearerequired--slack-messaging-action/v3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wearerequired--slack-messaging-action/v3.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in the workflow files use mutable version tags instead of full 40-character SHA commit digests, making the workflows vulnerable to supply-chain attacks if the referenced action tags are moved or compromised. Failing references:
- `actions/checkout@v3` (build.yml, coding-standards.yml)
- `actions/setup-node@v3` (build.yml, coding-standards.yml)
- `EndBug/add-and-commit@v9` (build.yml)
- `wearerequired/lint-action@v2` (coding-standards.yml)
- `Actions-R-Us/actions-tagger@v2` (versioning.yml)

Locations:

- `.github/workflows/build.yml:15`
- `.github/workflows/build.yml:19`
- `.github/workflows/build.yml:30`
- `.github/workflows/coding-standards.yml:16`
- `.github/workflows/coding-standards.yml:21`
- `.github/workflows/coding-standards.yml:27`
- `.github/workflows/coding-standards.yml:33`
- `.github/workflows/versioning.yml:10`

### missing-permissions (severity: medium)

None of the three workflow files declare a top-level `permissions:` block, and no job within them declares job-level permissions either. Without explicit permissions, workflows run with the default (potentially broad) GITHUB_TOKEN permissions, violating the principle of least privilege. Affected files: build.yml, coding-standards.yml, versioning.yml.

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/coding-standards.yml:1`
- `.github/workflows/versioning.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 3 workflow files: (1) Pinned all 5 action references to full 40-char SHAs with original tags as comments: actions/checkout@v3→a37ce91, actions/setup-node@v3→3235b87, EndBug/add-and-commit@v9→a94899b, wearerequired/lint-action@v2→548d8a7, Actions-R-Us/actions-tagger@v2→330ddfa. (2) Added top-level `permissions: {}` to all 3 files plus job-level permissions: build.yml and versioning.yml get `contents: write` (needed to commit/tag), coding-standards.yml gets `contents: read` (read-only linting).

