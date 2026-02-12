---
description: Bidirectional sync between Linear and roadmap.md. Run after manual Linear changes or after sprint with failed syncs.
---

# Sync Roadmap

Bidirectional sync between Linear and roadmap.md with automatic conflict resolution.

## Quick Start

```bash
/sync-roadmap          # Bidirectional sync: Linear ↔ roadmap.md
```

---

## Prerequisites

**This command requires Linear integration to be enabled.**

I'll automatically check `CLAUDE.md` for `linear_enabled` field:
- If `false`: Exit with message "Linear integration not enabled for this project"
- If `true`: Proceed with bidirectional sync

**For projects without Linear:** Use roadmap.md as single source of truth, no sync needed.

---

## When to Run

- After making changes in Linear UI manually
- After sprint completes with "Pending Manual Sync" entries
- At sprint start (invoked by `/sprint`)
- At sprint end (invoked by `/sprint`)
- When Linear was down during sprint
- On demand: when you notice Linear ↔ roadmap.md are out of sync

---

## How It Works

### Phase 1: Read Configuration

I'll read `CLAUDE.md` to validate Linear configuration:
- Check `linear_enabled: true/false`
- If `false`: EXIT
- If `true`: Extract team ID and issue prefix

### Phase 2: Pull from Linear (Linear → roadmap.md)

I'll query Linear for all issues in the team using `mcp__linear__list_issues`.

**Reconciliation rules:**

| Status in Linear | Source of Truth | Action                                      |
| ---------------- | --------------- | ------------------------------------------- |
| Backlog          | Linear          | Update roadmap.md to match Linear           |
| Todo             | Linear          | Update roadmap.md to match Linear           |
| Canceled         | Linear          | Update roadmap.md (remove or mark canceled) |
| In Progress      | roadmap.md      | Flag discrepancy (handled in Phase 4)       |
| In Review        | roadmap.md      | Flag discrepancy (handled in Phase 4)       |
| Done             | roadmap.md      | Flag if not deployed to production          |

**Track issues:**
- Issues in Linear but not in roadmap.md → Add to roadmap.md
- Issues in roadmap.md but not in Linear → Flag for your decision

### Phase 3: Push to Linear (roadmap.md → Linear)

I'll check for pending syncs:
1. Read active sprint file "Pending Manual Sync" sections
2. Check `docs/roadmap.md` for In Progress/In Review/Done statuses
3. Check `docs/PROJECT_STATE.md` for deployment state

**Determine correct Linear status:**
- **In staging only** → "In Review"
- **In production** → "Done"
- **Work in progress** → "In Progress"

**Push updates with soft retry:**
```
For each issue needing sync:

Issue: EXP-31
Current Linear status: [query via mcp__linear__get_issue]
Expected status: Done (from roadmap.md/deployment state)

If statuses differ:
  Attempt 1: mcp_linear_update_issue(issueId, status: "<UUID>")
  If fails: Wait 2s
  Attempt 2: mcp_linear_update_issue(issueId, status: "<UUID>")
  If still fails: Report to user
```

### Phase 4: Handle Conflicts

**Conflict scenarios:**

| Linear Status | roadmap.md Status | Resolution                                                                     |
| ------------- | ----------------- | ------------------------------------------------------------------------------ |
| Backlog       | In Progress       | Linear wins (user changed priority) → Update roadmap.md                        |
| Todo          | In Review         | roadmap.md wins (agent work state) → Update Linear                             |
| In Progress   | Done              | Check deployment state → If deployed: roadmap.md wins, else: flag for decision |
| Done          | In Review         | Flag for user: "Linear shows Done but not in production?"                      |

I'll present conflicts for your decision:

```markdown
## Sync Conflicts Detected

| Issue  | Linear  | roadmap.md  | Recommendation                    |
| ------ | ------- | ----------- | --------------------------------- |
| EXP-42 | Backlog | In Progress | Keep Linear (user demoted)        |
| EXP-43 | Done    | In Review   | Keep roadmap.md (not deployed yet) |

Apply recommendations? (yes/no/review each)
```

