<!-- markdownlint-disable -->

# Hardening Report: cloudnative-pg--ciclops/v1.3.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **cloudnative-pg--ciclops/v1.3.2** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow uses `actions/upload-artifact@v5`, which is pinned to a mutable version tag rather than an immutable 40-character commit SHA. This exposes the workflow to supply-chain attacks if the tag is moved to a different (potentially malicious) commit. It should be replaced with a full SHA pin, e.g. `actions/upload-artifact@<40-char-sha> # v5`.

Locations:

- `.github/workflows/overflow-test.yaml:29`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned `actions/upload-artifact@v5` to its full commit SHA `330a01c490aca151604b8cf639adc76d48f6c5d4` in `.github/workflows/overflow-test.yaml` (line 29). The mutable tag is preserved as a comment for readability.

