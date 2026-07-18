<!-- markdownlint-disable -->

# Hardening Report: wearerequired--slack-messaging-action/v2.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wearerequired--slack-messaging-action/v2.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in the workflow files use mutable version tags instead of pinned 40-character SHA digests, making them vulnerable to supply-chain attacks if the referenced action is compromised or its tag is moved.

Failing references:
- `.github/workflows/build.yml`: `actions/checkout@v3`, `actions/setup-node@v3`, `EndBug/add-and-commit@v9`
- `.github/workflows/coding-standards.yml`: `actions/checkout@v3` (×2), `actions/setup-node@v3`, `wearerequired/lint-action@v1`
- `.github/workflows/versioning.yml`: `Actions-R-Us/actions-tagger@v2`

Locations:

- `.github/workflows/build.yml:15`
- `.github/workflows/build.yml:18`
- `.github/workflows/build.yml:28`
- `.github/workflows/coding-standards.yml:15`
- `.github/workflows/coding-standards.yml:20`
- `.github/workflows/coding-standards.yml:25`
- `.github/workflows/coding-standards.yml:31`
- `.github/workflows/versioning.yml:10`

### missing-permissions (severity: medium)

None of the workflow files define a top-level `permissions:` block, and no individual job defines its own `permissions:` block. Without explicit permissions, workflows run with the default (potentially broad) token permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/coding-standards.yml:1`
- `.github/workflows/versioning.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 3 workflow files:

1. **unpinned-uses**: Pinned all 8 `uses:` references to full 40-char SHA digests with original tags preserved as comments:
   - actions/checkout@v3 → @f43a0e5ff2bd294095638e18286ca9a3d1956744 # v3
   - actions/setup-node@v3 → @3235b876344d2a9aa001b8d1453c930bba69e610 # v3
   - EndBug/add-and-commit@v9 → @a94899bca583c204427a224a7af87c02f9b325d5 # v9
   - wearerequired/lint-action@v1 → @42567b31ed576cdd5a431d77ca5bc8822430d1d0 # v1
   - Actions-R-Us/actions-tagger@v2 → @330ddfac760021349fef7ff62b372f2f691c20fb # v2

2. **missing-permissions**: Added top-level `permissions: {}` to all 3 workflows plus minimal job-level permissions:
   - build.yml job: `contents: write` (needed to commit dist)
   - coding-standards.yml job: `contents: read` (read-only for linting)
   - versioning.yml job: `contents: write` (needed to create/update tags)

