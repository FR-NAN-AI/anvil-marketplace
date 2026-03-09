---
on:
  issues:
    types: [labeled]
    labels: [bug]

engine: copilot

permissions:
  contents: write
  issues: write
  pull-requests: write

imports:
  - ../../skills/coding-principles.md
  - ../../skills/testing-tdd.md
  - ../../skills/security-review.md
  - ../../skills/read-ticket/base.md
  - ../../skills/implement-feature.md
  - ../../skills/create-pr.md

tools:
  github:
    toolsets: [repos, issues, pull_requests]
  bash: ["git:*", "npm", "node"]

safe-outputs:
  create-pull-request:
    labels: [agent-generated]
    draft: false
  create-comment:
---

# Bugfix

Fix a labeled bug with a minimal, well-tested change.

## Steps
1. Read issue #${{ github.event.issue.number }} and confirm expected behavior
2. Reproduce the bug if possible and document reproduction steps
3. Add a failing test that captures the bug
4. Implement the fix and refactor if needed
5. Run relevant tests
6. Open a PR and link the issue
