---
on:
  push:
    branches: [main, develop]

engine: copilot

permissions:
  contents: read
  issues: write

imports:
  - ../../skills/security-review.md

tools:
  github:
    toolsets: [repos, issues]

safe-outputs:
  create-comment:
---

# Post-Merge Check

After merge, verify that critical paths remain healthy.

## Steps
1. Identify the merged changeset from ${{ github.event.after }}
2. Check for config or dependency changes
3. Scan for potential security or performance regressions
4. If issues are detected, open a follow-up issue with details
