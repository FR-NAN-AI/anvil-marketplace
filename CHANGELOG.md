# Changelog

All notable changes to the anvil marketplace will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this marketplace adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Security
- Aligned `permissionMode: plan` on every agent that declares `Bash`, `Edit`,
  or `Write` in its Claude target. Previously only `architect`,
  `legacy-cartographer`, and `requirement-analyzer` enforced this; now
  `task-executor`, `code-reviewer`, `quality-guardian`, and `security-auditor`
  do as well. Closes MKT-01 and MKT-07.

### Added
- `SECURITY.md` — declares the private disclosure channel (GitHub Security
  Advisories) and the security expectations for new components.
- `CONTRIBUTING.md` — documents the `component.yaml` schema, the PR process,
  and the security checklist authors must satisfy.
- `CHANGELOG.md` — this file. Adopts Keep a Changelog + SemVer to match the
  anvil CLI release discipline.

## [0.1.0] - 2026-05-08

Initial tagged baseline. The marketplace ships:

### Skills

- `coding-principles` — Universal coding principles (DRY, YAGNI, KISS, SOLID)
- `test-driven-development` — RED-GREEN-REFACTOR cycle
- `systematic-debugging` — 4-phase root cause analysis
- `verification-before-completion` — Evidence before claims
- `security-review` — Security review checklist
- `implement-feature` — Feature implementation workflow
- `create-pr` — Pull request creation guidance
- `onboarding` — Intelligent project setup and configuration (synced with
  anvil CLI's `onboarding-phase` mechanism for Phases 2–6)
- `anvil-cli-reference` — Complete anvil CLI command reference for AI agents
- `catalog-browser` — Browse and install components from the marketplace
- `frontend-design` — Distinctive, production-grade frontend interfaces
- `analyze-codebase-deep` — 4-layer method for legacy codebase analysis
- `generate-project-wiki` — Cartography → navigable wiki under `.anvil/wiki/`
- `generate-runnable-readme` — Clone-install-run README at
  `.anvil/wiki/01-getting-started/`

### Agents

- `requirement-analyzer` — Analyzes tickets, produces structured briefs
- `task-executor` — Implements tasks and delivers tested changes
- `code-reviewer` — Reviews work against plan and coding standards
- `architect` — Architecture analysis, patterns, trade-offs
- `quality-guardian` — Code quality audit (complexity, duplication, debt)
- `security-auditor` — Security audit (OWASP, injections, secrets)
- `legacy-cartographer` — Sourced cartography for opaque legacy codebases

### Hooks

- `pre-commit-check` — Lint, tests, and secret scan before each commit
- `post-merge-sync` — Update dependencies and regenerate artifacts after merge
- `changelog-update` — Generate or update CHANGELOG from commits after tags
- `dependency-check` — Check for outdated or vulnerable dependencies

### Tools

- `github-standard` — Standard GitHub toolsets
- `bash-dev` — Developer bash tools
- `web-tools` — Limited web access

### Recipes

- `development/us-to-pr` (with sub-steps `us-analyze`, `us-brainstorm`,
  `us-plan`, `us-implement`, `us-deliver`) — Issue to PR workflow with
  checkpoints
- `development/bugfix` — Bugfix workflow
- `review/pr-review` — PR review workflow
- `review/post-merge-check` — Post-merge health check

### Bundles

- `starter` — Minimal bundle for any project
- `full-stack` — Complete bundle (all skills, recipes, agents)
- `tech-lead` — Quality, reviews, and process bundle
- `legacy-archaeology` — Drops a team onto an opaque legacy codebase and
  produces a complete documentation wiki
