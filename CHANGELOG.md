# Antigravity Config Changelog

All notable changes to Antigravity workflows will be documented in this file.

## [2026-02-12] - Improved change-process workflow

### Changed
- **change-process.md**: Complete redesign to leverage Antigravity capabilities
  - Added automatic workflow discovery using `find_by_name`
  - Implemented parallel file reading for faster analysis
  - Added smart reference checking with `grep_search`
  - Improved validation and git diff preview
  - More conversational, less prescriptive flow
  - Removed Claude Code subagent references

### Added
- This CHANGELOG.md file for tracking workflow changes

### Reason
Adapted workflow from Claude Code architecture to Antigravity's direct execution model with better tooling support.
