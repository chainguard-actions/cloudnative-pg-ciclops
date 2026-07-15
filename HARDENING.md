<!-- markdownlint-disable -->

# Hardening Report: cloudnative-pg--ciclops/v1.2.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **cloudnative-pg--ciclops/v1.2.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a run: shell command. In test.yaml, the step 'If there are alerts, echo them' runs: `echo "${{steps.generate-summary.outputs.alerts}}"`. The value of steps.generate-summary.outputs.alerts flows through YAML template substitution before the shell sees it, allowing an attacker who can influence the action's output to inject arbitrary shell commands.

Locations:

- `.github/workflows/test.yaml:16`

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable version tags instead of full 40-character commit SHAs, making them vulnerable to supply-chain attacks if the tag is moved. Failing references: overflow-test.yaml: actions/checkout@v3, actions/upload-artifact@v3; test.yaml: actions/checkout@v3; unit-test.yaml: actions/checkout@v3, actions/setup-python@v4.

Locations:

- `.github/workflows/overflow-test.yaml:14`
- `.github/workflows/overflow-test.yaml:22`
- `.github/workflows/test.yaml:9`
- `.github/workflows/unit-test.yaml:9`
- `.github/workflows/unit-test.yaml:13`

### missing-permissions (severity: medium)

None of the three workflow files define a top-level permissions: key, and no individual job within them defines a job-level permissions: key. Without explicit permissions, workflows inherit the default repository token permissions, which may be broader than necessary (e.g. write access to contents). All three files are affected: overflow-test.yaml, test.yaml, and unit-test.yaml.

Locations:

- `.github/workflows/overflow-test.yaml:1`
- `.github/workflows/test.yaml:1`
- `.github/workflows/unit-test.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed all three findings across .github/workflows/test.yaml, overflow-test.yaml, and unit-test.yaml:
1. script-injection: Moved ${{ steps.generate-summary.outputs.alerts }} from run: shell command into env: block as ALERTS, referenced as $ALERTS in shell.
2. unpinned-uses: Pinned actions/checkout@v3 to SHA f43a0e5ff2bd294095638e18286ca9a3d1956744, actions/upload-artifact@v3 to SHA ff15f0306b3f739f7b6fd43fb5d26cd321bd4de5, and actions/setup-python@v4 to SHA 7f4fc3e22c37d6ff65e88745f38bd3157c663f7c, all with tag comments.
3. missing-permissions: Added permissions: {} at the top level of all three workflow files.

