---
on:
  pull_request_review:
    types: [submitted]

engine: copilot

permissions:
  contents: write
  pull-requests: write

imports:
  - ../../skills/coding-principles.md

tools:
  github:
    toolsets: [pull_requests, repos]

safe-outputs:
  create-comment:
---

# Review Response

Respond to review feedback by clarifying intent or addressing requested changes.

## Steps
1. Read the review comments on PR #${{ github.event.pull_request.number }}
2. Group feedback into required changes vs. questions
3. Implement changes or respond with rationale
4. Summarize updates in a PR comment
