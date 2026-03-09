---
---

# Project Onboarding

**IMPORTANT**: This skill only executes if `.agent/config.json` exists but `onboarded` is not `true`. If you see `"onboarded": true` in the config, skip this skill entirely and proceed with normal operation.

This skill guides you through intelligent project setup and configuration. It's conversational — ask questions when you're unsure, don't work in silence.

## Phase 1: Stack Detection

### Scan Project Structure
Analyze the project files to detect:

**Technology Stack:**
- Java: Look for `pom.xml`, `build.gradle`, `gradle.properties`
- Node.js: Look for `package.json`, `package-lock.json`, `yarn.lock`
- Python: Look for `requirements.txt`, `pyproject.toml`, `setup.py`, `Pipfile`
- Flutter: Look for `pubspec.yaml`
- PHP: Look for `composer.json`
- .NET: Look for `*.csproj`, `*.sln`
- Rust: Look for `Cargo.toml`
- Go: Look for `go.mod`

**Framework Detection:**
- Spring Boot: Look for `@SpringBootApplication`, spring dependencies in config files
- React: Look for react dependencies in `package.json`
- Angular: Look for `@angular/` dependencies, `angular.json`
- Next.js: Look for `next` dependency, `next.config.js`
- Vue.js: Look for vue dependencies
- Django: Look for Django in requirements, `manage.py`
- Flask: Look for Flask in requirements
- Express: Look for express dependency

**Database Detection:**
- Look in `application.yml`, `application.properties`
- Check `docker-compose.yml` for database services
- Scan `.env` files for database URLs
- Check dependencies for database drivers (postgres, mysql, mongodb, etc.)

**Ticket Management:**
- Look for `.jira` file or Jira configurations
- Check for Linear configurations
- Check for Azure DevOps configurations
- **IMPORTANT**: Do NOT assume the ticket system from the bundle's `overrides`. Bundles provide defaults, not facts. Always detect from project files first.
- If detection finds clear evidence (e.g., `.jira` file, Jira URLs in configs), present it as a suggestion to confirm.
- If nothing is detected, **always ask the developer**: "What ticket management system do you use? (Jira / Linear / Azure DevOps / GitHub Issues / Other)"
- Only after the developer confirms should you set the override in the config.

### Present Findings
Summarize your detection results to the developer:

```
🔍 **Project Analysis**

**Technology Stack**: [Detected stack]
**Framework**: [Detected framework]
**Database**: [Detected database or "None detected"]
**Ticket Management**: [Detected system or "GitHub Issues (default)"]

**Key Files Found**:
- [List important config files you found]

**Questions**:
- [Any clarifications needed about your detection]

Is this analysis correct? Please confirm or correct any mistakes.
```

Wait for confirmation before proceeding to Phase 2.

## Phase 2: Tools and Dependencies Verification

### Read Bundle Configuration
1. Read `.agent/config.json` to get the configured bundle
2. Load the bundle YAML from the framework to see required tools and skills
3. For each skill/recipe in the bundle, check if required tools are available

### CLI Tools Check
Based on detected stack, verify these tools are available:
- `gh` (GitHub CLI) - Always required
- `git` - Always required
- `npm` or `yarn` - If Node.js detected
- `mvn` or `gradle` - If Java detected
- `pip` or `poetry` - If Python detected
- `flutter` - If Flutter detected
- `composer` - If PHP detected
- `dotnet` - If .NET detected
- `cargo` - If Rust detected
- `go` - If Go detected

### MCP Tools Check
For each MCP tool referenced in the bundle:
- Check if the tool is available in your environment
- If missing, provide specific installation instructions
- Explain what the tool is used for in the context of this project

