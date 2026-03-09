---
---

# Changelog Update

Generate or update the project's CHANGELOG based on commit history when a version tag is created.

## Trigger

After creating a version tag (`v*`), or manually when preparing a release.

## Process

### 1. Detect Version Range

- Find the current tag and the previous tag
- If no previous tag exists, use all commits from the beginning

```bash
CURRENT_TAG=v1.2.0
PREVIOUS_TAG=$(git tag --sort=-v:refname | head -2 | tail -1)
```

### 2. Collect Commits

Parse commits between the two tags. Group by Conventional Commit type:

| Prefix | Section |
|--------|---------|
| `feat:` | Features |
| `fix:` | Bug Fixes |
| `perf:` | Performance |
| `refactor:` | Refactoring |
| `docs:` | Documentation |
| `test:` | Tests |
| `chore:` | Maintenance |
| `BREAKING CHANGE:` | Breaking Changes (always first) |

Commits without a conventional prefix go into "Other".

### 3. Generate Entry

Format:

```markdown
## [1.2.0] — 2026-03-09

### Breaking Changes
- Remove deprecated `/api/v1` endpoints (#142)

### Features
- Add user profile page (#138)
- Support dark mode (#141)

### Bug Fixes
- Fix login redirect loop on expired sessions (#139)
- Correct timezone handling in date picker (#140)

### Maintenance
- Update dependencies (#143)
```

### 4. Update CHANGELOG.md

- Prepend the new entry after the header
- Keep existing entries intact
- If `CHANGELOG.md` doesn't exist, create it with a header:

```markdown
# Changelog

All notable changes to this project will be documented in this file.

Format based on [Keep a Changelog](https://keepachangelog.com/).
```

## Output

```
Changelog Update
────────────────
✅ Version: v1.2.0 (previous: v1.1.0)
✅ Commits: 12 commits parsed
✅ Sections: 2 features, 3 fixes, 1 breaking change
✅ CHANGELOG.md updated

Review the changelog before committing.
```

## Rules

- Never overwrite existing changelog entries — only prepend
- Include PR/issue numbers when available (from commit messages)
- Flag commits that don't follow Conventional Commits format
- Always generate a draft for review — don't auto-commit the changelog
- Sort breaking changes first — they're the most important for users
