# ADR-0002 — AI-assisted development workflow

- Status: Proposed
- Date: 2026-10-22
- Owners: Team

## Context
The team uses ChatGPT/Codex and Claude/Claude Code. Independent assistants can produce inconsistent code if each relies on private conversation history.

## Decision
GitHub documentation is the shared memory for all humans and AI agents.

`AGENTS.md` defines common agent rules.
`CLAUDE.md` imports the common rules and adds Claude-specific instructions.

AI coding tasks follow these principles:
- one Issue at a time;
- fresh coding-agent conversation for a new task when practical;
- read only relevant repository context;
- do not silently change architecture or dependencies;
- run relevant checks;
- report changes, tests and risks;
- submit work through a branch/PR;
- AI output requires human review.

Chat conversations are working spaces, not authoritative project documentation.

## Alternatives considered
### One giant shared chat
Rejected because it becomes noisy, expensive and hard to keep authoritative.

### Separate undocumented AI contexts
Rejected because assistants would gradually diverge.

## Consequences
### Positive
- consistent instructions across tools;
- reduced repeated context/tokens;
- easier onboarding;
- better traceability;
- independent AI cross-review becomes possible.

### Negative / trade-offs
- repository documentation must be kept current;
- prompts still need clear task scope.

## Validation
Review whether agents:
- follow Issue scope;
- avoid duplicate components;
- respect decisions;
- produce smaller, reviewable diffs.

## Related
- Issue #1