### Override-dependent Tools Check
Based on what was confirmed in Phase 1, verify the tools needed for those choices:
- If **Jira** was confirmed → check that a Jira MCP is available (e.g., `jira-mcp`, Jira API token configured). If not, explain how to set it up.
- If **Linear** was confirmed → check that a Linear MCP is available. If not, explain how to set it up.
- If **Azure DevOps** was confirmed → check that an Azure DevOps MCP is available. If not, explain how to set it up.
- If a **database** was detected → check that any required database MCP or CLI client is available.
- **Do NOT skip this step.** The overrides confirmed in Phase 1 imply tool dependencies that must be verified here.

### Report Tool Status
```
🔧 **Tools Status**

**CLI Tools**:
✅ gh (GitHub CLI) - Available
✅ git - Available
❌ npm - Not found
   💡 Install: Run `npm install -g npm` or install Node.js

**MCP Tools**:
✅ github-standard - Available
❌ jira-mcp - Not configured
   💡 Setup: Configure Jira MCP in your agent environment

**Next Steps**:
[List what needs to be installed/configured manually]
```

## Phase 2.5: Skills Selection

### Catalog-Based Skill Recommendations

Now that tools are verified, help the developer select relevant skills for their project.

#### Read Available Skills
1. Use the `catalog-browser` skill to read the framework catalog
2. Present available skills organized by category
3. Based on the stack detected in Phase 1, **recommend** relevant skills

#### Technology-Based Recommendations

**For Java projects:**
- ✅ `coding-principles` (always recommended)
- ✅ `testing-tdd` (essential for Java development)
- ✅ `implement-feature` (structured feature workflow)
- ✅ `create-pr` (PR creation guidance)
- ✅ `security-review` (security patterns for Java/Spring)

**For Node.js/React projects:**
- ✅ `coding-principles` (always recommended)
- ✅ `testing-tdd` (adapted for Jest/Cypress/Playwright)
- ✅ `implement-feature` (feature workflow)
- ✅ `create-pr` (PR creation guidance)

**For all projects with ticket management:**
- ✅ `read-ticket` (with appropriate override from Phase 1 confirmation)

**Always recommended:**
- ✅ `coding-principles` (universal coding standards)
- ✅ `create-pr` (PR workflow)

#### Present Skills Marketplace

Show a customized marketplace view:

```
🎯 **Recommended Components for [Detected Stack]**

**Essential Skills (strongly recommended):**
📦 coding-principles — Core coding standards and principles
📦 testing-tdd — Test-driven development for [detected test frameworks]
📦 implement-feature — Structured feature implementation workflow

**For your ticket system ([confirmed system]):**
📦 read-ticket — Ticket reading with [jira/linear/azure-devops] integration

**Additional useful skills:**
📦 security-review — Security patterns for [detected frameworks]
📦 create-pr — Pull request creation and review guidance

**Recommended recipes for [Detected Stack]:**
📦 development/us-to-pr — Full ticket-to-PR workflow (with checkpoints)
📦 development/bugfix — Structured bug fixing process
📦 review/pr-review — Code review workflow

**Useful hooks for [confirmed system]:**
📦 [jira-sync/linear-sync] — Sync workflow status to your ticket system
📦 slack-notify — Send notifications to team channels
📦 update-project-board — Keep GitHub project boards in sync

**What would you like to install?**
- Type 'all essential' to install recommended essentials
- Type 'all recommended' to install skills + common recipes/hooks
- Type individual component names to install specific ones  
- Type 'browse' to see full marketplace
- Type 'skip' to proceed without additional components (you can install later)
```

#### Interactive Selection Process

1. **Let the developer choose** which components to install (skills, recipes, hooks)
2. For each selected component:
   - Use the catalog-browser installation flow (unified for all component types)
   - Add to `.agent/config.json` in appropriate section with `source: "framework"` and calculated hash
   - Provide brief description of what the component provides
3. **Handle overrides** correctly:
   - If `read-ticket` is selected, use the override determined in Phase 1
   - Store override in config: `"overrides": { "read-ticket": "jira" }`
