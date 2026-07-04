---
---

# Wiki Freshness

Keep the project knowledge base honest: notice when `.anvil/wiki/` has fallen
behind the code and offer to refresh it. This runs at the **start of a session**,
so the person opening the project is the trigger — no scheduler, no server.

Project-agnostic: it reads state from the target project and never assumes a
project name or ticket prefix.

## Trigger

At session start, if a `.anvil/wiki/` directory exists.

## Check

1. Read `lastSyncedCommit` from `.anvil/wiki/.sync-state.json`.
   - If the file is missing, treat the wiki as never-synced and suggest a first
     `doc-sync` run.
2. Count the drift: `git rev-list --count <lastSyncedCommit>..HEAD`.
3. If the count is **greater than 15**, surface a short notice. Otherwise stay
   silent (do not interrupt the developer).

## Notice (only when drift > 15)

```
⚠  Wiki drift: <N> commits behind the code (last sync: <lastSyncedCommit>).
   Run the `doc-sync` recipe to refresh the wiki and open a PR? (y/n)
```

- Only **offer** — never run `doc-sync` automatically.
- Show this at most once per session.
- Keep it to a couple of lines; it is a nudge, not a report.

## Rules

- Read-only. This hook inspects state and git; it never edits the wiki itself.
- The drift threshold (15) is a sensible default; a project may override it by
  storing `driftThreshold` in `.sync-state.json`, in which case use that value.
