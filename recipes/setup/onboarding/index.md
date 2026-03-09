---
engine: copilot

permissions:
  contents: write
  issues: read
  pull-requests: read

imports:
  - ../../skills/onboarding.md

tools:
  github:
    toolsets: [repos]

checkpoints:
  after-detection:
    type: dialogue
    description: "Agent presents detected stack and framework, developer confirms or corrects"
    
  after-docs:
    type: approval
    description: "Developer reviews and approves generated documentation"

safe-outputs:
  create-file:
    description: "Create configuration and documentation files"
  update-config:
    description: "Update .agent/config.json with onboarding completion"
---

# Project Onboarding Recipe

This recipe guides the agent through intelligent project setup and configuration.

## Objective

Set up a new project with:
- Detected technology stack and framework
- Appropriate documentation generated from code analysis
- Proper configuration overrides
- Tool verification and setup guidance

## Workflow

1. **Detection Phase**: Analyze project structure to detect stack, framework, database, and ticket management system

2. **Checkpoint: Confirm Detection** 
   - Present findings to developer
   - Wait for confirmation or corrections
   - Adjust analysis based on feedback

3. **Tools Verification**: Check required CLI tools and MCP availability based on detected stack and configured bundle

4. **Documentation Setup**: Generate intelligent documentation based on actual code structure (not empty templates)

5. **Checkpoint: Approve Documentation**
   - Show preview of documentation to be generated
   - Get approval before creating files
   - Allow customization requests

6. **Configuration**: Apply appropriate overrides based on detected stack

7. **Finalization**: Mark onboarding as complete and provide summary

## Success Criteria

- ✅ Project stack correctly identified
- ✅ All required tools available or installation guidance provided  
- ✅ Documentation generated with real content based on codebase
- ✅ Configuration overrides applied appropriately
- ✅ Onboarding marked complete in config

## Notes

- This recipe should only run when `.agent/config.json` exists but `onboarded` is not `true`
- Focus on configuration and documentation, not code implementation
- Be conversational - ask questions when uncertain
- Generate documentation intelligently, don't copy empty templates