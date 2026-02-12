# Change Process

**Note:** This command updates global `antigravity` configurations and syncs them to your config repository.

## Instructions

You are helping the User update the engineering process. Your goal is to ensure changes are consistent across all workflow files and don't add noise.

## 1. Understand the Change

**STOP AND ASK QUESTIONS - DO NOT PROCEED UNTIL USER RESPONDS**

Output ONLY the following question block and nothing else:

---

## Phase 1: Understanding Your Change

Please answer these questions so I can review the right files:

**1. What change do you want to make?**
(Describe the change in your own words)

**2. Why are you making this change?**

- What problem does this solve?
- What triggered this request?

**3. What's the scope of this change?**

- Does this affect all projects (global workflows)?
- Does this affect just one project?
- Something else?

**Based on your description, I may also need to know:**

[Include ONLY the applicable questions below - skip sections that don't apply]

**If the change affects workflow:**

- Does this change the order of operations?
- Does this change who is responsible for what?

**If adding a new file or concept:**

- Who creates it?
- Who updates it?
- When does it get created/updated?

**If removing something:**

- What replaces it?
- How do we handle existing references to it?

---

I'll wait for your answers before reviewing the affected files.

## 2. Review All Files

Read and analyze every file that might be affected:

### Global Workflows (.agent/workflows/)

**Commands:**

- `.agent/workflows/context.md` - Load project context
- `.agent/workflows/sprint.md` - Autonomous execution
- `.agent/workflows/create-issue.md` - Quick issue capture
- `.agent/workflows/new-project.md` - Project setup guide
- `.agent/workflows/learning-opportunity.md` - Teaching mode
- `.agent/workflows/change-process.md` - This command

### Current Project Files

Read the project's CLAUDE.md to find the docs location, then check:

- `CLAUDE.md` - Project entry point
- `docs/PROJECT_STATE.md` - Codebase state
- `docs/roadmap.md` - Task index
- `docs/technical-specs/` - Spec files

## 3. Impact Analysis

For each file, determine:

1. **Needs update?** (Yes/No/Maybe)
2. **What changes?** (Specific sections or references)
3. **Risk level:** (Low = wording change, Medium = workflow change, High = structural change)

Present findings as a table:

```
| File | Needs Update | What Changes | Risk |
|------|--------------|--------------|------|
| sprint.md | Yes | Add new step to workflow | Medium |
| create-issue.md | No | — | — |
| context.md | Yes | Update diagram | Low |
```

## 4. Challenge the Change

**STOP AND ASK CHALLENGE QUESTIONS - DO NOT PROCEED UNTIL USER RESPONDS**

After completing file review and impact analysis, check if you found any gaps, contradictions, complexity issues, or scope concerns.

**If you found concerns:**

Output ONLY the following question block and nothing else:

---

## Phase 4: Challenge Questions

Based on my file review and impact analysis, I need clarification on these points:

[Include ONLY the sections below that have actual concerns - skip empty sections]

**Gaps I Found:**

[List each gap with a numbered question:]

1. I noticed [X] isn't covered. How should that work?
2. What happens when [edge case]?
3. Who is responsible for [new thing]?

**Contradictions I Found:**

[List each contradiction with a numbered question:]

1. This conflicts with [existing rule in file.md]. Which takes priority?

**Complexity/Noise Concerns:**

[Ask if the change seems to add unnecessary complexity:]

1. Is this adding complexity that could be avoided?
2. Could this be solved with existing tools/processes instead?

**Scope Concerns:**

[Ask if scope seems unclear or too broad:]

1. You mentioned [X] - is that part of this change or should it be separate?

---

Please address these concerns before I propose the specific file changes.

**If you found NO concerns:**

Skip the question block entirely. Instead, output:

```
I reviewed all affected files and found no gaps, contradictions, or scope issues. Proceeding to propose specific changes.
```

## 5. Propose Changes

Once clarified, present the exact changes:

```
## Proposed Changes

### 1. .agent/workflows/context.md
**Section:** Workflow
**Change:** Add step 5: "Update roadmap.md"
**Reason:** [why]

[Continue for all affected files]
```

Ask: "Does this look right? Any adjustments before I make the changes?"

## 6. Execute and Sync

Only after the User confirms:

1. Make all changes to `.agent/workflows/` (the live config).
2. Verify consistency across files.
3. Summarize what was changed.
4. **Sync to Antigravity Config Repo:**

   ```bash
   # Ensure target directory exists
   mkdir -p /Users/royfrenkiel/Documents/repos/antigravity-config/.agent/workflows

   # Copy workflows to repo
   cp -R .agent/workflows/*.md /Users/royfrenkiel/Documents/repos/antigravity-config/.agent/workflows/

   # Commit and push
   cd /Users/royfrenkiel/Documents/repos/antigravity-config
   git add -A && git commit -m "process: $ARGUMENTS" && git push origin main
   ```

## Rules

- **Don't assume** - Ask if unclear
- **Don't add noise** - Every addition should solve a real problem
- **Challenge gently** - The User might have missed something, help them see it
- **Be thorough** - Read every file, don't skip
- **Be specific** - Vague changes lead to inconsistency

---

**Start by asking:** "What change do you want to make to the process?"
