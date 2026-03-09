---
---

# Post-Merge Sync

Run after merging a branch to ensure the local environment stays consistent with the latest code.

## Trigger

After `git merge` or `git pull` that introduces changes to dependency files or generated artifacts.

## Actions

### 1. Dependency Update

If dependency files changed, reinstall:

| File Changed | Action |
|-------------|--------|
| `package.json` or `package-lock.json` | `npm ci` |
| `yarn.lock` | `yarn install --frozen-lockfile` |
| `pnpm-lock.yaml` | `pnpm install --frozen-lockfile` |
| `requirements.txt` or `pyproject.toml` | `pip install -r requirements.txt` or `pip install -e .` |
| `Pipfile.lock` | `pipenv sync` |
| `go.mod` or `go.sum` | `go mod download` |
| `Cargo.lock` | `cargo fetch` |
| `pom.xml` or `build.gradle` | `mvn dependency:resolve` or `gradle dependencies` |
| `Gemfile.lock` | `bundle install` |

### 2. Database Migrations

If migration files changed, notify the developer:

```
⚠️  New migrations detected:
  - 2026-03-09_add_users_table.sql
  - 2026-03-09_add_roles.sql

Run migrations before testing:
  npm run migrate   (or your project's migration command)
```

**Don't auto-run migrations** — they may require review or specific environments.

### 3. Code Generation

If schema or config files changed that trigger code generation:

| File Changed | Action |
|-------------|--------|
| `.proto` files | Run protobuf generation |
| `openapi.yaml` / `swagger.json` | Run API client generation |
| `.graphql` schema | Run codegen |
| Anvil config (`.anvil/config.json`) | `anvil generate --json` |

### 4. Environment Check

Warn if new environment variables appeared:

```
⚠️  New environment variables detected in .env.example:
  - REDIS_URL
  - FEATURE_FLAG_SERVICE

Make sure these are set in your local .env file.
```

## Output

```
Post-Merge Sync
───────────────
✅ Dependencies: npm ci (12.3s)
⚠️  Migrations: 2 new — run manually
✅ Codegen: anvil generate (0.8s)
⚠️  Environment: 1 new variable — check .env

Sync complete.
```

## Rules

- Only run actions for files that actually changed in the merge
- Never auto-run destructive operations (migrations, database resets)
- Keep total sync time under 60 seconds where possible
- If a command fails, report clearly but don't block the developer
