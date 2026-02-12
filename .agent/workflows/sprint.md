---
description: Run the engineering sprint autonomously. Reads the roadmap and executes Priority 1 task with spec-first workflow.
---

# Autonomous Sprint Execution

Run the engineering sprint autonomously. Reads Linear for Priority 1 task and executes with proper spec creation and plan approval.

> **Tip:** Clear context before starting a new sprint to maximize available context.

## Linear Configuration

I will check `CLAUDE.md` for Linear integration settings:

- `linear_enabled: true` → Use Linear for issue tracking, sync bidirectionally with roadmap.md
- `linear_enabled: false` → Use roadmap.md only, skip all Linear MCP calls
- Missing field → Default to `false` (roadmap.md only)

**If Linear is disabled:**

- Skip all Linear sync attempts
- Use `docs/roadmap.md` as single source of truth

**If Linear is enabled:**

- Use `/sync-roadmap` (if available) for bidirectional sync
- Follow existing workflow with soft retry logic

## Workflow

1. **Read Configuration**:
   - Read `CLAUDE.md` to get `linear_enabled` status and settings.

2. **Sync & Select Issues**:
   - If enabled, run sync.
   - **Selection**:
     - If issue IDs provided (e.g., `/sprint QUO-57`), use them.
     - If not, check `docs/roadmap.md` (or Linear if enabled) for "Todo" items.
     - If "Todo" items found, ask to confirm.
     - If no items, ask user what to work on.

3. **Sprint File Management**:
   - **Check for active sprint**: `find docs/sprints/ -name "*.active.md"`
   - **If multiple**: Error, ask user to resolve.
   - **If one exists**: Resume it.
   - **If none**: Create new sprint file (e.g., `docs/sprints/sprint-006-feature.active.md`).
     - Calculate next sprint number based on existing files in `docs/sprints/`.

4. **Technical Spec Check**:
   - For each issue, check if `docs/technical-specs/{PREFIX}-##.md` exists.
   - **If missing**, create it:
     - **Analyze**: I will analyze the codebase to understand requirements.
     - **Write Spec**: Create the file with "Summary", "Exploration", and "Implementation Plan". (See Template below).

5. **Implementation Plan Checking**:
   - **Analyze Dependencies**: Check if tasks depend on each other.
   - **File Conflicts**: Check if tasks modify the same files.
   - **Create Execution Plan**: Update the spec with a plan (Waves of tasks).
   - **Approval**: Present the plan to the user for approval.

6. **Execution (The Build Loop)**:
   - **For each task in the plan**:
     - **Implementation**:
       - Update spec status (🟥 -> 🟨).
       - Implement code changes.
       - **Verification**: Run build, lint, tests.
     - **Review Gate**:
       - **Visual/Design Review**: If UI changes, ask user to verify or check against design.
       - **Code Review**: I (the agent) will review the code against `rules/coding-style.md` and `rules/security.md`.
       - **User Review**: Ask user for confirmation/approval before "deploying" (merging/finishing task).
     - **Completion**:
       - Update spec status (🟨 -> 🟩).
       - Mark as deployed/done in sprint file.

7. **Pre-handoff Verification**:
   - Run full test suite.
   - Verify all acceptance criteria are met.

8. **Completion**:
   - **Sync**: Update Linear (if enabled) and `docs/roadmap.md`.
   - **Close Sprint**:
     - Rename `.active.md` to `.done.md`.
     - Update `docs/roadmap.md` (move to Completed).

## Sprint File Template

```markdown
# Sprint [###]: [Name]

**Status:** 🟨 In Progress
**Started:** [date]
**Issues:** [QUO-##]

## Issues in Sprint

| Issue  | Title   | Spec                                 | Status       |
| ------ | ------- | ------------------------------------ | ------------ |
| QUO-## | [Title] | [spec](../technical-specs/QUO-##.md) | 🟨 In Review |

## Iteration Log

[Log of bugs/fixes]

## Notes
```

## Technical Spec Template

```markdown
# {PREFIX}-##: [Title]

**Status:** 🟨 In Progress

## Summary

[Description]

## Implementation Plan

- [ ] 🟥 Task 1: [description]
```

## Rules

- **Spec-first**: Never implement without a technical spec.
- **Plan approval**: Wait for user approval of the plan.
- **Verification**: Run tests before marking tasks complete.
- **Review**: Always review code against rules.
