---
description: Set up documentation structure for a new project.
---

# New Project Setup

Automatically set up documentation structure for a new project with interactive templates.

## Quick Start

```bash
/new-project                    # Interactive setup
/new-project my-app             # With project name
```

---

## How It Works

### Phase 1: Gather Information

I'll ask you a few questions:
- Project name
- Tech stack (frontend, backend, database)
- Linear integration (yes/no)
- Deployment setup (staging/production URLs)

### Phase 2: Create Structure

I'll automatically create:

```
project/
├── CLAUDE.md                    # How to work on this project
└── docs/
    ├── PROJECT_STATE.md         # Current codebase state
    ├── roadmap.md               # Task tracking
    ├── technical-specs/         # Spec files per issue
    │   └── .gitkeep
    └── sprints/                 # Sprint iteration tracking
        └── .gitkeep
```

### Phase 3: Populate Templates

I'll fill in the templates with your project-specific information:
- Project name and tech stack
- Linear configuration (if enabled)
- Deployment URLs
- Git workflow

### Phase 4: Confirmation

I'll show you what was created and next steps.

---

## Documentation Structure

### CLAUDE.md (Root)

**Purpose:** Entry point for Antigravity. How to operate on this project.
**Updates:** Rarely. Only when workflows or tooling change.

**Contains:**
- Quick start (points to PROJECT_STATE.md)
- Running commands (dev server, tests)
- Working with Antigravity (/sprint, /iterate, /create-issue)
- Linear integration (team, issue prefix) - optional
- Before you commit checklist

**Does NOT contain:**
- File structure (goes in PROJECT_STATE.md)
- Database schema (goes in PROJECT_STATE.md)
- API endpoints (goes in PROJECT_STATE.md)
- Key technical decisions (goes in PROJECT_STATE.md)
- Known issues (goes in Linear or roadmap.md)

### docs/PROJECT_STATE.md

**Purpose:** Current state of the codebase. Living reference for Antigravity.
**Updates:** After every deployment or major change.

**Contains:**
- File structure
- Database schema
- API endpoints
- Tech stack
- Key technical decisions
- Infrastructure/deployment config
- Recent changes (last 10)
- Supported features

### docs/roadmap.md

**Purpose:** Index of all technical specs. Mirrors Linear or standalone.
**Updates:** When issues are created, completed, or status changes.

**Contains:**
- Active sprint tasks with status
- Backlog items
- Completed items (recent)
- Links to spec files

**Use as:**
- Fallback when Linear is unavailable (if Linear enabled)
- Single source of truth (if Linear disabled)

### docs/technical-specs/{ISSUE_ID}.md

**Purpose:** Single spec file per issue containing exploration + implementation plan.
**Updates:** Created by /sprint, updated during development.

---

## CLAUDE.md Template

```markdown
# Antigravity Project Guide

> Start here when working on this project.

---

## Quick Start

1. **Read the project state first:** `docs/PROJECT_STATE.md`
   - File structure, database schema, API endpoints
   - Tech stack, infrastructure, recent changes

2. **Check roadmap:** `docs/roadmap.md`
   - Current sprint tasks
   - Fallback when Linear unavailable

---

## Running the Project

### Backend

\`\`\`bash
cd /path/to/project/backend

# Add your run command

\`\`\`

### Frontend

\`\`\`bash
cd /path/to/project/frontend

# Add your run command

\`\`\`

### Tests

\`\`\`bash

# Backend tests

# Frontend tests

\`\`\`

---

## Working with Agents

Use the `/sprint` command for autonomous execution.

### Key Commands

- **`/sprint`**: Autonomous execution of Priority 1 tasks from Linear/Roadmap.
- **`/create-issue`**: Capture new bug/feature.
- **`/iterate`**: Continue iterating on current sprint.

---

## Linear Integration

**Note:** Set `linear_enabled: false` if this project doesn't use Linear. All task tracking will use roadmap.md only.

linear_enabled: true

| Setting         | Value                            |
| --------------- | -------------------------------- |
| Issue Prefix    | `XXX`                            |
| Team            | TeamName                         |
| Team ID         | `<team-uuid>`                    |
| Technical Specs | `docs/technical-specs/XXX-##.md` |

### Status UUIDs (for `mcp_linear_update_issue`)

| Status      | UUID     |
| ----------- | -------- |
| Backlog     | `<uuid>` |
| Todo        | `<uuid>` |
| In Progress | `<uuid>` |
| In Review   | `<uuid>` |
| Done        | `<uuid>` |
| Canceled    | `<uuid>` |

| Label     | Purpose                                                                |
| --------- | ---------------------------------------------------------------------- |
| agent     | Applied to ALL issues created by Claude (not human-created)            |
| technical | Applied to backend/infra/tech-debt issues Claude inferred or initiated |

> **Note:** Create these labels in Linear if they don't exist. Agent will apply them automatically.

All task tracking happens in Linear. Agents post updates as comments on issues.
Use `docs/roadmap.md` as fallback when Linear is unavailable.

---

## Deployment

| Environment | Branch    | URL                   |
| ----------- | --------- | --------------------- |
| Staging     | `develop` | [your-staging-url]    |
| Production  | `main`    | [your-production-url] |

