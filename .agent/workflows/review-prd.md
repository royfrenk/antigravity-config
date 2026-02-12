---
description: Review a Product Requirements Document thoroughly, extract stories, and create issues.
---

# Review PRD

Multi-perspective PRD review with automatic story extraction and issue creation.

## Quick Start

```bash
/review-prd [paste PRD content]
/review-prd [path to PRD file]
```

---

## How It Works

### Phase 1: Receive PRD

I'll read the PRD from:
- Pasted content in your message
- File path you provide
- Attached document

### Phase 2: Multi-Perspective Review

I'll review from three perspectives:

**1. Technical Review**
- Scope and feasibility
- Architecture implications
- Edge cases and error handling
- Testing requirements
- Performance considerations

**2. Design Review** (using `.agent/skills/design-core.md` if available)
- User experience flow
- UI states (loading, error, empty, success)
- Platform considerations (mobile, desktop, tablet)
- Accessibility requirements

**3. Legal/Compliance Review** (using `.agent/guides/legal.md` if available)
- Privacy implications
- Data handling requirements
- Compliance risks (GDPR, CCPA, etc.)
- Terms of service updates needed

### Phase 3: Present Questions

I'll compile questions from all perspectives:

```markdown
## PRD Review: [Feature Name]

### Technical Questions
1. How should we handle offline mode for this feature?
2. What's the expected data volume? (impacts database design)
3. Should this integrate with existing auth system or be separate?

### Design Questions
1. What happens when the user has no data yet? (empty state)
2. How should errors be displayed? (toast, modal, inline?)
3. Mobile-first or desktop-first approach?

### Legal Considerations
1. Does this feature collect new user data? (privacy policy update needed)
2. Are we storing sensitive information? (encryption requirements)
3. Do we need user consent for this feature?
```

### Phase 4: Dialogue

I'll wait for your answers and iterate until requirements are clear.

**I'll keep this conversational** - not a rigid Q&A, but a collaborative discussion.

### Phase 5: Extract Stories

Once requirements are clear, I'll propose stories in table format:

```markdown
## Proposed Stories

| ID | Title | Type | Priority | Effort | Notes |
|----|-------|------|----------|--------|-------|
| 1 | User authentication flow | Feature | High | Large | Requires OAuth setup |
| 2 | Dashboard UI | Feature | High | Medium | Use existing design system |
| 3 | Data export to CSV | Feature | Medium | Small | Simple implementation |
| 4 | Error handling | Technical | High | Small | Critical for UX |
```

I'll wait for your approval before creating issues.

### Phase 6: Create Issues

After approval, I'll:

1. **Check `CLAUDE.md`** for Linear settings
2. **Create issues** in Linear (if enabled) or roadmap.md
3. **Add to Backlog** section of `docs/roadmap.md`
4. **Create spec files** if needed for complex stories

---

## Smart Features

### Automatic Skill Loading
I'll load design and legal skills if they exist in `.agent/skills/` or `.agent/guides/`.

### Context-Aware Questions
Questions are tailored to the specific feature being reviewed.

### Story Sizing
I'll suggest effort estimates based on complexity analysis.

### Dependency Detection
I'll identify which stories depend on others and note it.

---

## Output Format for Roadmap

```markdown
## Backlog

| Issue    | Title                  | Added      | Notes    |
| -------- | ---------------------- | ---------- | -------- |
| EXP-36   | User authentication    | 2026-02-12 | From PRD |
| EXP-37   | Dashboard UI           | 2026-02-12 | From PRD |
| EXP-38   | Data export to CSV     | 2026-02-12 | From PRD |
```

---

## Examples

**Simple feature PRD:**
```
You: /review-prd "Add export to CSV feature for expenses"
Me: [reviews, asks 3-4 questions]
You: [answers]
Me: [proposes 2 stories, creates issues]
    ✅ Created EXP-36, EXP-37
```

**Complex feature PRD:**
```
You: /review-prd [pastes detailed PRD]
Me: [multi-perspective review, 10+ questions]
You: [answers over multiple messages]
Me: [proposes 8 stories with dependencies]
    ✅ Created 8 issues in Backlog
```

**PRD with legal implications:**
```
You: /review-prd "Add user data export feature"
Me: ⚠️ Legal consideration: This requires GDPR compliance
    [asks about data handling, consent, encryption]
You: [clarifies]
Me: ✅ Created issues with compliance notes
```

---

## Rules

- **Don't modify the PRD**: Treat it as an input artifact
- **Don't start implementation**: Just create the tickets
- **Multi-perspective**: Always review from technical, design, and legal angles
- **Collaborative**: Iterate until requirements are crystal clear
- **Actionable**: Stories should be implementable without ambiguity
