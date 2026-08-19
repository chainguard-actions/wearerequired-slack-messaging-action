<!-- markdownlint-disable -->

# Hardening Report: wearerequired--slack-messaging-action/v2.0.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wearerequired--slack-messaging-action/v2.0.2** was hardened automatically. 6 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in build.yml use mutable version tags instead of pinned 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if any referenced action is compromised or its tag is moved. Failing references: `actions/checkout@v3` (line 15), `actions/setup-node@v3` (line 18), `EndBug/add-and-commit@v9` (line 30).

Locations:

- `.github/workflows/build.yml:15`
- `.github/workflows/build.yml:18`
- `.github/workflows/build.yml:30`

### unpinned-uses (severity: high)

All `uses:` references in coding-standards.yml use mutable version tags instead of pinned 40-character commit SHAs. Failing references: `actions/checkout@v3` (lines 16 and 21), `actions/setup-node@v3` (line 27), `wearerequired/lint-action@v2` (line 34).

Locations:

- `.github/workflows/coding-standards.yml:16`
- `.github/workflows/coding-standards.yml:21`
- `.github/workflows/coding-standards.yml:27`
- `.github/workflows/coding-standards.yml:34`

### unpinned-uses (severity: high)

The `uses:` reference in versioning.yml uses a mutable version tag instead of a pinned 40-character commit SHA. Failing reference: `Actions-R-Us/actions-tagger@v2` (line 10).

Locations:

- `.github/workflows/versioning.yml:10`

### missing-permissions (severity: medium)

build.yml has no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (write access to contents, packages, etc.).

Locations:

- `.github/workflows/build.yml:1`

### missing-permissions (severity: medium)

coding-standards.yml has no top-level `permissions:` key and no job-level `permissions:` key on any job. This workflow is triggered by `pull_request_target` (which runs with write access to the base repository), making the absence of explicit minimal permissions especially risky.

Locations:

- `.github/workflows/coding-standards.yml:1`

### missing-permissions (severity: medium)

versioning.yml has no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, the workflow inherits the repository's default token permissions.

Locations:

- `.github/workflows/versioning.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 6 findings across 3 workflow files:

1. build.yml: Pinned actions/checkout@v3→@a37ce9120846195fa4ece8f58b268e6043cb2f26, actions/setup-node@v3→@3235b876344d2a9aa001b8d1453c930bba69e610, EndBug/add-and-commit@v9→@a94899bca583c204427a224a7af87c02f9b325d5. Added top-level `permissions: contents: write` (required to commit dist files back to the repo).

2. coding-standards.yml: Pinned both actions/checkout@v3 occurrences→@a37ce9120846195fa4ece8f58b268e6043cb2f26, actions/setup-node@v3→@3235b876344d2a9aa001b8d1453c930bba69e610, wearerequired/lint-action@v2→@548d8a7c4b04d3553d32ed5b6e91eb171e10e7bb. Added top-level `permissions: contents: read, pull-requests: read` (minimal for a linting workflow; especially important given the pull_request_target trigger).

3. versioning.yml: Pinned Actions-R-Us/actions-tagger@v2→@330ddfac760021349fef7ff62b372f2f691c20fb. Added top-level `permissions: contents: write` (required to create/update version tags).

All SHAs were resolved via lookup_action_sha and are real 40-character commit SHAs.