**Git workflow:** `feature/*` → `develop` (staging) → `main` (production)

---

## Before You Commit

Checklist:

- [ ] Tests pass
- [ ] No unintended file changes
- [ ] Commit message describes the "why"
```

---

## PROJECT_STATE.md Template

```markdown
# Project State

> **Purpose:** Current state of the [ProjectName] codebase
> **Updated by:** Developer Agent after each completed task
> **Last updated:** [date]

---

## File Structure

\`\`\`
project/
├── backend/
│ └── ...
├── frontend/
│ └── ...
└── docs/
├── PROJECT_STATE.md
├── roadmap.md
└── technical-specs/
\`\`\`

---

## Database Schema

[Database type] at `path/to/database`

\`\`\`sql
-- Key tables
\`\`\`

---

## API Endpoints

| Method | Endpoint | Description |
| ------ | -------- | ----------- |
| GET    | /api/... | ...         |

---

## Tech Stack

| Component | Technology |
| --------- | ---------- |
| Backend   | ...        |
| Frontend  | ...        |
| Database  | ...        |

---

## Key Technical Decisions

These are established patterns - don't change without discussion:

1. **[Decision]** - [rationale]

---

## Infrastructure

### Git Branch Structure

| Branch    | Purpose    | Auto-deploys to |
| --------- | ---------- | --------------- |
| `main`    | Production | Production      |
| `develop` | Staging    | Staging         |

### Deployment

| Environment | Branch    | URL              |
| ----------- | --------- | ---------------- |
| Staging     | `develop` | [staging-url]    |
| Production  | `main`    | [production-url] |

Platform: [Vercel/Railway/etc]

### Environment Variables

- `VAR_NAME` - description

---

## Recent Changes

> Add new entries at the top. Keep last 10 entries.

| Date       | Change        | Commit |
| ---------- | ------------- | ------ |
| YYYY-MM-DD | Initial setup | abc123 |
```

---

## roadmap.md Template

```markdown
# Roadmap

> **Purpose:** Index of all tasks and specs. Mirrors Linear.
> **Updated by:** EM Agent when tasks change status
> **Fallback:** Use this when Linear is unavailable

---

## Sync Status

| Status     | Last Synced | Notes |
| ---------- | ----------- | ----- |
| ✅ In sync | [date]      | —     |

> Update this when Linear is unavailable. Clear pending items after syncing.

### Pending Updates (when Linear unavailable)

<!-- Add items here when Linear MCP fails, remove after syncing -->

---

## Active Sprint

| Priority | Issue  | Title   | Status         | Spec                              |
| -------- | ------ | ------- | -------------- | --------------------------------- |
| 1        | XXX-## | [Title] | 🟨 In Progress | [spec](technical-specs/XXX-##.md) |
| 2        | XXX-## | [Title] | 🟥 To Do       | [spec](technical-specs/XXX-##.md) |

**Status:** 🟥 To Do | 🟨 In Progress | 🟩 Done | ⏸️ Blocked

---

## Recently Completed Sprints

### Sprint 001: [Name] (Completed YYYY-MM-DD)

[Sprint file](sprints/sprint-001-[name].done.md)

| Priority | Issue  | Title   | Status  | Spec                              |
| -------- | ------ | ------- | ------- | --------------------------------- |
| 1        | XXX-## | [Title] | 🟩 Done | [spec](technical-specs/XXX-##.md) |

---

## Backlog

Sort by priority (High → Medium → Low), then by issue number.

| Priority | Issue  | Title   | Added      | Notes           |
| -------- | ------ | ------- | ---------- | --------------- |
| High     | XXX-## | [Title] | YYYY-MM-DD | [brief context] |
| Medium   | XXX-## | [Title] | YYYY-MM-DD | [brief context] |
| Low      | XXX-## | [Title] | YYYY-MM-DD | [brief context] |

---

## Notes

- **Linear is source of truth** - this file mirrors it
- **Sync timing:** After Linear changes, at sprint start, at sprint end
- If Linear unavailable, this becomes temporary source of truth
- When Linear is added/restored, EM reconciles (shows diff, User approves)
- Specs live in `technical-specs/{ISSUE_ID}.md`
```

---

## Setup Checklist

When setting up a new project:

1. [ ] Create `CLAUDE.md` in project root using template above
2. [ ] Create `docs/` directory
3. [ ] Create `docs/PROJECT_STATE.md` using template above
4. [ ] Create `docs/roadmap.md` using template above
5. [ ] Create `docs/technical-specs/` directory
6. [ ] Create `docs/sprints/` directory (for sprint iteration tracking)
7. [ ] **Decide if project uses Linear:**
   - Set `linear_enabled: true` in CLAUDE.md (requires steps 8-11)
   - OR set `linear_enabled: false` (skip steps 8-11, use roadmap.md only)
8. [ ] Set up Linear team and issue prefix (if enabled)
9. [ ] Add project to Linear with correct team assignment (if enabled)
10. [ ] Update CLAUDE.md with actual Linear team and prefix (if enabled)
11. [ ] **Get Linear status UUIDs** and add to CLAUDE.md (if enabled)
12. [ ] **Set up Git branches:** Create `develop` branch, configure auto-deploy to staging
13. [ ] **Add deployment URLs** to CLAUDE.md (staging + production)
