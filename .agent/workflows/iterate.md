---
description: Continue iterating on current sprint. Use after /sprint when user reports bugs or issues.
---

# Sprint Iteration

Continue working on the current sprint after testing reveals bugs or issues. Fast iteration loop with automatic context loading.

## Quick Start

```bash
/iterate                          # Continue current sprint
/iterate "bug description"        # Add specific bug
```

---

## How It Works

### Phase 1: Load Sprint Context

I'll automatically:
1. **Find active sprint** using `find docs/sprints/ -name "*.active.md"`
2. **Read sprint file** and linked spec files in parallel
3. **Load last checkpoint** to understand current state
4. **Check deployment status** from `docs/PROJECT_STATE.md`

If no active sprint found, I'll exit with an error.

### Phase 2: Receive Bug Batch

You can provide bugs in several ways:
- List them directly: "Button doesn't work, form validation broken"
- Reference them: "Check the issues I mentioned earlier"
- Continue from last batch: Just say "/iterate"

I'll categorize each as:
- **Failed AC**: Acceptance criteria that didn't pass
- **New AC**: New requirement discovered during testing
- **Bug**: Unexpected behavior

### Phase 3: Update Sprint File

I'll add the batch to the **Iteration Log**:

```markdown
## Iteration Log

### Batch 3 — 2026-02-12 10:30
- [ ] Button click doesn't trigger save
- [ ] Form validation allows empty email
- [ ] Mobile layout breaks on iPhone
```

### Phase 4: Fix Loop

For each issue:

1. **Analyze**: Use `grep_search` to find relevant code
2. **Fix**: Make targeted changes
3. **Verify**: Run `npm run lint && npm test`
4. **Review**: Check against coding standards
5. **Mark complete**: Update iteration log with `[x]`

I'll show you progress after each fix:
```
✅ Fixed: Button click now triggers save
   - Updated: public/js/dashboard.js (line 45)
   - Tests: All passing
   
Working on: Form validation...
```

### Phase 5: Batch Completion

After fixing all issues in the batch:
- Run full test suite
- Ask you to verify fixes
- Update spec file if new ACs were added

### Phase 6: Wrap-up

When you say "Done" or "All fixed":
- Rename sprint file: `.active.md` → `.done.md`
- Update `docs/roadmap.md`
- Sync to Linear (if enabled)

---

## Smart Features

### Automatic Context Loading
No need to explain what sprint you're working on - I'll find and load it automatically.

### Parallel Analysis
I'll use `grep_search` to find all occurrences of the bug pattern across the codebase.

### Incremental Testing
Tests run after each fix, not just at the end.

### Checkpoint Tracking
Each batch is saved to the sprint file, so you can pause and resume anytime.

---

## Output Format

After each batch:

```markdown
## Iteration Update — 2026-02-12 10:45

### Fixed This Batch (3/3)
- [x] Button click triggers save (commit: abc123)
- [x] Form validation checks email (commit: def456)
- [x] Mobile layout responsive (commit: ghi789)

### Tests
✅ All tests passing (42/42)
✅ Lint clean
✅ Build successful

### What You Should Do Next
1. Test on staging: https://staging.example.com
2. Report any new findings or say "Done"
```

---

## Examples

**Simple bug fix:**
```
You: /iterate "Save button doesn't work"
Me: [finds issue in dashboard.js, fixes it, runs tests]
    ✅ Fixed - ready for you to test
```

**Multiple bugs:**
```
You: /iterate "3 issues: save button, validation, mobile layout"
Me: [fixes all three, shows progress for each]
    ✅ All fixed - please verify on staging
```

**New acceptance criteria:**
```
You: /iterate "Actually, we need email validation too"
Me: [adds to spec as new AC, implements it]
    ✅ Added to spec and implemented
```

---

## Rules

- **Track everything**: Every bug goes in the iteration log
- **New ACs**: Add to spec file if it's a new requirement
- **Context-aware**: Read sprint file frequently to stay in sync
- **Non-blocking**: If a fix is complex, I'll flag it and move to the next
- **Incremental**: Test after each fix, not just at the end
