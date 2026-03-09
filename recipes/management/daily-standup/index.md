---
on:
  schedule:
    cron: "0 14 * * 1-5"

engine: copilot

permissions:
  issues: read

imports:
  - ../../skills/read-ticket/base.md

tools:
  github:
    toolsets: [issues]

safe-outputs:
  create-comment:
---

# Daily Standup

Generate a daily standup summary from active issues.

## Steps
1. Identify issues labeled "in-progress"
2. Summarize progress and blockers
3. Post a brief standup update on the team status issue
