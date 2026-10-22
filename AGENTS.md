# UEMF Studio — Shared Agent Instructions

## Mission
Build a production-oriented Euromed Studios application with a three-person engineering team.

## Priorities
1. Simple, readable, maintainable code.
2. Minimal merge conflicts between team members.
3. Premium, intuitive UX/UI with purposeful animations.
4. Fast and reliable user experience.
5. Security, testability and production readiness.

## Source of truth
GitHub is the source of truth for the project.
Before working on a task, read:
1. this file;
2. the linked GitHub Issue;
3. only the relevant files in `docs/`;
4. the existing implementation related to the task.

Do not rely on old chat context when repository documentation exists.

## Scope discipline
- Work on one GitHub Issue at a time.
- Modify only files required by that Issue.
- Do not silently change architecture, dependencies, naming conventions, permissions or business rules.
- If a structural change is useful, propose it explicitly and wait for team approval.
- Do not duplicate an existing component, service or model.

## Git workflow
- Never implement features directly on `main`.
- One Issue = one primary owner = one focused branch.
- Branch examples: `feat/27-reservation-summary`, `fix/42-slot-collision`, `chore/1-repository-foundation`.
- Changes enter `main` through Pull Requests.
- A different team member reviews the PR before merge.
- Prefer small PRs that are easy to understand and verify.

## Flutter rules
When Flutter development starts:
- Organize code by feature with shared core infrastructure.
- Keep widgets small and focused.
- Separate UI, business logic and data access.
- Reuse design-system components.
- Avoid hard-coded colors, typography and repeated dimensions.
- Avoid unnecessary dependencies.
- Prefer clear code over clever code.
- Keep rebuild scope small and use `const` where appropriate.
- Responsive/adaptive behavior must be considered for supported screen sizes.

## UX/UI rules
- Premium means coherent, calm and polished—not overloaded.
- Animations must provide feedback, orientation or hierarchy.
- Accessibility, loading, empty, error and offline states are part of the feature.
- Avoid flows that require unnecessary steps.

## Quality gate
Before marking a coding task complete:
- format the code;
- run static analysis;
- run relevant tests;
- inspect the final diff;
- verify acceptance criteria;
- report remaining risks or assumptions.

For Flutter, the baseline checks will include:
- `dart format`;
- `flutter analyze`;
- relevant automated tests.

## AI usage
AI-generated code is treated exactly like human-written code: it requires review and tests.

At the end of an AI coding task, report:
1. summary;
2. files changed;
3. checks/tests executed;
4. unresolved risks;
5. decisions that should be documented.

Never start another Issue automatically.
