---
description: Run the engineering sprint autonomously. Reads the roadmap and executes Priority 1 task with spec-first workflow.
---

# Autonomous Sprint Execution

Execute engineering sprints autonomously with automatic context loading, spec creation, and verification.

## Quick Start

```bash
/sprint                    # Auto-select from roadmap
/sprint EXP-42            # Specific issue
/sprint EXP-42 EXP-43     # Multiple issues
```

---

## How It Works

### Phase 1: Configuration & Context

I'll automatically:
1. **Read `CLAUDE.md`** to check Linear integration (`linear_enabled: true/false`)
2. **Load project state** from `docs/PROJECT_STATE.md`
3. **Check for active sprint** using `find docs/sprints/ -name "*.active.md"`
4. **Sync with Linear** (if enabled) using `/sync-roadmap`

### Phase 2: Issue Selection

**If you provided issue IDs:**
- I'll use those directly

**If not:**
- I'll check `docs/roadmap.md` for "Todo" items
- Show you the list and ask which to work on
- Default to Priority 1 if available

### Phase 3: Sprint File Management

I'll check for active sprints:
- **None found**: Create new sprint file `docs/sprints/sprint-###-[name].active.md`
- **One found**: Resume it
- **Multiple found**: Ask you which one to continue

Sprint number auto-increments based on existing files.

### Phase 4: Technical Spec Creation

For each issue, I'll check if `docs/technical-specs/{PREFIX}-##.md` exists.

**If missing**, I'll create it with:
1. **Codebase Analysis**: Use `grep_search` and `view_file_outline` to understand the codebase
2. **Dependency Mapping**: Check which files need changes
3. **Implementation Plan**: Break down into tasks with status indicators (🟥/🟨/🟩)

**Spec Template:**
```markdown
# {PREFIX}-##: [Title]

**Status:** 🟨 In Progress

## Summary
[What this issue is about]

## Implementation Plan

### Wave 1: Foundation
- [ ] 🟥 Task 1: [description]
  - Files: `path/to/file.js`
  - Why: [rationale]

### Wave 2: Integration
- [ ] 🟥 Task 2: [description]
  - Files: `path/to/file.js`
  - Depends on: Task 1

## Acceptance Criteria
- [ ] [User-testable outcome]
```

### Phase 5: Plan Approval

I'll present the implementation plan and wait for your approval:
```
## Implementation Plan for EXP-42

**Waves:** 2
**Total Tasks:** 5
**Estimated Complexity:** Medium

### Wave 1: Database Changes
- Update schema in utils/database.js
- Add migration script

### Wave 2: API Implementation
- Create new endpoint in routes/
- Add validation middleware

**Approve to proceed?**
```

### Phase 6: Execution (The Build Loop)

For each task:

1. **Implementation**
   - Update spec status: 🟥 → 🟨
   - Make code changes using `multi_replace_file_content` or `write_to_file`
   - Run verification: `npm run lint && npm test`

2. **Review Gate**
   - **Visual Review** (if UI changes): Ask you to verify
   - **Code Review**: Check against coding standards
   - **Test Coverage**: Ensure tests pass

3. **Completion**
   - Update spec status: 🟨 → 🟩
   - Mark as complete in sprint file
   - Commit changes

### Phase 7: Pre-Handoff Verification

Before marking the sprint complete:
- ✅ Run full test suite
- ✅ Verify all acceptance criteria met
- ✅ Check for unintended file changes
- ✅ Ensure build succeeds

### Phase 8: Completion & Sync

1. **Update Linear** (if enabled) with status
2. **Update `docs/roadmap.md`** to reflect completion
3. **Rename sprint file**: `.active.md` → `.done.md`
4. **Summary report** with what was accomplished

---

## Smart Features

### Automatic Context Loading
No need to manually load project state - I'll read all necessary files in parallel.

### Dependency Detection
I'll analyze task dependencies and execute in the right order (waves).

### File Conflict Detection
If multiple tasks modify the same file, I'll warn you and adjust the plan.

### Incremental Verification
Tests run after each task, not just at the end.

### Resume Support
If a sprint is interrupted, I can resume from the last checkpoint in the spec file.

---

## Sprint File Format

```markdown
# Sprint 006: Material Design

**Status:** 🟨 In Progress
**Started:** 2026-02-12
**Issues:** EXP-31, EXP-32

## Issues in Sprint

| Issue  | Title                    | Spec                                 | Status       |
| ------ | ------------------------ | ------------------------------------ | ------------ |
| EXP-31 | Material Design System   | [spec](../technical-specs/EXP-31.md) | 🟩 Done      |
| EXP-32 | Update Dashboard UI      | [spec](../technical-specs/EXP-32.md) | 🟨 In Review |

## Progress

- [x] EXP-31: All tasks complete, deployed to staging
- [ ] EXP-32: 3/5 tasks complete

## Notes
- Material Design tokens defined in public/css/tokens.css
- Dashboard components updated to use new system
```

---

## Linear Integration

**If `linear_enabled: true` in CLAUDE.md:**
- I'll sync status updates to Linear automatically
- Post comments on issues with progress
- Update issue status as tasks complete

**If `linear_enabled: false`:**
- All tracking happens in `docs/roadmap.md`
- No Linear API calls made

---

## Error Handling

**If tests fail:**
- I'll show you the error
- Propose a fix
- Re-run tests after fixing

**If Linear sync fails:**
- I'll continue with the sprint
- Add to "Pending Manual Sync" section
- You can run `/sync-linear` later

**If you need to pause:**
- Just say "pause" or "stop"
- I'll checkpoint current progress to the spec file
- You can resume later with `/sprint` (I'll detect the active sprint)

---

## Examples

**Simple feature:**
```
You: /sprint EXP-42
Me: [loads context, creates spec, shows plan]
You: Looks good
Me: [implements, tests, deploys, marks complete]
```

**Multi-issue sprint:**
```
You: /sprint EXP-42 EXP-43 EXP-44
Me: [analyzes dependencies, proposes execution order]
You: Approve
Me: [executes in waves, reports progress]
```

**Resume interrupted sprint:**
```
You: /sprint
Me: Found active sprint-006. Last checkpoint: EXP-32 task 3/5 complete. Continue?
You: Yes
Me: [resumes from checkpoint]
```

---

## Rules

- **Spec-first**: Never implement without a technical spec
- **Plan approval**: Always wait for your approval before coding
- **Incremental verification**: Test after each task
- **Checkpoint frequently**: Save progress to spec files
- **Non-blocking**: Linear failures don't stop the sprint
