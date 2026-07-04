---
---

# Doc Sync — keep the project wiki alive

Refresh the `.anvil/wiki/` knowledge base from what actually changed in the code
since the last sync, cross-referenced with the tickets mentioned in commits, then
**propose** the update as a Pull Request. Never edit code, never auto-merge.

This recipe is **project-agnostic**: everything project-specific (ticket prefix,
ticket provider, sync state) is resolved at runtime from the target project. Do not
hardcode any project name, ticket prefix, or file mapping.

## Inputs (read at runtime, never hardcode)

| Source | Used for |
|--------|----------|
| `.anvil/wiki/.sync-state.json` | Last synced commit + persisted ticket prefix |
| `.anvil/wiki/.cartography.yaml` | `gitInsights.ticketPrefix`, `gitInsights.branchNaming`, hotspots, module→path map |
| `.anvil/config.json` | Ticket provider (Jira / Linear / GitHub Issues), if configured |
| `git log` / `git diff` | The actual delta to document |

## Preconditions

1. A `.anvil/wiki/` directory exists. If not, tell the user to run the
   `generate-project-wiki` skill first, and stop.
2. Read `.anvil/wiki/.sync-state.json`.
   - If it is missing, create it with `lastSyncedCommit` = the wiki's
     `.cartography.yaml` `generatedFromCommit` (fallback: the first commit of the
     repo), and inform the user this is the first run.

## Steps

1. **Read state.** Load `lastSyncedCommit` from `.sync-state.json`.

2. **Compute the delta.** Run `git log <lastSyncedCommit>..HEAD --oneline` and
   `git diff --name-only <lastSyncedCommit>..HEAD`. If the delta is empty, report
   "wiki already up to date" and stop.

3. **Filter noise.** Drop commits that carry no documentation value: dependency
   bumps, formatting-only changes, merge commits, generated files (lockfiles,
   build output). Keep only changes that affect behaviour, architecture, data
   model, conventions, or features.

4. **Resolve the ticket prefix (generic).** In this order:
   a. `gitInsights.ticketPrefix` from `.cartography.yaml`;
   b. else the provider declared in `.anvil/config.json`;
   c. else **infer** it from commit messages using the regex `[A-Z]{2,}-\d+`
      (most frequent prefix wins). If a prefix is inferred or chosen, **persist it**
      to `.sync-state.json` (`ticketPrefix`) so later runs are deterministic.
   Then extract the matching ticket keys from the filtered commits. If a
   `.jira-analysis/` (or equivalent) folder holds notes for those keys, read them
   for business intent.

5. **Route changes to wiki pages (generic).** Map each significant changed file to
   the wiki page(s) it affects, using the standard Anvil wiki structure
   (`02-architecture`, `03-stack`, `04-features`, `05-domain`, `06-conventions`,
   `07-operations`, `08-known-issues`) plus a path/name heuristic and the
   module→path map in `.cartography.yaml`. Do **not** rely on a project-specific
   lookup table.

6. **Update the wiki.** Edit only the impacted pages. Preserve the existing house
   style:
   - keep the confidence markers (✅ certain / 🟡 probable / ⚠ to confirm);
   - keep links pointing to real source files;
   - add a trace comment on updated sections, e.g.
     `<!-- source: <TICKET-KEY>, commit <sha> -->`.
   Also refresh `.cartography.yaml` where it is stale (recent commits, hotspots,
   domain vocabulary) and the domain glossary when new business terms appear.
   Flag — do not silently rewrite — any page whose underlying code changed but
   whose meaning you cannot confirm.

7. **Write state.** Update `.sync-state.json`:
   `lastSyncedCommit` = current `HEAD`, `lastSyncedAt` = today (ISO date),
   `pagesTouched` = the list of pages you edited.

8. **Open a PR.** Create a branch `docs/sync-<shortHead>`, commit the wiki changes
   with message `docs: sync wiki <lastSyncedCommit>..<HEAD>`, and open a Pull
   Request summarising: pages updated, tickets covered, and any page flagged as
   "to confirm". **Stop there — a human reviews and merges.**

## Guardrails

- **Propose, never apply blindly.** The only output is a reviewable PR.
- **Delta-only.** Never re-scan the whole repository; always start from
  `lastSyncedCommit`.
- **Never touch application code**, tests, or config — only `.anvil/wiki/` and
  `.sync-state.json`.
- **No secrets.** Do not read or write credentials; use the configured provider's
  existing auth only if already available.
- **Idempotent.** Running twice with no new commits must be a no-op.

## Output

```
Doc Sync
────────
Delta: <N> commits (<lastSyncedCommit>..<HEAD>), <M> significant
Tickets: <TICKET-KEY>, ...
Pages updated: 04-features/<x>.md, 05-domain/glossary.md, ...
Flagged "to confirm": <page> (<reason>)
PR: docs/sync-<shortHead>  → awaiting human review
```

## Notes

- This recipe is designed to run **inside the developer's IDE agent** on any active
  surface (Copilot / Claude / Cursor / OpenCode). It does not require any server or
  scheduler. The companion `wiki-freshness` hook proposes it automatically when the
  wiki drifts.
- Promotion to an autonomous GitHub Agentic Workflow (`anvil promote`) is possible
  but out of scope here — it introduces CI infrastructure.
