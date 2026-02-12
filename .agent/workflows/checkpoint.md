---
description: Save current work state to spec file before context compaction or breaks.
---

# Checkpoint

Save current work state to the active spec file. Automatic progress tracking with git integration.

## When to Use

Checkpoint automatically when:
- After completing 15+ file edits
- Before taking a break
- Before switching tasks
- When context feels "full"
- Before major refactoring

---

## How It Works

### Phase 1: Identify Active Spec

I'll automatically find the active spec file:
1. Check recent context for which issue you're working on
2. Look for spec at `docs/technical-specs/{ISSUE_ID}.md`
3. If multiple specs are in progress, I'll ask which one to checkpoint

### Phase 2: Gather Progress

I'll collect:
- **Completed tasks**: What's done since last checkpoint
- **Key changes**: Which files were modified
- **Current state**: Where things stand right now
- **Next steps**: What should happen next
- **Git commit**: Latest commit hash (if in git repo)

### Phase 3: Update Spec File

I'll add a checkpoint section to the spec:

```markdown
## Checkpoint: 2026-02-12 10:45

**Completed:**
- Implemented material design tokens
- Updated dashboard components
- Added responsive breakpoints

**Key changes:**
- `public/css/tokens.css`: New design system variables
- `public/css/style.css`: Updated component styles
- `views/dashboard.html`: Applied new classes

**Current state:**
- 3/5 tasks complete (🟩🟩🟩🟥🟥)
- All tests passing
- Ready for visual review

**Next steps:**
- Update remaining components (invoice, settings)
- Test on mobile devices
- Deploy to staging for review

**Git:** abc123def (feat: implement material design system)

**Notes:**
- Color tokens follow Material Design 3 spec
- Breakpoints: mobile (320px), tablet (768px), desktop (1024px)
```

### Phase 4: Update Task Status

I'll update the task progress indicators:
- 🟥 (To Do) → 🟨 (In Progress) → 🟩 (Done)
- Calculate and update progress percentage

### Phase 5: Confirmation

I'll show you a summary:

```
✅ Checkpoint saved to docs/technical-specs/EXP-31.md

Summary:
- Completed: 3 tasks (design tokens, dashboard, responsive)
- In progress: Component updates
- Next: Update invoice and settings pages

Progress: 60% complete (3/5 tasks)

Context can now be safely compacted.
```

---

## Smart Features

### Automatic File Detection
I'll use `grep_search` to find which files were modified since the last checkpoint.

### Git Integration
If you're in a git repo, I'll include the latest commit hash for reference.

### Progress Calculation
Automatically calculates percentage based on completed vs total tasks.

### Context Preservation
Checkpoints survive context compaction - they're your "save points" for long sessions.

---

## When NOT to Checkpoint

Skip checkpointing if:
- You're in the middle of an incomplete thought
- No meaningful progress since last checkpoint
- Working on trivial tasks that don't need state preservation
- About to immediately continue working (no break)

---

## Checkpoint Format

**Minimal (for simple tasks):**
```markdown
## Checkpoint: 2026-02-12 10:45
- Completed: Fixed save button bug
- Next: Test on staging
```

**Detailed (for complex work):**
```markdown
## Checkpoint: 2026-02-12 10:45

**Completed:**
- [detailed list]

**Key changes:**
- [file-by-file breakdown]

**Current state:**
- [where things stand]

**Next steps:**
- [what's coming]

**Git:** [commit hash]

**Notes:**
- [important context]
```

---

## Examples

**Simple checkpoint:**
```
You: /checkpoint
Me: [finds active spec EXP-31, saves progress]
    ✅ Checkpoint saved - 3/5 tasks complete
```

**Mid-sprint checkpoint:**
```
You: /checkpoint
Me: [saves detailed checkpoint with git hash]
    ✅ Saved to EXP-32.md
    Progress: 60% (3/5 tasks)
    Latest commit: abc123d
```

**Multiple specs in progress:**
```
You: /checkpoint
Me: Found 2 active specs: EXP-31, EXP-32
    Which one to checkpoint?
You: EXP-31
Me: ✅ Checkpoint saved for EXP-31
```

---

## Rules

- **Concise**: Key facts only, not full file contents
- **Actionable**: Next steps should be clear
- **Timestamped**: Always include date and time
- **Git-aware**: Include commit hash when available
- **Progress-tracked**: Update task status indicators