4. **Smart recommendations** based on selections:
   - If `implement-feature` skill is selected, suggest `development/us-to-pr` recipe
   - If ticket system is Jira, suggest `jira-sync` hook
   - If team collaboration is detected, suggest `slack-notify` hook
5. **Regenerate agent files** after all selections
6. **Confirm installation summary**

#### Installation Summary

```
✅ **Component Installation Complete**

**Installed skills:**
- coding-principles (framework)
- testing-tdd (framework) 
- implement-feature (framework)
- read-ticket (framework, jira override)

**Installed recipes:**
- development/us-to-pr (framework)
- development/bugfix (framework)

**Installed hooks:**
- jira-sync (framework)
- slack-notify (framework)

**Configuration updated:**
- Added 4 skills to .agent/config.json
- Added 2 recipes to .agent/config.json
- Added 2 hooks to .agent/config.json
- Set read-ticket override: jira
- All components ready for Phase 3

**Next:** Project documentation setup
```

#### Skip Option

If developer chooses to skip:
```
⏭️ **Skipped Component Selection**

You can install components later using the catalog browser:
- Ask your agent to "browse components" or "show marketplace"
- Use the `catalog-browser` skill anytime to install skills, recipes, or hooks
- Or run `adf generate --agent [your-agent]` after manual config edits

**Next:** Project documentation setup with minimal component set
```

**Important:** This phase should integrate seamlessly with the catalog-browser skill. Use the same installation mechanisms and configuration patterns for all component types (skills, recipes, hooks).

## Phase 3: Project Documentation Setup

### Analyze Current Documentation
Check for these files in `.agent/docs/`:
- `ARCHITECTURE.md` - System architecture overview
- `CODEBASE.md` - Code organization and structure
- `DATABASE.md` - Database schema and setup
- `API.md` - API endpoints and contracts
- `CONVENTIONS.md` - Team coding conventions

### Intelligent Documentation Generation
For each missing document, **DO NOT** copy empty templates. Instead:

**For ARCHITECTURE.md:**
- Scan for architectural patterns (MVC, microservices, layered, etc.)
- Look at folder structure to understand modules/components
- Check for infrastructure files (docker-compose, kubernetes manifests)
- Pre-fill with what you actually see in the codebase

**For CODEBASE.md:**
- Analyze folder structure and document it
- Identify main entry points
- Document build/run commands based on detected tools
- List key directories and their purposes

**For DATABASE.md:**
- If database detected, scan schema files or migrations
- Document connection configuration
- List main entities/tables if you can identify them
- Include setup/migration commands

**For API.md:**
- Scan for API controllers/routes
- Look for OpenAPI/Swagger files
- Document authentication methods if visible
- List main endpoints you can identify

**For CONVENTIONS.md:**
- Check existing code for patterns (naming, formatting)
- Look for linting configs (.eslintrc, checkstyle.xml, etc.)
- Document what you observe as existing conventions

### Interactive Documentation Creation

**This phase is a CONVERSATION, not a batch job. The documentation is the foundation for everything the agent does next — both you and the developer must be fully aligned.**

#### Step 1: Present the documentation plan
Before generating anything, present an overview:

```
📚 **Documentation Plan**

Based on your project, here's what I recommend:

**To generate:**
- ARCHITECTURE.md — [brief description of what you'll document]
- CODEBASE.md — [brief description]
- CONVENTIONS.md — [brief description]

**Not applicable (skipping):**
- DATABASE.md — No database detected
- API.md — No HTTP API detected

**Do you agree with this plan?**
- Want to add a document I didn't mention? (e.g., DEPLOYMENT.md, TESTING.md, SECURITY.md)
- Want to skip one I suggested?
- Want to rename or restructure anything?
```

Wait for the developer to validate the plan. They might want additional documents specific to their project.

#### Step 2: Generate and review ONE document at a time
For each document:
1. Generate a draft based on what you see in the codebase
2. **Show the FULL content** to the developer (not just a summary)
3. Ask explicitly:

