<!-- markdownlint-disable -->

# Hardening Report: ilammy--msvc-dev-cmd/v1.13.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **ilammy--msvc-dev-cmd/v1.13.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference GitHub Actions using mutable tags instead of pinned full-length SHA commit hashes. This exposes the workflow to supply-chain attacks where a tag could be moved to point to malicious code.

In .github/workflows/main.yml:
- `uses: actions/checkout@v4` (3 occurrences)

In .github/workflows/release.yml:
- `uses: ilammy/msvc-dev-cmd@release/v1`
- `uses: actions/checkout@v4` (2 occurrences)

All of these should be pinned to a full 40-character commit SHA, e.g. `uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/main.yml:14`
- `.github/workflows/release.yml:11`

### missing-permissions (severity: medium)

Neither .github/workflows/main.yml nor .github/workflows/release.yml defines a top-level `permissions:` key, and no individual job within either file defines a `permissions:` key. Without explicit permissions, workflows run with the default (potentially broad) token permissions, violating the principle of least privilege. A top-level `permissions: {}` block with only the specific scopes required should be added to each workflow.

Locations:

- `.github/workflows/main.yml:1`
- `.github/workflows/release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:
1. main.yml: Pinned all 3 occurrences of actions/checkout@v4 to SHA 11d5960a326750d5838078e36cf38b85af677262 and added top-level `permissions: {}`.
2. release.yml: Pinned 2 occurrences of actions/checkout@v4 to SHA 11d5960a326750d5838078e36cf38b85af677262, pinned ilammy/msvc-dev-cmd@release/v1 to SHA 0b201ec74fa43914dc39ae48a89fd1d8cb592756, and added top-level `permissions: {}`.

