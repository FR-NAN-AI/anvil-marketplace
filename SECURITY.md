# Security Policy

## Reporting a Vulnerability

If you discover a security issue in any component published by this marketplace
(skills, agents, hooks, recipes, tools), **do not open a public issue**.

Use **GitHub Security Advisories** to report privately:
<https://github.com/FR-NAN-AI/anvil-marketplace/security/advisories/new>

Include in your report:

- The component name and version
- A clear description of the vulnerability and the security impact
- Steps to reproduce (or a proof of concept)
- Any suggested mitigation if you have one in mind
- The agent surfaces affected (Claude, Copilot, Cursor) if known

We will acknowledge receipt within 5 working days and aim to provide an
initial assessment within 10 working days.

## Scope

This policy covers components shipped from this repository's `catalog.yaml`.
Vulnerabilities in the **anvil CLI** itself should be reported on the CLI repo:
<https://github.com/FR-NAN-AI/anvil/security/advisories/new>

## Security expectations for components

Anyone authoring a new component for this marketplace must follow the rules in
[CONTRIBUTING.md](CONTRIBUTING.md), especially:

- Agents that declare `Bash`, `Edit`, or `Write` tools **MUST** also declare
  `permissionMode: plan` on the Claude target. This forces a plan-approval
  step before any potentially destructive action.
- `tools.bash` allowlists in `tool` components must be scoped to specific
  sub-commands (`git:status`, not `git:*`) to limit RCE surface from
  hostile repositories.
- Network egress declarations (`network.allow`) are currently informational
  only — do not rely on them as a hard sandbox. Any component that requires
  network access should make this explicit in its `index.md`.

## Disclosure timeline

Once a fix is shipped, an advisory is published with:

- The CVE (if assigned by GitHub)
- Affected versions of the component
- Mitigation guidance
- Credits to the reporter (unless anonymity is requested)
