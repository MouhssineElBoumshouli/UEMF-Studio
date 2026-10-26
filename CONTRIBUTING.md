# Contributing to UEMF Studio

This repository is maintained by a three-person engineering team. The workflow is designed to reduce conflicts and make AI-assisted work reviewable.

## Project language
Use English for repository-facing documentation, Issues, Pull Requests, review comments, branch names, and commit messages. Preserve source-language quotations only when needed for accuracy.

## 1. Start from an Issue
No feature, fix or structural change starts without a GitHub Issue.

Each Issue should contain:
- context;
- goal;
- acceptance criteria;
- relevant business rules;
- out-of-scope items when useful.

One person is the primary owner of an Issue.

## 2. Create a focused branch
Update local `main`, then create a branch dedicated to the Issue.

Naming examples:
- `feat/12-studio-details`
- `fix/34-reservation-collision`
- `docs/8-role-permissions`
- `chore/1-repository-foundation`

Do not work directly on `main`.

## 3. Implement only the Issue scope
Read `AGENTS.md` first.

Do not bundle unrelated cleanup, dependency upgrades or architecture changes into the same PR.

If a new decision is required, raise it explicitly. Durable architectural/process decisions belong in `docs/decisions/`.

## 4. Verify locally
Before opening a PR:
- format;
- run analysis/linting;
- run relevant tests;
- review your own diff;
- verify the Issue acceptance criteria.

## 5. Open a Pull Request
A PR must:
- link the Issue;
- explain what changed;
- explain how it was verified;
- identify risks or follow-up work;
- remain small enough that another developer can genuinely understand and explain the change. Prefer several focused PRs over one very large AI-generated PR when that improves human reviewability.

## 6. Review
At least one other team member reviews the PR before merge.

Review for:
- correctness;
- business rules;
- architecture;
- maintainability;
- UX/UI consistency;
- performance;
- security;
- tests;
- unnecessary scope.

AI review can assist, but a human remains responsible for approval.

Before approving an important PR, the reviewer should be able to answer: **“What would break if this PR were wrong?”** Identify the affected behavior, users, modules, data or workflow. Tests and AI review support this judgment; they do not replace it.

## 7. Merge
Use the team's chosen merge method consistently once agreed.

Delete obsolete feature branches after merge.

## Conflict prevention
Before Flutter implementation begins, the team must agree on the main module boundaries, primary ownership and interfaces between shared areas such as reservations, projects, equipment, notifications, QR access, incidents/maintenance and the shared design system.

Before starting an Issue, check the current assignments and active PRs.
Avoid assigning two people to modify the same shared module simultaneously when tasks can be sequenced.

When unavoidable, agree on interfaces/contracts first.
