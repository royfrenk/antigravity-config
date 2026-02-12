# Antigravity Config Changelog

All notable changes to Antigravity workflows will be documented in this file.

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
