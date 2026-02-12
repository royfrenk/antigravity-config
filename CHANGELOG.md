# Antigravity Config Changelog

All notable changes to Antigravity workflows will be documented in this file.

## [2026-02-12] - Added explicit review gates to sprint workflow

### Changed
- **sprint.md**: Made review gates explicit and actionable
  - Split "Review Gate" into Self-Review, Visual Review, and Code Review Gate
  - Added STOP points for visual and code reviews
  - Added Review Policy section with mandatory vs optional reviews
  - Added examples showing review flow and auto-approve options

### Added
- **Multi-Model Support**: Sprint workflow now includes explicit model switching recommendations
  - Design Review: Recommends switching to Gemini 2.0 Pro
  - Code Review: Recommends switching to Claude 3 Opus
  - Implementation: Recommends Claude 3.5 Sonnet
- **Review Policy**: Clear guidelines on when reviews are mandatory
  - Visual reviews: Required for all UI changes
  - Code reviews: Required after each task
  - Auto-approve option for simple changes
  - Examples of controlling review behavior

### Reason
User reported that sprint didn't pause for design/code reviews. Made review gates explicit with clear STOP points and approval requirements.

---

## [2026-02-12] - Complete workflow redesign for Antigravity

### Changed
- **ALL workflows**: Redesigned for Antigravity's direct execution model
  - Removed Claude Code subagent architecture references
  - Added automatic discovery using `find_by_name` and `grep_search`
  - Implemented parallel file reading for faster context loading
  - Added smart validation and error handling
  - More conversational, less prescriptive flows
  - Integrated git awareness and progress tracking

**Specific workflow improvements:**

- **change-process.md**: Auto-discovery, parallel analysis, smart syncing
- **sprint.md**: Automatic context loading, dependency detection, incremental verification
- **iterate.md**: Automatic sprint detection, parallel analysis, checkpoint tracking
- **context.md**: Parallel file reading, sprint gap detection, orphan detection
- **checkpoint.md**: Automatic file detection, git integration, progress calculation
- **create-issue.md**: Auto file discovery, smart categorization, fast capture
- **design.md**: Automatic context detection, skill loading
- **review-prd.md**: Multi-perspective review, automatic story extraction
- **sync-linear.md**: Automatic state detection, parallel checks
- **sync-roadmap.md**: Parallel state checking, automatic conflict detection
- **learning-opportunity.md**: Progressive disclosure, codebase-aware examples
- **new-project.md**: Interactive setup with automatic file creation

### Added
- Smart features across all workflows:
  - Automatic file discovery
  - Parallel execution where applicable
  - Git integration
  - Progress tracking
  - Non-blocking error handling
  - Context preservation

### Reason
Adapted all workflows from Claude Code's subagent architecture to Antigravity's direct execution model with better tooling support, parallel execution, and smarter automation.
