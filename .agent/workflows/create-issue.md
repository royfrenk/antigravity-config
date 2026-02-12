---
description: Create a new issue/task. Checks CLAUDE.md for Linear integration and defaults to updating roadmap.md if Linear is disabled.
---

# Create Issue

User is mid-development and thought of a bug/feature/improvement. Capture it fast so they can keep working.

**Input:** Any arguments provided by the user (e.g., title, description)

## Goal

Create a complete task/issue with:

- Clear title
- TL;DR of what this is about
- Current state vs expected outcome
- **Acceptance criteria** (for features/UI - user-testable actions and expected results)
- Relevant files that need touching (max 3)
- Risk/notes if applicable
- Proper type/priority/effort labels

## Phase 1: Information Gathering

**Ask questions** to fill gaps - be concise, respect the user's time. They're mid-flow and want to capture this quickly. Usually need:

- What's the issue/feature (if not clear from input)
- Current behavior vs desired behavior
- Type (bug/feature/improvement) and priority if not obvious

Keep questions brief. One message with 2-3 targeted questions beats multiple back-and-forths.

## Phase 2: Configuration Check & Execution

Before creating the issue, check project configuration:

1. **Read CLAUDE.md** to extract Linear settings:
   - Look for "Linear Integration" section
   - Extract `linear_enabled: true/false` (default: false if missing)
   - Extract `Team ID: <uuid>` (required if enabled)
   - Extract `Issue Prefix: <PREFIX>`

2. **Determine behavior:**
   - If `linear_enabled: false` or missing → Skip Linear, only update `docs/roadmap.md`
   - If `linear_enabled: true` and Team ID missing → Error: "Linear enabled but Team ID not found in CLAUDE.md"
   - If `linear_enabled: true` and Team ID present → Attempt to create Linear issue. Note: Check if Linear MCP tool is available first.

### Workflow A: Roadmap.md Only (linear_enabled: false)

1. **Edit `docs/roadmap.md`**:
   - Add the new task to the appropriate section (e.g., Backlog or Current Sprint).
   - Use the format defined in `roadmap.md` (usually `- [ ] Task description`).
   - If a detailed spec is needed, create a new file in `docs/technical-specs/EXP-##.md` (incrementing the number) and link it in the roadmap.

### Workflow B: Linear + Roadmap.md (linear_enabled: true)

1. **Create Linear Issue**:
   - Use available Linear MCP tools (e.g., `linear_create_issue`).
   - Include team parameter from CLAUDE.md.
   - Set title, description, and labels.
2. **Update Status**:
   - Set status to Todo.
3. **Update Roadmap**:
   - Add the new Linear issue link/ID to `docs/roadmap.md`.

## Issue Body Format (for specs or descriptions)

```markdown
## TL;DR

[One sentence summary]

## Current State

[What happens now / what's missing]

## Expected Outcome

[What should happen / what we want]

## Acceptance Criteria

**Functional (pass/fail):**

- [ ] [Action] → [Expected result]
- [ ] [Action] → [Expected result]

**Quality (requires evals):**

- [ ] [Action] → [Quality outcome - will be expanded into detailed evals during planning]

## Relevant Files

- `path/to/file1.ts` - [why relevant]
- `path/to/file2.ts` - [why relevant]

## Notes

[Any risks, dependencies, or considerations - omit if none]
```
