---
description: Create a new issue/task. Checks CLAUDE.md for Linear integration and defaults to updating roadmap.md if Linear is disabled.
---

# Create Issue

Fast issue capture with automatic context detection and smart routing to Linear or roadmap.

## Quick Start

```bash
/create-issue "Save button doesn't work"
/create-issue                              # I'll ask for details
```

---

## How It Works

### Phase 1: Quick Information Gathering

I'll ask concise questions to fill gaps (only what's needed):
- What's the issue/feature? (if not clear from input)
- Current behavior vs desired behavior
- Type (bug/feature/improvement) and priority

**I'll keep it brief** - you're mid-flow and want to capture this quickly. One message with 2-3 questions beats multiple back-and-forths.

### Phase 2: Configuration Check

I'll automatically read `CLAUDE.md` to check:
- `linear_enabled: true/false` (default: false if missing)
- Team ID (required if Linear enabled)
- Issue Prefix (e.g., `EXP`)

### Phase 3: Create Issue

**If Linear disabled (or missing):**
- Add to `docs/roadmap.md` Backlog section
- Create spec file if needed: `docs/technical-specs/{PREFIX}-##.md`
- Auto-increment issue number

**If Linear enabled:**
- Create Linear issue with MCP tool
- Set status to "Todo"
- Add to `docs/roadmap.md` as backup
- Apply labels: `agent`, `technical` (if applicable)

### Phase 4: Confirmation

Show you what was created:

```markdown
✅ Issue created: EXP-36

**Title:** Save button doesn't trigger form submission
**Type:** Bug
**Priority:** High
**Added to:** docs/roadmap.md (Backlog)

**Next steps:**
- Run /sprint to work on it
- Or continue with your current work
```

---

## Issue Format

I'll create a well-structured issue with:

```markdown
## TL;DR
Save button click doesn't trigger form submission on dashboard

## Current State
Clicking "Save" button does nothing - no network request, no console errors

## Expected Outcome
Button should validate form and submit to /api/expenses

## Acceptance Criteria

**Functional (pass/fail):**
- [ ] Click Save → Form validates
- [ ] Valid form → POST to /api/expenses
- [ ] Invalid form → Show error message
- [ ] Success → Redirect to expenses list

**Quality (requires testing):**
- [ ] Button shows loading state during submission
- [ ] Error messages are clear and actionable

## Relevant Files
- `views/dashboard.html` - Contains the form
- `public/js/dashboard.js` - Button click handler
- `routes/expenses.js` - API endpoint

## Notes
- May be related to recent refactoring of form validation
- Check if event listener is properly attached
```

---

## Smart Features

### Automatic File Discovery
I'll use `grep_search` to find relevant files based on the issue description.

### Context-Aware Categorization
I'll infer if it's a bug, feature, or improvement based on your description.

### Auto-Incrementing Issue Numbers
No need to manually track issue numbers - I'll find the next available number.

### Acceptance Criteria Generation
I'll suggest testable acceptance criteria based on the issue type.

---

## Acceptance Criteria Guidelines

**Include for:**
- Features with user interaction
- UI changes
- Bug fixes (how to verify it's fixed)

**Skip for:**
- Pure refactoring (no user-facing change)
- Documentation updates
- Simple config changes

**Categorize as:**

**Functional (pass/fail)** - Binary tests:
- "Click Save" → "Form submits"
- "Enter invalid email" → "Error message shows"

**Quality (requires testing)** - Subjective measures:
- "Load page" → "Page loads quickly"
- "View results" → "Results are relevant"

---

## Examples

**Quick bug capture:**
```
You: /create-issue "Save button broken"
Me: What happens when you click it?
You: Nothing
Me: ✅ Created EXP-36 - added to Backlog
```

**Feature request:**
```
You: /create-issue "Add export to CSV feature"
Me: [asks about scope, creates detailed spec]
    ✅ Created EXP-37 with acceptance criteria
```

**Mid-sprint capture:**
```
You: /create-issue "Mobile layout breaks on iPhone"
Me: [creates issue, doesn't interrupt current sprint]
    ✅ Added to Backlog - continue with current work
```

---

## Linear Integration

**If `linear_enabled: true`:**
- Creates issue in Linear with team parameter
- Sets status to "Todo"
- Applies labels: `agent`, `technical`
- Syncs to `docs/roadmap.md`

**If Linear API fails:**
- Falls back to roadmap.md only
- Adds to "Pending Manual Sync" section
- You can run `/sync-linear` later

**If `linear_enabled: false`:**
- Only updates `docs/roadmap.md`
- No Linear API calls made

---

## Rules

- **Fast**: Capture quickly so you can get back to work
- **Complete**: Include enough detail for future implementation
- **Testable**: Acceptance criteria should be verifiable
- **Non-blocking**: Linear failures don't stop issue creation
- **Auto-increment**: Issue numbers managed automatically