```
📝 **[FILENAME] — Draft**

[Full generated content]

---
**Review this document:**
- Is the content accurate?
- Anything missing or incorrect?
- Want me to add, remove, or rewrite sections?

Once you're satisfied, I'll save it and move to the next document.
```

4. **Iterate** — if the developer wants changes, make them and show the updated version
5. Only move to the next document when the developer explicitly approves

#### Step 3: Final documentation review
After all documents are created:

```
📋 **Documentation Summary**

Here's what we've created:
- ✅ ARCHITECTURE.md — [one-line summary]
- ✅ CODEBASE.md — [one-line summary]
- ✅ CONVENTIONS.md — [one-line summary]
- ⏭ DATABASE.md — Skipped (not applicable)
- ⏭ API.md — Skipped (not applicable)

**This documentation will be the reference for all agent operations on this project.**
Every skill and recipe will use these docs to understand your codebase, conventions, and architecture.

Is everything correct? Any final changes before we lock this in and move to Phase 4?
```

**CRITICAL: Do NOT rush through this phase. Do NOT batch-create all docs at once. The quality of this documentation directly determines the quality of every future agent interaction with this project.**

## Phase 4: Configuration Overrides

### Analyze and Propose Overrides
Based on your detections, propose appropriate overrides for `.agent/config.json`:

**Ticket Management Override:**
- Only set this override based on what the developer **confirmed** in Phase 1.
- Do NOT inherit overrides from the bundle defaults — those are generic suggestions, not project-specific configuration.
- If Jira confirmed: `"overrides": { "read-ticket": "jira" }`
- If Linear confirmed: `"overrides": { "read-ticket": "linear" }`
- If Azure DevOps confirmed: `"overrides": { "read-ticket": "azure-devops" }`
- If GitHub Issues: no override needed (default behavior)

**Technology-specific Overrides:**
- Add any stack-specific configurations based on what you detected

### Update Configuration
Show the developer what overrides you want to add:

```
⚙️  **Configuration Updates**

I'll add these overrides to `.agent/config.json`:

{
  "overrides": {
    "read-ticket": "jira",
    "database": "postgresql"
  }
}

These overrides customize the agent behavior for your detected stack.
Proceed? (y/n)
```

## Phase 5: Finalization

### Mark Onboarding Complete
Update `.agent/config.json` with:
```json
{
  "onboarded": true,
  "onboardedAt": "[ISO-8601 timestamp]"
}
```

### Summary Report
Provide a comprehensive summary:

```
🎉 **Onboarding Complete!**

**Project Configuration:**
- Stack: [detected stack]
- Bundle: [bundle name]
- Documentation: [X/5 files created]
- Overrides: [list applied overrides]

**What's Ready:**
- ✅ Agent configuration
- ✅ Project documentation
- ✅ Tool dependencies verified

**Manual Steps Required:**
- [ ] Configure JIRA_TOKEN secret in GitHub (if applicable)
- [ ] Install missing CLI tools: [list]
- [ ] Set up MCP tools: [list]

**Next Steps:**
You can now use your agent for development tasks. The onboarding will not run again unless you remove the `onboarded` flag from `.agent/config.json`.

Try running a recipe: `adf generate --agent copilot --bundle [your-bundle]`
```

## Important Behaviors

### Hard Gates
- **DO NOT** start coding or implementing features during onboarding
- **DO NOT** create pull requests or make code changes
- **ONLY** create configuration files and documentation

### Conversation Style
- Ask questions when detection is unclear
- Show previews before creating files
- Wait for confirmations on significant actions
- Explain the reasoning behind your suggestions

### Error Handling
- If bundle file is missing or invalid, explain the issue clearly
- If tools are missing, provide specific installation steps
- If directories don't exist, create them as needed

### Skip Conditions
- Skip entirely if `config.onboarded === true`
- Skip phases if equivalent work was already done
- Detect partial completion and resume from appropriate phase