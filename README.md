# anvil-marketplace

Official component marketplace for [@fr-nan-ai/anvil](https://github.com/FR-NAN-AI/anvil).

Generic, project-agnostic components that work with any tech stack.

## What's Inside

### Skills
| Name | Description |
|------|-------------|
| `coding-principles` | Universal coding principles — DRY, YAGNI, KISS, SOLID |
| `test-driven-development` | RED-GREEN-REFACTOR cycle |
| `systematic-debugging` | 4-phase root cause analysis |
| `verification-before-completion` | Evidence before claims |
| `security-review` | Security review checklist |
| `implement-feature` | Feature implementation workflow |
| `create-pr` | Pull request creation guidance |
| `onboarding` | Intelligent project setup |
| `anvil-cli-reference` | Complete anvil CLI reference |
| `catalog-browser` | Browse and install marketplace components |
| `frontend-design` | Distinctive, production-grade frontend interfaces |

### Recipes
| Name | Description |
|------|-------------|
| `development/us-to-pr` | Issue to PR workflow (with checkpoints) |
| `development/bugfix` | Structured bugfix workflow |
| `review/pr-review` | PR review workflow |
| `review/post-merge-check` | Post-merge health check |

### Agents
| Name | Description |
|------|-------------|
| `requirement-analyzer` | Analyzes tickets, produces structured briefs |
| `task-executor` | Implements tasks, delivers tested changes |
| `code-reviewer` | Reviews completed work against plan and standards |
| `architect` | Analyzes architecture, proposes patterns, evaluates trade-offs |
| `quality-guardian` | Audits quality — complexity, duplication, debt, coverage |
| `security-auditor` | Security audit — OWASP, injections, secrets, dependencies |

### Tools
| Name | Description |
|------|-------------|
| `github-standard` | Standard GitHub toolsets |
| `bash-dev` | Developer bash tools |
| `web-tools` | Limited web access |

### Bundles
| Name | Description |
|------|-------------|
| `starter` | Minimal — coding-principles, create-pr, onboarding |
| `full-stack` | Complete — all skills, recipes, agents, tools |
| `tech-lead` | Quality, reviews, and process |

## Usage

```bash
# Browse available components
anvil catalog --json

# Install a skill
anvil install skill test-driven-development --json

# Initialize with a bundle
anvil init --agent copilot --preset starter
```

## Structure

```
skills/          AI agent skills
agents/          Autonomous agents
tools/           Tool configurations
recipes/         Multi-step workflows
bundles/         Curated component sets
catalog.yaml     Component registry
```

## Contributing

Each component is a directory with:
- `component.yaml` — metadata (name, description, type, targets)
- `index.md` — the component content

To add a new component:
1. Create the directory under the right type folder
2. Add `component.yaml` and `index.md`
3. Register it in `catalog.yaml`
4. Open a PR

## License

MIT — see [LICENSE](LICENSE)
