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

## Human-understanding workflow
Before an assistant tells a teammate to start a significant Issue, run Codex, approve a major decision, or merge an important PR, it must explain in plain language:
- what already exists;
- what is changing;
- why the change is needed;
- which decisions belong to humans;
- what result the developer should be able to see, test, read or understand.

Humans retain final authority over product behavior, permissions/business rules, UX flows, visual direction, architecture, technology choices, scope changes and important trade-offs. AI may recommend, research, explain, draft, implement approved work and review, but must not silently turn a recommendation into a project decision.

Significant product, UX, architecture, workflow or technical proposals must first be discussed in plain language with the responsible human. The discussion should cover the recommendation, alternatives, trade-offs, assumptions and risks before the proposal is recorded on GitHub. Other teammates should then review significant proposals independently with their own assistants before final team approval; assistants should help each teammate form an independent opinion rather than simply endorse another assistant. Work asynchronously by default. Use meetings or shared consensus when there is a genuine cross-team dependency, disagreement, major phase transition or whole-project decision.

For meaningful implementation work, explain enough for the developer to understand and maintain the feature, including important files/components and their responsibilities, the data/action flow, the location of business logic, how to run or test it, and any unfamiliar framework or library concepts introduced.

Before moving from one major project phase to another, perform a human walkthrough of the completed phase. Any changed or reopened decision must be recorded on GitHub before downstream work treats it as authoritative.

Important PRs must stay small enough that another developer can genuinely understand, explain and verify them. Prefer several focused PRs over one very large AI-generated change when that improves human reviewability. Before an important PR is merged, the developer should receive a plain-language completion review covering what changed, what did not change, important decisions represented, affected files/areas, verification, and anything the developer should personally inspect or try. Before approving an important PR, the reviewer should be able to answer: **“What would break if this PR were wrong?”** Identify the practical impact on behavior, users, modules, data or workflows. Passing tests or receiving a clean AI review does not replace understanding that risk.

## Project language
- Use English for repository-facing documentation, Issues, Pull Requests, review comments, branch names, commit messages, and durable project decisions unless quoting an external source that must remain in its original language.
- Do not translate official source material itself when accuracy requires preserving the original wording.

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
- Keep PRs small enough that another developer can genuinely understand, explain and verify them. Prefer several focused PRs over one very large AI-generated change when that improves human reviewability.

## Flutter rules
Before Flutter implementation starts, the team must agree on the main module boundaries, current working responsibilities, and the important interfaces between modules. Responsibilities are task-based and can change; they do not create permanent ownership of parts of the codebase.
The repository remains shared by the whole team.

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
