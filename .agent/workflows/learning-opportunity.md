---
description: Pause development mode to teach a concept. Suitable for Technical PMs.
---

# Learning Opportunity

Pause development to teach a concept with three-level explanations and automatic documentation.

## Quick Start

```bash
/learning-opportunity "explain webhooks"
/learning-opportunity                      # I'll ask what you want to learn
```

---

## Target Audience

Technical PM with mid-level engineering knowledge who:
- Understands architecture and can read code
- Ships production apps with AI assistance
- Has solid fundamentals, wants to deepen understanding
- Not a senior engineer, but not a beginner either

---

## How It Works

### Phase 1: Understand the Topic

I'll ask what concept you want to learn about (if not provided).

### Phase 2: Three-Level Explanation

I'll explain the concept at three increasing complexity levels. You can stop at any level.

**Level 1: Core Concept** (5 minutes)
- What this is and why it exists
- The problem it solves
- When you'd reach for this pattern
- How it fits into the broader architecture

**Level 2: How It Works** (10 minutes)
- The mechanics underneath
- Key tradeoffs and why we chose this approach
- Edge cases and failure modes to watch for
- How to debug when things go wrong

**Level 3: Deep Dive** (15 minutes)
- Implementation details that affect production behavior
- Performance implications and scaling considerations
- Related patterns and when to use alternatives
- The "senior engineer" perspective on this

### Phase 3: Offer to Document

After teaching, I'll ask:

> "Would you like me to add this to your Technical Learnings document?"

If yes, I'll look for `Technical-Learnings.md` in your workspace (or ask where to create it).

### Phase 4: Add to Technical Learnings

I'll add an entry using this format:

```markdown
## N. Topic Title

`Category` · `Tags` · `project-name`
_2026-02-12_

[↑ Back to top](#table-of-contents)

### The Problem

What problem does this concept solve?

### Level 1: Core Concept

What is it and why does it exist?

### Level 2: How It Works

Mechanics, tradeoffs, gotchas.

### Level 3: Deep Dive

Implementation details, performance, advanced patterns.

---
```

And update the Table of Contents:

```markdown
| N | [Topic Title](#n-topic-title) | Category | project-name | 2026-02-12 |
```

---

## Teaching Approach

**Philosophy:** 80/20 rule - focus on concepts that compound. Don't oversimplify, but prioritize practical understanding over academic completeness.

**Tone:**
- Peer-to-peer, not teacher-to-student
- Technical but not jargon-heavy
- Concrete examples from the current codebase
- Acknowledge complexity honestly - "this is genuinely tricky because..."

**Examples:**
- Use real code from the project when possible
- Show before/after comparisons
- Demonstrate with actual use cases

---

## Categories

- `Antigravity` - Agent workflows and capabilities
- `Architecture` - System design patterns
- `Frontend` - UI/UX implementation
- `Backend` - Server-side concepts
- `DevOps` - Deployment and infrastructure
- `Security` - Security patterns and best practices

---

## Examples

**Simple concept:**
```
You: /learning-opportunity "explain async/await"
Me: [Level 1: What it is and why it exists]
    Want to go deeper?
You: Yes
Me: [Level 2: How it works under the hood]
    Want the deep dive?
You: No, that's enough
Me: Add to Technical Learnings?
You: Yes
Me: ✅ Added to Technical-Learnings.md
```

**Complex concept:**
```
You: /learning-opportunity "explain database indexing"
Me: [Level 1: Core concept with examples from your DB]
You: More
Me: [Level 2: B-tree mechanics, query planning]
You: More
Me: [Level 3: Index types, performance implications, when to avoid]
You: Perfect, save it
Me: ✅ Added to Technical-Learnings.md with all 3 levels
```

**Project-specific:**
```
You: /learning-opportunity "why did we use SQL.js instead of PostgreSQL?"
Me: [Explains with context from your Expensinator project]
    [Shows tradeoffs specific to your use case]
You: Save it
Me: ✅ Added to Technical-Learnings.md under Architecture
```

---

## Smart Features

### Codebase-Aware Examples
I'll use `grep_search` to find relevant examples from your actual codebase.

### Progressive Disclosure
You control how deep to go - stop at any level.

### Automatic Documentation
Learnings are saved to a searchable document for future reference.

### Context-Specific
Explanations are tailored to your project and tech stack.

---

## Rules

- **Respect time**: Keep each level concise and focused
- **Use real examples**: Pull from the current codebase when possible
- **Progressive**: Let user control depth
- **Honest**: Acknowledge when something is genuinely complex
- **Practical**: Focus on what matters for production apps
- **Peer-to-peer**: Collaborative tone, not lecturing
