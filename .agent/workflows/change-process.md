---
description: Update global Antigravity configurations and sync to config repo.
---

# Change Process

Update workflows systematically with automatic discovery, parallel analysis, and smart syncing.

## How to Use

```bash
/change-process [what you want to change]
```

If you don't provide details, I'll ask you what you want to change.

---

## Step 1: Understand the Change

I'll start by asking you a few quick questions about the change you want to make. This helps me understand:
- What needs to change
- Why you're making the change
- What scope it affects (global vs project-specific)
- Any edge cases or dependencies

**I'll keep this conversational** - if you've already explained something, I won't ask again.

---

## Step 2: Auto-Discovery & Analysis

I'll automatically:

1. **Discover all workflows** using `find_by_name` to scan `.agent/workflows/`
2. **Read relevant files in parallel** using multiple `view_file` calls
3. **Search for references** using `grep_search` to find cross-file dependencies
4. **Check project files** like `CLAUDE.md`, `docs/roadmap.md`, etc.

Then I'll present an **impact analysis table**:

```
| File | Impact | What Changes | Risk |
|------|--------|--------------|------|
| sprint.md | High | Update workflow step 3 | Medium |
| create-issue.md | None | — | — |
| context.md | Low | Update terminology | Low |
```

---

## Step 3: Challenge & Validate

Before proposing changes, I'll check for:
- **Gaps**: Missing edge cases or incomplete logic
- **Contradictions**: Conflicts with other workflows
- **Complexity**: Is this adding unnecessary noise?
- **Scope creep**: Should this be split into multiple changes?

If I find issues, I'll point them out. If not, I'll move forward.

---

## Step 4: Propose Specific Changes

I'll show you exactly what I plan to change:

```markdown
## Proposed Changes

### 1. .agent/workflows/sprint.md (Lines 45-52)

**Current:**
...existing code...

**New:**
...updated code...

**Reason:** [explanation]

---

### 2. .agent/workflows/context.md (Lines 12-15)

**Current:**
...existing code...

**New:**
...updated code...

**Reason:** [explanation]
```

You can:
- Approve all changes
- Request modifications
- Approve some, reject others

---

## Step 5: Execute & Sync

After your approval, I'll:

1. **Make the changes** using `multi_replace_file_content` (for multiple edits) or `replace_file_content` (for single edits)
2. **Verify consistency** by checking cross-references
3. **Show git diff** to confirm what's changing
4. **Sync to repo**:
   ```bash
   # Copy to antigravity-config repo
   cp .agent/workflows/*.md /Users/royfrenkiel/Documents/repos/antigravity-config/.agent/workflows/
   
   # Show diff and commit
   cd /Users/royfrenkiel/Documents/repos/antigravity-config
   git diff
   git add -A && git commit -m "process: [description]" && git push origin main
   ```

---

## Smart Features

### Automatic Workflow Discovery
No hardcoded file lists. I'll discover workflows dynamically:
```bash
find .agent/workflows/ -name "*.md" -type f
```

### Parallel File Reading
I'll read multiple files at once for faster analysis.

### Reference Checking
I'll use `grep_search` to find all references to changed concepts across all workflows.

### Change Tracking
Each sync updates `/Users/royfrenkiel/Documents/repos/antigravity-config/CHANGELOG.md` with:
- Date and description
- Files changed
- Reason for change

### Validation
Before syncing, I'll check:
- YAML frontmatter is valid
- Markdown syntax is correct
- File references exist
- No broken cross-references

---

## Examples

**Simple terminology change:**
```
You: "Change all references from 'Claude Code' to 'Antigravity'"
Me: [discovers 3 workflows affected, shows changes, syncs after approval]
```

**Workflow addition:**
```
You: "Add a new step to /sprint that checks for breaking changes"
Me: [analyzes sprint.md, proposes insertion point, shows before/after, syncs]
```

**Multi-workflow update:**
```
You: "Update the checkpoint format to include git commit hash"
Me: [finds /checkpoint, /sprint, /iterate all use checkpoints, updates all three]
```

---

## Rules

- **Don't assume** - Ask if unclear
- **Don't add noise** - Every change should solve a real problem
- **Challenge gently** - Point out issues early
- **Be thorough** - Check all affected files
- **Be specific** - Show exact before/after changes
- **Validate** - Test that references still work

---

**Ready to start?** Tell me what you want to change, or just say `/change-process` and I'll ask.
