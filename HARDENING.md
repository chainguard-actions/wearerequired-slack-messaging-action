<!-- markdownlint-disable -->

# Hardening Report: wearerequired--slack-messaging-action/v2.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wearerequired--slack-messaging-action/v2.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in the workflow files are pinned to mutable version tags rather than immutable full 40-character SHA commit hashes. This exposes the workflows to supply-chain attacks if the referenced action tags are moved or overwritten. Failing references:
- build.yml: `actions/checkout@v3`, `actions/setup-node@v3`, `EndBug/add-and-commit@v9`
- coding-standards.yml: `actions/checkout@v3` (×2), `actions/setup-node@v3`, `wearerequired/lint-action@v2`
- versioning.yml: `Actions-R-Us/actions-tagger@v2`

Locations:

- `.github/workflows/build.yml:13`
- `.github/workflows/build.yml:17`
- `.github/workflows/build.yml:28`
- `.github/workflows/coding-standards.yml:14`
- `.github/workflows/coding-standards.yml:19`
- `.github/workflows/coding-standards.yml:24`
- `.github/workflows/coding-standards.yml:32`
- `.github/workflows/versioning.yml:8`

### missing-permissions (severity: medium)

None of the workflow files define a `permissions:` key at the top level or at the job level. Without explicit permissions, workflows run with the default token permissions, which may be broader than necessary (e.g., write access to repository contents). This is especially concerning for `coding-standards.yml`, which uses the high-privilege `pull_request_target` trigger. All three workflow files should declare minimal required permissions.

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/coding-standards.yml:1`
- `.github/workflows/versioning.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 3 workflow files:

**unpinned-uses**: Pinned all 7 action references to full 40-char SHAs:
- actions/checkout@v3 → @f43a0e5ff2bd294095638e18286ca9a3d1956744 (used in build.yml and coding-standards.yml)
- actions/setup-node@v3 → @3235b876344d2a9aa001b8d1453c930bba69e610 (used in build.yml and coding-standards.yml)
- EndBug/add-and-commit@v9 → @a94899bca583c204427a224a7af87c02f9b325d5 (build.yml)
- wearerequired/lint-action@v2 → @548d8a7c4b04d3553d32ed5b6e91eb171e10e7bb (coding-standards.yml)
- Actions-R-Us/actions-tagger@v2 → @330ddfac760021349fef7ff62b372f2f691c20fb (versioning.yml)

**missing-permissions**: Added top-level `permissions:` blocks with minimal required permissions:
- build.yml: `contents: write` (required to commit dist files)
- coding-standards.yml: `contents: read` (read-only linting; minimal for pull_request_target trigger)
- versioning.yml: `contents: write` (required to create/update version tags)

