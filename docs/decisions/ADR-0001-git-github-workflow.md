# ADR-0001 — Git and GitHub workflow

- Status: Proposed
- Date: 2026-10-22
- Owners: Team

## Context
Three developers and multiple AI coding agents will work on the same project. Uncontrolled direct edits increase merge conflicts, duplicated work and inconsistent architecture.

## Decision
GitHub is the source of truth.

The team uses:
- one GitHub Issue per focused task;
- one primary owner per Issue;
- one dedicated branch per Issue;
- no implementation directly on `main`;
- Pull Requests for integration;
- review by another human team member before merge;
- small, focused changes instead of large mixed PRs.

Branch names include the Issue number when practical.

## Alternatives considered
### Direct work on main
Rejected because it makes review, rollback and parallel work unsafe.

### Long-lived personal branches
Rejected for normal feature development because they drift and create difficult merges.

## Consequences
### Positive
- fewer conflicts;
- easier reviews;
- clear responsibility;
- traceability between requirements and code;
- easier AI review.

### Negative / trade-offs
- additional Issue/PR overhead;
- team members must keep branches up to date.

## Validation
After several Issues, evaluate:
- merge conflict frequency;
- PR size;
- review time;
- duplicated work.

## Related
- Issue #1
