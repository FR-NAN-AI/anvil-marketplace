---
hard_gate: "Do NOT start brainstorming, planning, or implementing. Your only output is a structured brief."
---

# Read Ticket (Base)

Use this skill to parse incoming work requests consistently.

## Extract
- Title and summary
- Acceptance criteria  
- Constraints (performance, security, API contracts)
- Dependencies and stakeholders
- Non-goals and out-of-scope items

**📚 Read project context first**: Before analysis, check if these docs exist and read them for context:
- `.agent/docs/ARCHITECTURE.md` - System architecture and components
- `.agent/docs/API.md` - API endpoints and contracts
- `.agent/docs/DATABASE.md` - Database schema and constraints

## Clarify
- Ask targeted questions if acceptance criteria are missing or ambiguous.
- Confirm assumptions explicitly before implementation.

## Complexity Scoring (S/M/L)
Use acceptance criteria count, dependencies, and affected modules. Reference `.agent/docs/ARCHITECTURE.md` if available to understand module boundaries:
- **S (Small)**: 1-3 criteria, no external dependencies, 1 module.
- **M (Medium)**: 4-7 criteria, 1-2 dependencies, 2-3 modules.
- **L (Large)**: 8+ criteria, 3+ dependencies, 4+ modules or cross-team work.

## Structured Brief Template
Use this format in responses:

```
Title:
Summary:

Acceptance Criteria:
- ...

Constraints:
- ...

Dependencies:
- ...

Out of Scope:
- ...

Risks / Unknowns:
- ...

Complexity: S|M|L (why)
```

## Definition of Done Checklist
- Acceptance criteria mapped to tests
- Impacted modules identified
- Risks and assumptions documented
- Rollout or migration steps noted (if applicable)
- Stakeholders or reviewers identified

## Ambiguity Detection Heuristics
- Vague verbs (e.g., "improve", "optimize", "handle") without success metrics
- Missing error states or edge case behavior
- Unspecified data sources or payload formats
- Unclear ownership of dependencies or APIs
- Conflicting requirements in the same ticket

## Output
- Provide a structured brief with the items above.
- List risks and unknowns.

## Hard Gate
- Do NOT start brainstorming, planning, or implementing.
- Your only output is a structured brief.
- Do not write code, create branches, or propose solutions.
