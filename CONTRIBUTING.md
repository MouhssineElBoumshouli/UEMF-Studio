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

## 2. Human-understanding workflow
Before an assistant tells a teammate to start a significant Issue, run Codex, approve a major decision, or merge an important PR, explain in plain language:
- what already exists;
- what is changing;
- why the change is needed;
- which decisions belong to humans;
- what result the developer should be able to see, test, read or understand.

Humans retain final authority over product behavior, permissions/business rules, UX flows, visual direction, architecture, technology choices, scope changes and important trade-offs. AI may recommend, research, explain, draft, implement approved work and review, but must not silently turn a recommendation into a project decision.

Discuss significant product, UX, architecture, workflow or technical proposals in plain language with the responsible human before recording them on GitHub. Cover the recommendation, alternatives, trade-offs, assumptions and risks. Other teammates should review significant proposals independently with their own assistants before final team approval so they can form their own opinions. Work asynchronously by default; use meetings or shared consensus only for a genuine cross-team dependency, disagreement, major phase transition or whole-project decision.

For meaningful implementation work, explain the important files/components and their responsibilities, the data/action flow, where business logic lives, how to run or test the work, and any unfamiliar framework or library concepts introduced.

Before moving from one major project phase to another, perform a human walkthrough of the completed phase. Record any changed or reopened decisions on GitHub before downstream work treats them as authoritative.

## 3. Create a focused branch
Update local `main`, then create a branch dedicated to the Issue.

Naming examples:
- `feat/12-studio-details`
- `fix/34-reservation-collision`
- `docs/8-role-permissions`
- `chore/1-repository-foundation`

Do not work directly on `main`.

## 4. Implement only the Issue scope
Read `AGENTS.md` first.

Do not bundle unrelated cleanup, dependency upgrades or architecture changes into the same PR.

If a new decision is required, raise it explicitly. Durable architectural/process decisions belong in `docs/decisions/`.

## 5. Verify locally
Before opening a PR:
- format;
- run analysis/linting;
- run relevant tests;
- review your own diff;
- verify the Issue acceptance criteria.

## 6. Open a Pull Request
A PR must:
- link the Issue;
- explain what changed;
- explain how it was verified;
- identify risks or follow-up work;
- remain small enough that another developer can genuinely understand and explain the change. Prefer several focused PRs over one very large AI-generated PR when that improves human reviewability.

## 7. Review
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

Before approving an important PR, the reviewer should be able to answer: **“What would break if this PR were wrong?”** Identify the practical impact on behavior, users, modules, data or workflows. Passing tests or receiving a clean AI review does not replace understanding that risk.

Before an important PR is merged, the developer must receive a plain-language completion review covering:
- what changed;
- what did not change;
- important decisions represented;
- affected files/areas;
- how it was verified;
- anything they should personally inspect or try.

## 8. Merge
Use the team's chosen merge method consistently once agreed.

Delete obsolete feature branches after merge.

## Conflict prevention
Before Flutter implementation starts, the team must agree on the main module boundaries, current working responsibilities, and the important interfaces between modules. Responsibilities are task-based and can change; they do not create permanent ownership of parts of the codebase. The repository remains shared by the whole team.

Before starting an Issue, check the current assignments and active PRs.
Avoid assigning two people to modify the same shared module simultaneously when tasks can be sequenced.

When unavoidable, agree on interfaces/contracts first.
