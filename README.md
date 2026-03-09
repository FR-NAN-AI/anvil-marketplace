# anvil-marketplace

Official component marketplace for [@fr-nan-ai/anvil](https://github.com/FR-NAN-AI/anvil).

## Structure

```
skills/          AI agent skills (coding, testing, security, etc.)
hooks/           Workflow hooks (Jira sync, Slack notify, etc.)
agents/          Autonomous agents (requirement analyzer, task executor)
tools/           Tool configurations (GitHub, bash, web)
recipes/         Multi-step workflows (dev, review, management)
bundles/         Curated component sets (java-dev, react, tech-lead)
catalog.yaml     Component registry
```

## Usage

Components are consumed by the anvil CLI:

```bash
# Browse available components
anvil marketplace list

# Install a skill
anvil install skill coding-principles

# Install from a bundle
anvil init --agent copilot --preset java-developer
```

## Contributing

Each component is a directory with:
- `component.yaml` — metadata (name, description, type, dependencies)
- `index.md` — the component content

To add a new component:
1. Create the directory under the right type folder
2. Add `component.yaml` and `index.md`
3. Register it in `catalog.yaml`
4. Open a PR

## License

MIT — see [LICENSE](LICENSE)
