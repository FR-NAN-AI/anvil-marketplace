---
# Shared skill: coding principles
# Allowed import fields only
---

# Coding Principles

Use these principles for all changes unless a repository standard conflicts.

## Core Values
- Prefer clarity over cleverness. Optimize for future readers.
- Keep changes small and cohesive. One intent per change set.
- Favor stable, predictable behavior. Avoid surprising side effects.
- Preserve public contracts unless explicitly changing them.

## Style and Structure
- Match existing patterns in the codebase before introducing new ones.
- Keep functions short and named for behavior, not implementation.
- Avoid deep nesting; refactor into helpers when logic branches multiply.
- Use strong typing where available to prevent misuse.

## Git Conventions
- Branch naming: `feature/short-slug`, `bugfix/short-slug`, `chore/short-slug`, `hotfix/short-slug`.
- Commit messages use Conventional Commits: `type(scope): summary`.
- Use present tense, imperative mood (e.g., "add validation", not "added").
- Include `BREAKING CHANGE:` footer when behavior or APIs change.

## Code Organization
- Keep files in the closest logical module; avoid cross-layer leakage.
- Prefer explicit module boundaries over shared "utils" buckets.
- Group by feature when changes cross multiple layers.
- Keep public interfaces stable; internal helpers can change freely.

## Error Handling
- Fail fast with clear messages when input is invalid.
- Wrap external calls with contextual errors that aid triage.
- Never swallow errors silently.
- Use typed errors or error codes where the system expects them.
- Normalize error messages at boundaries to avoid leaking internals.

## Logging Standards
- Log at appropriate levels: `debug` for development, `info` for state changes, `warn` for recoverable issues, `error` for failures.
- Include request IDs or correlation IDs when available.
- Never log secrets, tokens, or raw PII.
- Prefer structured logs over free-form strings.

## Performance
- Measure before optimizing. Prefer algorithmic wins over micro-optimizations.
- Avoid unnecessary work in hot paths (I/O, loops, serialization).

## Documentation
- Update inline docs and README when behavior changes.
- Keep comments focused on "why" rather than "what".

## Review Checklist
- Is the change minimal and focused?
- Are edge cases covered?
- Are errors surfaced and actionable?
- Did we avoid breaking compatibility?
