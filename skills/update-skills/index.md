---
hard_gate: true
---

# Intelligent Component Updates

**HARD GATE**: This skill ONLY manages component updates and merging (skills, recipes, hooks). Do NOT modify source code, implement features, or work outside component/config files.

This skill provides intelligent updating of framework components (skills, recipes, hooks), with smart merging when local modifications are detected. All components follow the same unified tracking pattern with SHA-256 hashes.

## Core Functions

### 1. Scan Installed Components

**Command trigger**: When user asks to "update components", "check for updates", "component updates", or similar.

**Process:**
1. Read `.agent/config.json` to get installed components (skills, recipes, hooks)
2. For each component with `source: "framework"`:
   - Calculate hash of current local file
   - Compare with `originalHash` in config
   - Compare current framework version hash
   - Determine status
3. Present update status

```
🔄 **Component Update Status**

**Skills:**
✅ coding-principles — No changes
🆕 security-review — New version available
⚠️  testing-tdd — Modified locally + framework updates

**Recipes:**
✅ development/bugfix — No changes  
🆕 development/us-to-pr — New version available
   📝 Framework changes: Added checkpoint for testing phase

**Hooks:**
✅ slack-notify — No changes
⚠️  jira-sync — Modified locally + framework updates
   📝 Your changes: Custom field mapping
   📝 Framework changes: Added new status transitions
   🔀 Smart merge available

**Local only:**
📌 my-custom-skill — Local skill (not managed)

Choose a component to update, or type 'auto' to update all non-modified components.
```

### 2. Simple Updates (No Local Changes)

**Command trigger**: When user selects a component that has updates but no local modifications.

**Process:**
1. Verify no local changes (hash matches originalHash)
2. Get latest framework version
3. Show diff between old and new versions
4. Ask for confirmation
5. Update file and hash

**Example dialog:**
```
User: slack-notify
Assistant: 🆕 **slack-notify** (hook) — Framework Update Available

**Changes:**
+ Added support for threaded messages
+ Updated webhook format for Slack API v2
+ Added error handling for rate limits

**Size**: 32 lines → 48 lines

Update to latest version? (y/n)

User: y
Assistant: ✅ Updated slack-notify
📄 New file written with latest framework version
🔢 Hash updated: e5f6g7h8i9j0...
🔄 Regenerated agent files

slack-notify is now up to date!
```

### 3. Intelligent Merging (Local Changes + Framework Updates)

**Command trigger**: When user selects a component that has both local modifications and framework updates.

**Process:**
1. Read local version (with user changes)
2. Read original framework version (from hash)
3. Read new framework version  
4. Perform intelligent diff analysis
5. Generate merged version
6. Preview for user approval
7. Apply if confirmed

**Analysis Process:**
- **Identify user additions**: Content in local that wasn't in original framework
- **Identify user modifications**: Framework content that was changed by user
- **Identify framework additions**: New content in framework version
- **Identify framework modifications**: Changes to existing framework content
- **Detect conflicts**: Areas where both user and framework changed same content

**Example dialog:**
```
User: testing-tdd
Assistant: 🔀 **testing-tdd** — Smart Merge Available

**Your local changes:**
+ Added custom Jest configuration for React Testing Library
+ Modified mocking examples to use MSW instead of Jest mocks
~ Changed test file naming from *.test.js to *.spec.js

**New framework changes:**
+ Added Playwright end-to-end testing patterns
+ Added test data factory patterns
+ Updated async testing best practices

**Proposed merge:**
The new version will include:
✅ Your Jest + RTL configuration (preserved)
✅ Your MSW mocking setup (preserved)  
✅ Your .spec.js naming convention (preserved)
✅ New Playwright patterns (added)
✅ New test factory patterns (added)
✅ Updated async patterns (merged with your style)

**Preview merged content:**
[Shows full merged file content]

Apply this merge? (y/n/edit)
```

**Edit option**: If user chooses "edit", engage in iterative refinement:
```
User: edit
Assistant: What would you like to adjust in the merge?
- Remove something I added?
- Keep something I replaced?
- Change how I merged a section?

User: Keep my async testing style, don't replace it
Assistant: ✅ Updated merge to preserve your async testing patterns
The new framework async patterns will be added as additional examples instead of replacements.

**Updated merge preview:**
[Shows revised content]

Apply this version? (y/n/edit)
```