### Phase 5: Present Changes

Before applying anything, I'll show you all changes:

```markdown
## Roadmap Sync — Bidirectional

### Phase 1: Pull from Linear (Linear → roadmap.md)

**Synced from Linear (automatic):**
- EXP-42: Backlog → Todo (user changed in Linear)
- EXP-45: New issue in Linear → Adding to roadmap.md

**New issues to add:**
- EXP-46: [Title] — Add to roadmap.md Backlog?

**Missing in Linear:**
- EXP-41: [Title] in roadmap.md but not in Linear — Was this deleted?

### Phase 2: Push to Linear (roadmap.md → Linear)

**Pushing agent status updates:**
- EXP-40: In Review (staged) → Updating Linear
- EXP-39: Done (deployed) → Updating Linear

**Pending manual syncs resolved:**
- 2 issues from sprint-003 "Pending Manual Sync" updated

### Phase 3: Conflicts (need decision)

- EXP-43: Linear shows "Done", roadmap.md shows "In Review"
  - Not deployed to production yet
  - **Recommendation:** Keep roadmap.md, update Linear to "In Review"
  - Approve? (yes/no)

---

**Apply all changes?** (yes/no/review)
```

### Phase 6: Execute After Confirmation

After your approval:
1. Apply approved changes to `docs/roadmap.md`
2. Push approved changes to Linear (with soft retry)
3. Update sprint file (if exists): Clear "Pending Manual Sync" entries
4. Update Sync Status in roadmap.md:
   ```
   | ✅ In sync | 2026-02-12 | Pulled 3 from Linear, Pushed 2 to Linear |
   ```

---

## Smart Features

### Parallel State Checking
I'll query Linear, roadmap.md, and PROJECT_STATE.md simultaneously.

### Automatic Conflict Detection
I'll identify conflicts and suggest resolutions based on deployment state.

### Soft Retry Logic
2 attempts per push operation with 2-second delay.

### Non-Blocking
Failed syncs don't prevent workflow continuation.

---

## Error Handling

**If Linear is unavailable:**

```
⚠️ Linear is unavailable (all sync attempts failed).

Completed:
- ✅ Phase 1: Pull from Linear — SKIPPED (Linear down)
- ✅ Phase 2: Push to Linear — FAILED 2 issues
- ✅ Phase 3: Conflicts — N/A

Options:
1. Wait and try /sync-roadmap again later
2. Update issues manually in Linear UI
3. Proceed without syncing (roadmap.md is source of truth)

Recommendation: Update manually in Linear UI using roadmap.md as reference.
```

**Partial sync success:**
- Report which operations succeeded/failed
- Update roadmap.md with successful pulls
- Track failed pushes in sprint file "Pending Manual Sync"
- Don't block workflow on failures

---

## Examples

**Full bidirectional sync:**
```
You: /sync-roadmap
Me: [pulls 3 from Linear, pushes 2 to Linear, resolves 1 conflict]
    ✅ Synced: 3 pulled, 2 pushed, 1 conflict resolved
```

**Conflict resolution:**
```
You: /sync-roadmap
Me: Found conflict: EXP-43 (Linear: Done, roadmap: In Review)
    Recommendation: Keep roadmap.md (not deployed yet)
    Approve?
You: Yes
Me: ✅ Conflict resolved, Linear updated
```

**Linear unavailable:**
```
You: /sync-roadmap
Me: ⚠️ Linear unavailable
    roadmap.md is source of truth
    Update Linear manually when it's back up
```

---

## Rules

- **Bidirectional**: Handles both Linear → roadmap.md AND roadmap.md → Linear
- **Non-blocking**: Failed syncs don't prevent workflow continuation
- **Soft retry**: 2 attempts per push operation (with 2s delay)
- **User confirmation**: Always present changes before applying
- **Source of truth split**:
  - Linear: Backlog, Todo, Canceled (user-managed)
  - roadmap.md: In Progress, In Review, Done (agent-managed)
- **Deployment check**: Flag "Done" issues that aren't actually in production
- **Parallel execution**: Query all sources simultaneously for faster sync
