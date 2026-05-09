# Hardening Report: martialonline--workflow-status/v4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **martialonline--workflow-status/v4** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

In action.yml, the `run:` block directly interpolates `${{ github.token }}` into the shell script (line 17: `token="${{ github.token }}"`). Per the security rules, all `github.*` expressions are considered attacker-influenced and must be passed via an `env:` variable rather than interpolated directly into the shell command string. The correct pattern would be to set `env: GITHUB_TOKEN: ${{ github.token }}` on the step and reference `$GITHUB_TOKEN` in the script.

Locations:

- `action.yml:17`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in action.yml: moved `${{ github.token }}` out of the `run:` block and into a step-level `env:` block as `GITHUB_TOKEN: ${{ github.token }}`. The shell script now references `${GITHUB_TOKEN}` as a plain environment variable instead of directly interpolating the GitHub expression into the shell command string.

