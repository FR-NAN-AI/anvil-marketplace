---
tools:
  bash:
    - "git:status"
    - "git:diff"
    - "git:log"
    - "git:branch"
    - "git:checkout"
    - "git:commit"
    - "git:push"
    - "npm:install"
    - "npm:test"
    - "npm:run-script"
    - "node:--version"
    - "python:--version"
    - "python:-m"
---

# Bash Dev Tools

Provide a narrowly-scoped set of developer commands for local automation,
builds, and tests. The allowlist above enumerates **specific sub-commands**
rather than wildcards: a hostile `package.json` postinstall or a piped
shell script cannot escape the surface declared here.

This declaration is informational at the moment — anvil does not yet
translate it into native sandbox rules on each surface (Claude permission
system, Copilot tool config, Cursor MCP). The narrowing still matters
because it documents the supported surface for any sandbox layer added
in the future, and because LLMs reading the tool description are far
less likely to attempt out-of-scope commands than with a `git:*` wildcard.

If you need a sub-command that's not on this list, prefer asking the user
to run it themselves rather than expanding the allowlist silently.
