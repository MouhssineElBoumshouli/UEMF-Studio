# UEMF Studio

A professional mobile application for managing Euromed Studios, developed by a three-person engineering team.

## Goal

Build an application that is fast, intuitive, maintainable, visually polished, and suitable for real institutional use.

The project priorities are:

1. simple, readable, maintainable code;
2. smooth collaboration with minimal Git conflicts;
3. premium, coherent, accessible UX/UI;
4. speed, reliability, and security;
5. disciplined use of AI assistants without allowing chats to become the source of truth.

## Source of truth

This GitHub repository is the official source of truth for the project.

ChatGPT and local Codex are used for planning, implementation, and review. Durable decisions must be recorded in GitHub.

## Before contributing

Read, in this order:

1. [AGENTS.md](AGENTS.md)
2. [CONTRIBUTING.md](CONTRIBUTING.md)
3. [PLANS.md](PLANS.md)
4. [docs/status/CURRENT.md](docs/status/CURRENT.md)
5. the assigned GitHub Issue;
6. only the documentation relevant to that Issue.

## Workflow

```text
GitHub Issue
    ↓
1 primary owner
    ↓
dedicated branch
    ↓
implementation or documentation work
    ↓
checks / verification
    ↓
Pull Request
    ↓
review by another team member
    ↓
merge into main
```

No feature work is developed directly on `main`.

## Decisions

Durable technical and process decisions are recorded as ADRs in [docs/decisions](docs/decisions).

## Current status

Phase 1 — Functional specification is complete. The project is now entering **Phase 2 — UX and product flows**, with Issue #9 as the current UX/UI exploration work.

- Product scope, actors, and roles are complete.
- Studios, availability, and reservation rules are complete.
- Equipment, projects, maintenance/incidents, notifications, and QR behavior are complete for the accepted Phase 1 baseline.
- Flutter implementation has **not** started.

The team is targeting a coherent, good working MVP first using the approved Phase 1 baseline. Non-essential unanswered UEMF policy details may remain open, configurable or deferred without blocking the MVP, but the project must not invent UEMF policy or contradict the Cahier des Charges.

See [docs/status/CURRENT.md](docs/status/CURRENT.md) and the active GitHub Issues for the latest status and details.
