---
description: Review a Product Requirements Document thoroughly, extract stories, and create issues.
---

# Review PRD

Review a Product Requirements Document thoroughly, extract stories, and create Linear issues (if enabled) or update roadmap.

## Overview

1. **Receive PRD**: Read the file/content.
2. **Review**: Multi-perspective review (Technical, Design, Legal).
3. **Q&A**: Ask clarifying questions.
4. **Extract Stories**: Propose actionable tasks.
5. **Create Issues**: add to `docs/roadmap.md` (and Linear if available).

## Workflow

1. **Phase A: Receive PRD**
   - Read the user provided content or file.

2. **Phase B: Review**
   - **Technical**: Scope, Architecture, Edge Cases, Testing.
   - **Design**: User Experience, States, Platform. (Reference `.agent/skills/design-core.md` if needed).
   - **Legal**: Privacy, Data, Risk. (Reference `.agent/guides/legal.md` if available).

   **Output**:

   ```markdown
   ## PRD Review: [Name]

   ### Technical Questions

   ...

   ### Design Questions

   ...

   ### Legal Considerations

   ...
   ```

3. **Phase C: Dialogue**
   - Wait for user answers.
   - Iterate until requirements are clear.

4. **Phase E: Extract Stories**
   - Propose stories in table format.
   - Wait for user approval.

5. **Phase G: Create Issues**
   - Check `CLAUDE.md` for Linear settings.
   - **If Linear enabled**: Use `mcp_linear_create_issue` (or equivalent) for each approved story.
   - **Always**: Add issues to `docs/roadmap.md` Backlog section.

## Output Format for Roadmap

```markdown
## Backlog

| Issue    | Title   | Added  | Notes    |
| -------- | ------- | ------ | -------- |
| TICKET-1 | [Title] | [Date] | From PRD |
```

## Rules

- **Do not modify the PRD**: Treat it as an input artifact.
- **Do not start implementation**: Just create the tickets.
