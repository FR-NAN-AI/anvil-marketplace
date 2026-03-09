---
on:
  schedule:
    cron: "0 12 * * 1"

engine: copilot

permissions:
  issues: read
  projects: read

imports:
  - ../../skills/read-ticket/base.md

tools:
  github:
    toolsets: [issues, repos]

safe-outputs:
  create-comment:
---

# Sprint Analysis

Produce a weekly sprint health report.

## Steps
1. Collect open issues labeled with the current sprint
2. Summarize progress, risks, and blockers
3. Highlight scope changes and carryover risk
4. Post the report as a comment on the sprint tracking issue
