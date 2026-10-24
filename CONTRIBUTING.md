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
- remain reasonably small.

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

## 7. Merge
Use the team's chosen merge method consistently once agreed.

Delete obsolete feature branches after merge.

## Shared coordination control
Cross-team routing is kept in the shared Google Sheet **UEMF Studio — Control Center**:
https://docs.google.com/spreadsheets/d/1m3_YMmqmdqU5cel-kCm3O23cX2nd8t5FbAR6IBpPBMU/edit

GitHub remains authoritative for Issues, PRs, reviews, and decisions. The Control Center only provides a simple current-action view for the three teammates and their assistants.

Project assistants must keep the relevant row synchronized with GitHub and must not require humans to relay routine technical instructions through WhatsApp or GitHub notification emails.

## Conflict prevention
Before starting an Issue, check the current assignments and active PRs.
Avoid assigning two people to modify the same shared module simultaneously when tasks can be sequenced.

When unavoidable, agree on interfaces/contracts first.