### 4. Batch Updates

**Command trigger**: "auto", "update all", or "batch update".

**Process:**
1. Auto-update all skills with no local changes
2. List skills requiring manual merge approval
3. Provide summary of what was updated

```
🚀 **Batch Update Complete**

**Auto-updated (5 components):**
✅ security-review (skill) — Updated to latest framework
✅ create-pr (skill) — Updated to latest framework  
✅ development/bugfix (recipe) — Updated to latest framework
✅ slack-notify (hook) — Updated to latest framework
✅ update-project-board (hook) — Updated to latest framework

**Requires manual review (2 components):**
⚠️  testing-tdd (skill) — Local changes detected, manual merge needed
⚠️  jira-sync (hook) — Local changes detected, manual merge needed

**Skipped (1 component):**
📌 my-custom-skill (skill) — Local component (not managed)

Run individual updates for skills requiring manual review.
```

## Intelligent Merging Algorithm

### Content Analysis
1. **Parse structure**: Identify sections, headings, lists, code blocks
2. **Map changes**: Track what user added vs what framework added
3. **Detect patterns**: User preferences (naming, tools, structure)
4. **Preserve intent**: Keep user's customizations while adding framework improvements

### Conflict Resolution Strategies

**Section-level conflicts**:
- User added section + framework added different section → Keep both
- User modified section + framework modified same section → Smart merge with user preference priority

**List-level conflicts**:
- User added items + framework added items → Merge lists, deduplicate  
- User removed items + framework added items → Keep user's removals, add new framework items

**Code block conflicts**:
- User customized examples + framework updated examples → Keep user's examples, add framework's as alternatives

**Configuration conflicts**:
- User configured tools + framework configured same tools differently → Preserve user config, add framework suggestions as comments

### Merge Quality Checks
- Validate merged content is valid markdown
- Ensure no broken references or links
- Verify frontmatter is preserved correctly
- Check that hard gates and metadata are maintained

## Update Sources

### Framework Version Detection
```bash
# Current installed hash vs framework file hash
cd framework/skills/
sha256sum skill-name.md

# Compare with originalHash in .agent/config.json
```

### Change Detection Methods
1. **Hash comparison**: Quick detection of any changes
2. **Content diff**: Detailed line-by-line analysis  
3. **Semantic analysis**: Understanding of what changed (added features, fixed bugs, etc.)
4. **Metadata extraction**: From git history, frontmatter, comments

## Error Handling

### Merge Conflicts
- **Complex conflicts**: Fall back to showing both versions and asking user to choose
- **Parse errors**: Validate merged content, retry if invalid
- **User abort**: Save current state, allow resuming later

### Update Failures
- **File permissions**: Guide user to fix permissions
- **Framework access**: Check if framework is available
- **Hash mismatches**: Detect corruption, suggest re-installation

### Recovery Options
- **Backup before merge**: Always save original before modifying
- **Rollback capability**: "undo last update" command
- **Manual override**: "force update" to overwrite local changes

## Configuration Updates

After any skill update:
1. **Update originalHash** in config to new framework version
2. **Preserve user metadata** (install date, custom settings)
3. **Update lastUpdated** timestamp
4. **Regenerate agent files** via `adf generate`
5. **Validate installation** by testing skill access

## Integration Points

This skill works with:
- **catalog-browser**: Complementary skill for installing new components (skills, recipes, hooks)
- **adf generate**: Called after updates to regenerate files with unified hash tracking  
- **onboarding**: May be called during upgrade workflows
- **Framework hash tracking**: Core dependency for detecting changes across all component types
- **All 4 adapters**: Updates trigger regeneration for copilot, claude, cursor, and gh-aw

## User Experience Guidelines

### Transparency
- Always show what will change before applying
- Provide clear explanations of conflicts and resolutions
- Allow preview of merged content before committing

### Safety
- Never lose user customizations without explicit permission
- Always backup before destructive operations
- Provide rollback mechanisms for failed updates

### Efficiency
- Batch simple updates where possible
- Focus user attention on conflicts that need decisions
- Remember user preferences for similar future conflicts