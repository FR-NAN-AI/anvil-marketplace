---
on:
  issues:
    types: [opened]
    labels: [requirements]

engine: copilot

permissions:
  issues: write

imports:
  - ../../skills/read-ticket/base.md

tools:
  github:
    toolsets: [issues]

safe-outputs:
  create-comment:
  add-labels:
    allowed: [needs-info, ready-for-dev]
---

# Requirement → User Story

Convert a requirements issue into a structured user story.

## Steps
1. Read issue #${{ github.event.issue.number }}
2. Extract goals, constraints, and acceptance criteria
3. Draft a user story and acceptance checklist in a comment
4. If missing info, ask questions and add label "needs-info"
5. If complete, add label "ready-for-dev"
