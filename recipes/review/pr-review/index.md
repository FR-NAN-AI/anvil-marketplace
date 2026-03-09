---
on:
  pull_request:
    types: [opened, synchronize]

engine: copilot

permissions:
  contents: read
  pull-requests: write

imports:
  - ../../skills/coding-principles.md
  - ../../skills/security-review.md

tools:
  github:
    toolsets: [pull_requests, repos]

safe-outputs:
  create-comment:
---

# PR Review

Review the pull request for correctness, security, and maintainability.

## Steps
1. Summarize the change in 3-5 sentences
2. Identify risks, regressions, or missing tests
3. Provide actionable feedback with file references
4. Approve if no blocking issues exist
