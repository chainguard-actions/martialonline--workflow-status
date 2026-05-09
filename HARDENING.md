# Hardening Report: martialonline--workflow-status/v3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **martialonline--workflow-status/v3** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

In action.yml, the `run:` block directly interpolates `${{ github.token }}` into the shell command string: `token="${{ github.token }}"`  (line 17). GitHub Actions expressions should never be interpolated directly into `run:` scripts; instead, the value should be passed via an `env:` mapping and referenced as a shell variable (e.g., `env: TOKEN: ${{ github.token }}` then `$TOKEN` in the script). Direct interpolation can allow expression values to break out of their intended context and execute arbitrary shell commands.

Locations:

- `action.yml:17`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in action.yml: moved `${{ github.token }}` out of the `run:` block and into an `env:` mapping as `GITHUB_TOKEN: ${{ github.token }}`. Updated all references in the shell script from `${token}` to `${GITHUB_TOKEN}`. The intermediate `token` variable assignment was also removed since it's no longer needed.

