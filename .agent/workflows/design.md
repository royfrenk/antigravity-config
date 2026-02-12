---
description: Invoke design skills with automatic context detection for marketing, applications, or dashboards.
---

# Design Command

Invoke design workflows with automatic context detection and skill loading.

## Quick Start

```bash
/design landing page          # Auto-detects marketing context
/design admin dashboard        # Auto-detects dashboard context
/design user settings          # Auto-detects application context
/design                        # I'll ask for context
```

---

## How It Works

### Phase 1: Context Detection

I'll analyze your request to detect the design context:

**Marketing** (landing pages, hero sections, pricing):
- Keywords: landing, hero, pricing, marketing, homepage
- Focus: Conversion, visual impact, storytelling

**Applications** (admin panels, CRUD, settings):
- Keywords: admin, panel, settings, form, CRUD
- Focus: Usability, efficiency, data density

**Dashboards** (analytics, charts, metrics):
- Keywords: dashboard, analytics, charts, metrics, stats
- Focus: Data visualization, insights, clarity

### Phase 2: Load Skills

I'll automatically load the relevant design skills:

**Always loaded:**
- `.agent/skills/design-core.md` - Design system fundamentals

**Context-specific:**
- `.agent/skills/design-marketing.md` - For marketing contexts
- `.agent/skills/design-applications.md` - For application contexts  
- `.agent/skills/design-dashboards.md` - For dashboard contexts

If skills don't exist, I'll work with design best practices.

### Phase 3: Execute Design Work

Following the loaded skill instructions, I'll:

1. **Plan Layout**
   - Define grid system and spacing
   - Plan component hierarchy
   - Consider responsive breakpoints

2. **Design Components**
   - Create reusable components
   - Define states (default, hover, active, disabled)
   - Ensure accessibility

3. **Review Content**
   - Check tone and voice
   - Ensure clarity and scannability
   - Validate CTAs and messaging

4. **Implement**
   - Generate HTML/CSS (or framework components)
   - Use design tokens for consistency
   - Add micro-animations where appropriate

---

## Design Process

### 1. Understand Requirements
- What's the goal of this design?
- Who's the target user?
- What actions should they take?

### 2. Create Design Plan
```markdown
## Design Plan: [Component Name]

**Context:** Marketing landing page
**Goal:** Convert visitors to sign-ups
**Target:** Technical PMs and developers

**Layout:**
- Hero section with value prop
- Feature showcase (3-column grid)
- Pricing table
- CTA section

**Components:**
- Hero with gradient background
- Feature cards with icons
- Pricing cards with hover effects
- CTA button (primary action)

**Tokens:**
- Colors: Primary (#6366f1), Secondary (#8b5cf6)
- Typography: Inter (headings), System UI (body)
- Spacing: 4px base unit
```

### 3. Implement Design
- Use design tokens from core skill
- Follow component patterns from context skill
- Ensure responsive behavior

### 4. Review Against Standards
- Accessibility (WCAG AA)
- Performance (optimized assets)
- Consistency (design system adherence)

---

## Smart Features

### Automatic Skill Loading
I'll detect context and load the right skills without you specifying them.

### Design Token System
All designs use a consistent token system for colors, typography, and spacing.

### Component Library
Reusable components that follow best practices.

### Responsive by Default
All designs work across mobile, tablet, and desktop.

---

## Examples

**Marketing landing page:**
```
You: /design landing page for expense tracking app
Me: [loads marketing skill, creates hero + features + pricing]
    ✨ Created landing page with conversion-focused design
```

**Admin dashboard:**
```
You: /design analytics dashboard
Me: [loads dashboard skill, creates charts + metrics + filters]
    ✨ Created dashboard with data visualization
```

**Application UI:**
```
You: /design user settings page
Me: [loads application skill, creates form + navigation + actions]
    ✨ Created settings page with clear UX
```

---

## Skill Locations

If you want to customize design skills:

- **Core**: `.agent/skills/design-core.md`
- **Marketing**: `.agent/skills/design-marketing.md`
- **Applications**: `.agent/skills/design-applications.md`
- **Dashboards**: `.agent/skills/design-dashboards.md`

---

## Rules

- **Context-aware**: Auto-detect design context from request
- **Skill-based**: Load and follow relevant skill instructions
- **Token-driven**: Use design tokens for consistency
- **Responsive**: All designs work across devices
- **Accessible**: Follow WCAG AA guidelines
- **Premium**: Avoid basic MVP aesthetics - make it beautiful
