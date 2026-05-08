# Contributing to the anvil marketplace

Thanks for considering a contribution. This marketplace ships skills, agents,
hooks, recipes, tools, and bundles consumed by the [anvil CLI](https://github.com/FR-NAN-AI/anvil).
Components are installed into user projects, so quality and security matter.

## Component layout

Each component lives in its own directory under `<type>s/<name>/`:

```
skills/<name>/
  component.yaml   ← metadata (name, type, version, targets, tools)
  index.md         ← instructions for the AI agent
```

The same shape applies to `agents/`, `hooks/`, `recipes/`, `tools/`. Bundles
are single YAML files under `bundles/<name>.yaml`.

After adding a new component, declare it in [`catalog.yaml`](catalog.yaml).

## `component.yaml` schema

Minimal required fields:

```yaml
schema: anvil.component/v1
type: skill | agent | hook | recipe | tool
name: <kebab-case-identifier>
title: <human-readable name>
version: <semver>           # 0.1.0 for new components
description: <one-line summary>
targets:
  copilot:
    supported: true
  claude:
    supported: true
  cursor:
    supported: true
entry:
  file: index.md
```

Versioning follows [SemVer](https://semver.org/). Bumping `version` is required
on any user-visible change to `index.md` or `component.yaml`.

## Security checklist for new components

Before submitting a PR, verify each item below.

### Agents

- [ ] If the agent declares `Bash`, `Edit`, or `Write` in any target's `tools`,
      the same target **must** declare `permissionMode: plan`.
- [ ] No hardcoded credentials, tokens, or internal hostnames in `index.md`.
- [ ] If the agent runs sub-processes, document which commands and why.

### Tools

- [ ] `tools.bash` allowlists must enumerate **specific sub-commands**
      (`git:status`, `npm:install`), never wildcards (`git:*`, `npm`).
- [ ] `network.allow` declarations are currently informational only.
      Document this caveat in the tool's `index.md` so consumers don't
      assume a hard egress filter.

### Skills, recipes, hooks

- [ ] No instructions to disable or bypass other components' safety checks.
- [ ] No instructions that exfiltrate user files, environment variables,
      or secrets to external endpoints.
- [ ] If the component triggers shell commands, the consumed `tools` declare
      a sufficiently narrow allowlist.

## `catalog.yaml` discipline

When adding an entry, mirror the existing pattern:

```yaml
- type: <skill|agent|hook|recipe|tool|bundle>
  name: <kebab-case>
  path: <type>s/<name>
  description: <one-line summary>
  tags: [<tag1>, <tag2>]   # optional
```

If your skill is an onboarding extension, add `tags: [onboarding-extension]`
**and** declare `onboarding-phase: <phase>` in its `component.yaml`. The
anvil CLI surfaces a coherence warning at `marketplace sync` time when these
two declarations drift apart, but it's better to get them right the first time.

Allowed `onboarding-phase` values:

- `prerequisites` — Phase 2, after standard tool checks
- `components` — Phase 3, during component recommendations
- `documentation` — Phase 4, after wiki generation
- `configuration` — Phase 5, after standard overrides
- `finalize` — Phase 6, before marking onboarding complete

## PR process

1. Fork and create a feature branch.
2. Add or modify components under the appropriate directory.
3. Update `catalog.yaml` if you added or renamed a component.
4. Update `CHANGELOG.md` under `## [Unreleased]` with a one-line entry.
5. Run `anvil marketplace sync <yourfork>` from a test project to verify the
   marketplace stays coherent (no `tag-mismatch` or `orphan-phase` warnings).
6. Open a PR with a description that covers: motivation, what changed, which
   surfaces were tested, and any security implications.

PR reviewers will verify the security checklist and may request a security
review on agents or tools that introduce new privileges.

## Reporting security issues

Do **not** open a public PR or issue for security findings. Follow the
process in [SECURITY.md](SECURITY.md) instead.
