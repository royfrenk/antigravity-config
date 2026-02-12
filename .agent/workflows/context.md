---
description: Load project context from CLAUDE.md and check for active sprints.
---

# Load Project Context

Automatically load project context with parallel file reading and smart sprint detection.

## Quick Start

```bash
/context                  # Load current project
/context expensinator     # Load specific project
```

---

## How It Works

### Phase 1: Discover Project Files

I'll automatically find and read in parallel:
1. **`CLAUDE.md`** - Project configuration and workflows
2. **`docs/PROJECT_STATE.md`** - Current codebase architecture
3. **`docs/roadmap.md`** - Task tracking and sprint status
4. **Active sprint files** - Using `find docs/sprints/ -name "*.active.md"`

### Phase 2: Validate Structure

I'll check for missing folders and create them if needed:
- `docs/technical-specs/` - For spec files
- `docs/sprints/` - For sprint iteration tracking

If missing, I'll create them with `.gitkeep` files.

### Phase 3: Sprint Detection

I'll search for active sprints and analyze their state:

**If active sprint found:**
- Read sprint file and linked specs
- Check last check-in timestamp
- Compare with spec file checkpoints
- Identify what's in progress vs completed

**If gaps detected:**
```
⚠️ Active sprint detected with potential gaps:

**Last Check-in:** 2026-02-12 09:30 - EXP-32 task 3/5 complete
**Sprint Status:** In Progress

**Resume Instructions:**
1. Review last checkpoint in spec file
2. Check what's deployed to staging
3. Use /iterate to continue if in testing
4. Use /sprint to continue if in development

**Pending Linear Syncs:**
- EXP-31: Status update failed
- Run /sync-linear to reconcile
```

**If multiple active sprints:**
- List them all
- Ask which one to focus on

**If no active sprints but specs show "In Progress":**
- Warn: "Orphaned spec files detected"
- Ask if you want to create a sprint file

### Phase 4: Summary Report

I'll provide a concise summary:

```markdown
## Project Context Loaded: Expensinator

**Project Type:** Full-stack expense management
**Stack:** Node.js + Express + SQL.js
**Linear:** Disabled (using roadmap.md)

### Current State
- **Active Sprint:** sprint-006-material-design
- **Issues in Progress:** EXP-31 (Done), EXP-32 (3/5 tasks)
- **Last Updated:** 2026-02-12

### Quick Links
- [Project State](docs/PROJECT_STATE.md)
- [Roadmap](docs/roadmap.md)
- [Active Sprint](docs/sprints/sprint-006-material-design.active.md)

### What's Next?
- Continue sprint with /iterate
- Check pending Linear syncs with /sync-linear
- Review completed work in staging
```

---

## Smart Features

### Parallel File Reading
All project files are read simultaneously for faster context loading.

### Automatic Sprint Detection
Uses `find` command to discover active sprints without hardcoded paths.

### Gap Detection
Compares sprint file timestamps with spec checkpoints to identify stale state.

### Orphan Detection
Warns if spec files show work in progress but no sprint file exists.

---

## Troubleshooting

### If no active sprint detected but work is in progress

I'll automatically:
1. Check for sprint files: `ls -la docs/sprints/*.active.md`
2. Check spec files for "In Progress" status
3. Ask: "Should I create a sprint file for these issues?"

### If sprint file shows stale status

I'll use spec files as source of truth:
- Read latest checkpoints from specs
- Show discrepancy between sprint file and specs
- Offer to update sprint file

### If CLAUDE.md not found

I'll list what's in the current directory and ask:
- "Is this the right project?"
- "Should I help set up the project structure?"

---

## What Gets Loaded

**Always:**
- `CLAUDE.md` - How to operate on this project
- `docs/PROJECT_STATE.md` - Current architecture
- `docs/roadmap.md` - Task tracking

**If exists:**
- Active sprint files
- Technical spec files for in-progress issues
- Pending sync information

**Never loaded:**
- `.env` files (sensitive)
- `node_modules/` (too large)
- Build artifacts

---

## Examples

**Simple context load:**
```
You: /context
Me: [loads all files in parallel, shows summary]
    Ready to work on Expensinator!
```

**Resume interrupted work:**
```
You: /context
Me: Found active sprint-006 with gaps detected.
    Last work: EXP-32 task 3/5 complete.
    Continue with /iterate?
```

**New project setup:**
```
You: /context
Me: CLAUDE.md not found. This looks like a new project.
    Run /new-project to set up the structure?
```

---

## Rules

- **Non-blocking**: Missing files don't stop context loading
- **Parallel**: Read all files simultaneously
- **Smart defaults**: Create missing folders automatically
- **Clear summary**: Always end with actionable next steps
