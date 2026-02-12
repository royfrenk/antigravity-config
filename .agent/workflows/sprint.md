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

## Multi-Model Strategy

Antigravity allows switching models mid-conversation. Use this to leverage each model's strengths:

**Phase 1: Planning & Architecture (EM/Planner)** → **Claude 3 Opus**
- Best for: Spec creation, dependency analysis, roadmap planning
- Why: Superior reasoning and long-term planning

**Phase 2: Implementation (Developer)** → **Claude 3.5 Sonnet**
- Best for: Writing code, tests, documentation
- Why: Fast, accurate, high recall

**Phase 3: Design & Visuals (Designer)** → **Gemini 2.0 Pro**
- Best for: UI implementation, CSS, animations, responsive design
- Why: Strong visual/spatial reasoning

**Phase 4: Code Review (Tech Lead)** → **Claude 3 Opus**
- Best for: Architecture, security, patterns, refactoring
- Why: Deep understanding of complex codebases

**Phase 5: Automated Peer Review (QA)** → **Gemini 2.0 Pro**
- Best for: Final verification, visual regression check, user perspective
- Why: Strong reasoning + fresh perspective before handoff

**How to switch:**
1. I'll recommend when to switch
2. You change the model in the interface
3. We continue the workflow seamlessly

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

2. **Self-Review**
   - **Code Quality**: Check against project standards
     - Proper error handling
     - No hardcoded values (use design tokens)
     - Consistent naming conventions
     - Comments for complex logic
   - **Test Coverage**: Verify tests cover new code paths
   - **Performance**: Check for obvious performance issues

3. **Visual Review** (if UI changes)
   - **Recommended Model:** Switch to Gemini 2.0 Pro (Visual reasoning)
   - **STOP and ask you to review**
   - I'll either:
     - Open browser to show the changes live
     - Generate screenshots of before/after
     - Describe what changed visually
   - **You verify:**
     - Design matches spec/Figma
     - Responsive on mobile/tablet/desktop
     - Animations work smoothly
     - Accessibility (keyboard nav, contrast)
   - **Wait for your approval** before proceeding

4. **Code Review Gate (Tech Lead)**
   - **Recommended Model:** Switch to Claude 3 Opus
   - **STOP and present changes**
   - Show git diff summary
   - Highlight key changes and rationale
   - **You verify:**
     - Logic is correct
     - No unintended side effects
     - Follows project patterns
   - **Wait for your approval** before proceeding

5. **Automated Peer Review (QA / Staging)**
   - **Recommended Model:** Switch to Gemini 2.0 Pro
   - **Run full test suite** on staging/local environment
   - **Check for visual regressions** or unexpected behavior
   - **Verify acceptance criteria** one last time
   - **Wait for your final sign-off**

6. **Completion**
   - Update spec status: 🟨 → 🟩
   - Mark as complete in sprint file
   - Commit changes with descriptive message

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

## Review Policy

### When Reviews Are MANDATORY

**Visual Review (UI changes):**
- ✅ Any changes to HTML/CSS
- ✅ New components or layouts
- ✅ Changes to animations or interactions
- ✅ Responsive design updates

**Code Review (all changes):**
- ✅ After each task completion
- ✅ Before committing to git
- ✅ Before deploying to staging/production

**I will STOP and wait for your approval** in these cases.

### When You Can Skip Reviews

You can say **"auto-approve"** or **"skip reviews"** for:
- ❌ Simple bug fixes (typos, obvious errors)
- ❌ Documentation updates
- ❌ Test additions (no logic changes)
- ❌ Dependency updates

**But I'll still:**
- Show you a summary of changes
- Run all tests
- Verify nothing breaks

### How to Control Reviews

**During sprint:**
```
You: "Auto-approve code reviews, but stop for visual reviews"
Me: ✅ Will skip code review gates, but pause for UI changes
```

```
You: "Show me everything"
Me: ✅ Will stop at every review gate
```

```
You: "Skip all reviews for this task"
Me: ✅ Will implement, test, and commit without pausing
```

**Default behavior:** Stop at both visual and code review gates.

---

## Examples

**Simple feature:**
```
You: /sprint EXP-42
Me: [loads context, creates spec, shows plan]
You: Looks good
Me: [implements task 1, STOPS for code review]
    Here's what I changed: [shows diff]
You: Approved
Me: [commits, continues to task 2]
```

**With auto-approve:**
```
You: /sprint EXP-42 --auto-approve
Me: [loads context, creates spec, shows plan]
You: Looks good
Me: [implements all tasks, shows summaries, auto-commits]
    ✅ Completed 5 tasks, all tests passing
```

**UI changes:**
```
You: /sprint EXP-32
Me: [implements Material Design changes, STOPS for visual review]
    I've updated the navigation drawer. Opening browser...
    [shows live preview]
You: Looks good, but can you make the pills more rounded?
Me: [adjusts border-radius, shows again]
You: Perfect, approved
Me: [commits and continues]
```

---

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
