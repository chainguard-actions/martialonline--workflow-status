# Hardening Report: martialonline--workflow-status/v4.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **martialonline--workflow-status/v4.2** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

In action.yml, the expression `${{ github.token }}` is interpolated directly inside a `run:` shell block (assigned to the shell variable `token`). This violates the safe pattern: GitHub Actions expressions should be passed via an `env:` variable and referenced as `$ENV_VAR` in the shell script, not interpolated inline. Although `github.token` itself is not attacker-supplied, direct expression interpolation in `run:` blocks is unsafe by design and can be exploited if the pattern is extended to other context values. The fix is to move the token to an `env:` key: `env:\n  GH_TOKEN: ${{ github.token }}` and reference `$GH_TOKEN` in the script.

Locations:

- `action.yml:17`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Moved `${{ github.token }}` from inline shell assignment in the `run:` block to an `env:` key (`GH_TOKEN: ${{ github.token }}`). Updated both `curl` invocations in the script to use `${GH_TOKEN}` instead of `${token}`. The inline `token=` variable assignment was removed entirely.

