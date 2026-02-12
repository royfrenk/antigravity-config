---
description: Continue iterating on current sprint. Use after /sprint when user reports bugs or issues.
---

# Sprint Iteration

Continue working on the current sprint after user testing reveals bugs or issues.

> **Purpose:** Maintain protocol and context during the bug-fix iteration loop.

## Workflow

1. **Load sprint context:**
   - **Search for active sprint file:** `find docs/sprints/ -name "*.active.md"`
   - **If none found**: Error, exit.
   - **If found**: Read sprint file + linked specs.

2. **Receive bug batch from user:**
   - User provides list of issues (or continue from last batch).
   - Identify if it's a bug (failed AC) or new AC.

3. **Update sprint file:**
   - Add new batch to **Iteration Log**.
   - Mark items as `[ ]`.

4. **Fix Loop**:
   - For each issue:
     - **Verification**: Run build, lint, tests.
     - **Review**: Review code changes against `rules/coding-style.md`.
     - **Commit**: Commit with message format.
     - **User Logic**: If user asks for specific checks, perform them.

5. **Completion**:
   - Mark as `[x]` in sprint file.
   - Run full tests.
   - Ask user to verify fix.

6. **Wrap-up**:
   - If user says "Done" or "Deployed", rename `.active.md` to `.done.md`.
   - Update `docs/roadmap.md`.

## Output Format

After each batch:

```markdown
## Iteration Update — [date]

### Fixed This Batch

- [x] [description] ([commit])

### Still Open

- [ ] [description]

### What You Should Do Next

1. Test on staging: [URL]
2. Report new findings.
```

## Rules

- **Track everything**: Every bug goes in the iteration log.
- **New ACs**: Add to spec file if new requirement.
- **Context**: Read sprint file often.
