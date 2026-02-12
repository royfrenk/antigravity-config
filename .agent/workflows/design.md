---
description: Invoke design skills with automatic context detection for marketing, applications, or dashboards.
---

# Design Command

Invoke design skills with automatic context detection.

## How It Works

1. **Analyze Request**: Detect context (marketing, applications, dashboards).
   - Marketing: landing page, hero, pricing.
   - Applications: admin panel, CRUD, settings.
   - Dashboards: analytics, charts.

2. **Load Skills**:
   - Always load: `.agent/skills/design-core.md`.
   - Context specific: `.agent/skills/design-marketing.md`, `.agent/skills/design-applications.md`, or `.agent/skills/design-dashboards.md`.

3. **Execute Design Work**:
   - Follow instructions in the skill file.
   - Plan layout, components, states.
   - Review content tone/voice.

## Usage

- `/design` -> Ask for context.
- `/design [description]` -> Auto-detect context.

## Skill Locations

- `design-core.md`: `.agent/skills/design-core.md`
- `design-marketing.md`: `.agent/skills/design-marketing.md`
- `design-applications.md`: `.agent/skills/design-applications.md`
- `design-dashboards.md`: `.agent/skills/design-dashboards.md`

## Process

1. **Read Request**: Analyze user input.
2. **Read Skill Files**: Use `view_file` to read the relevant skill files.
3. **Plan**: Create a design plan based on tokens/components defined in core + specific skill.
4. **Implementation**: Implement design (HTML/CSS/JS or framework components).
5. **Review**: Check against design standards.
