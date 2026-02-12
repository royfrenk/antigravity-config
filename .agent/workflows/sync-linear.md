---
description: Manually reconcile Linear with local state when syncs failed during sprint.
---

# Sync Linear

Recover from Linear sync failures with automatic state reconciliation.

## Quick Start

```bash
/sync-linear          # Sync all pending issues from active sprint
```

---

## When to Use

- After sprint completes with "Pending Manual Sync" entries
- When Linear was down during sprint
- Before closing sprint file (rename to .done.md)
- On demand: when Linear status doesn't match deployment state

---

## How It Works

### Phase 1: Find Active Sprint

I'll automatically:
```bash
find docs/sprints/ -name "*.active.md" 2>/dev/null
```

If no active sprint found, I'll exit with an error.

### Phase 2: Extract Pending Syncs

I'll read the sprint file and look for:
- "Pending Manual Sync" sections in check-ins
- "Linear sync: failed" notes
- Build list of issues needing status updates

### Phase 3: Determine Correct Status

For each issue, I'll check in parallel:
- `docs/PROJECT_STATE.md` - What's deployed
- `docs/roadmap.md` - Current status
- Spec files - Task completion state

**Status mapping:**
- **In staging only** → "In Review"
- **In production** → "Done"
- **Neither** → Based on spec file status

### Phase 4: Perform Sync with Soft Retry

For each issue:

```
Issue: EXP-31
Current Linear status: [query via mcp__linear__get_issue]
Expected status: Done (deployed to production)

If statuses differ:
  Attempt 1: mcp_linear_update_issue(issueId, status: "<UUID>")
  If fails: Wait 2s
  Attempt 2: mcp_linear_update_issue(issueId, status: "<UUID>")
  If still fails: Report to user
```

### Phase 5: Update Sprint File

I'll update the sprint file with results:

```markdown
## Check-in: Linear Sync Complete — 2026-02-12 10:45

**Synced Issues:** 3 successful
**Failed Issues:** 1 failed (requires manual intervention)

**Successful:**
- EXP-31: Updated to Done
- EXP-32: Updated to In Review
- EXP-33: Updated to In Progress

**Failed (still pending):**
- EXP-34: Rate limit exceeded - please update manually in Linear UI
```

### Phase 6: Report Results

```markdown
## Linear Sync Complete

**Successfully Synced:** 3 issues
**Failed:** 1 issue (see details below)

| Issue  | Status      | Result                         |
| ------ | ----------- | ------------------------------ |
| EXP-31 | Done        | ✅ Synced                      |
| EXP-32 | In Review   | ✅ Synced                      |
| EXP-33 | In Progress | ✅ Synced                      |
| EXP-34 | Done        | ❌ Failed - Rate limit exceeded |

**Next Steps:**
- For failed syncs: Update manually in Linear UI
- Sprint file updated with results
- Ready to close sprint (rename .active.md → .done.md)
```

---

## Smart Features

### Automatic State Detection
I'll check deployment state across multiple sources in parallel.

### Soft Retry Logic
2 attempts per issue with 2-second delay to handle transient failures.

### Non-Blocking
Failed syncs don't prevent sprint completion - you can always update Linear manually.

### Detailed Reporting
Clear breakdown of what succeeded and what needs manual intervention.

---

## Error Handling

**If Linear is still unavailable:**

```
⚠️ Linear is still unavailable (all sync attempts failed).

Options:
1. Wait and try /sync-linear again later
2. Update issues manually in Linear UI
3. Proceed without syncing (sprint file is source of truth)

Recommendation: Update manually in Linear UI using sprint file as reference.
```

**If some issues sync, some fail:**
- Update sprint file with partial results
- Report which issues need manual intervention
- Don't block sprint closure

---

## Examples

**All syncs successful:**
```
You: /sync-linear
Me: [finds 3 pending syncs, updates all successfully]
    ✅ All 3 issues synced to Linear
    Ready to close sprint
```

**Partial success:**
```
You: /sync-linear
Me: [syncs 2/3 issues, 1 fails due to rate limit]
    ✅ 2 synced, 1 failed
    Please update EXP-34 manually in Linear
```

**Linear still down:**
```
You: /sync-linear
Me: ⚠️ Linear unavailable
    Sprint file is source of truth
    Update Linear manually when it's back up
```

---

## Rules

- **Non-blocking**: Failed syncs don't prevent sprint completion
- **Soft retry**: 2 attempts per issue (with 2s delay)
- **Track results**: Update sprint file with sync outcomes
- **Manual fallback**: User can always update Linear UI directly
- **Parallel checks**: Query deployment state from multiple sources simultaneously
