# UEMF Studio — Project Plan

This is the high-level roadmap. Detailed work is tracked through GitHub Issues.

## Phase 0 — Repository & team foundation
- collaboration rules;
- AI-agent instructions;
- Issue/PR workflow;
- decision log;
- branch protection.

## Phase 1 — Functional specification
- complete scope;
- personas;
- roles and permissions;
- business rules;
- edge cases;
- V1 vs later scope.

## Phase 2 — UX and product flows
- information architecture;
- user journeys;
- reservation flow;
- approval flow;
- project flow;
- technician/admin flows;
- error/empty/offline states.

## Phase 3 — Design system
- visual direction;
- typography;
- spacing;
- color tokens;
- components;
- motion principles;
- responsive rules;
- accessibility.

## Phase 4 — Technical architecture
- Flutter architecture;
- state management;
- backend choice;
- database model;
- API contracts;
- authentication/authorization;
- file storage;
- notifications;
- QR access;
- observability.

## Phase 5 — Foundation implementation
- project bootstrap;
- environments/configuration;
- CI;
- routing;
- theme/design system;
- authentication shell;
- network/data foundation.

## Phase 6 — Core product modules
- studios;
- availability/calendar;
- reservations;
- equipment;
- approval workflow;
- projects;
- documents;
- notifications;
- QR access;
- incidents/maintenance;
- dashboards/admin.

## Phase 7 — Quality & production readiness
- automated testing;
- performance;
- security review;
- accessibility;
- analytics/monitoring;
- backup/recovery considerations;
- release process;
- deployment.

## Delivery and quality gates
- There is no hard delivery deadline. Incremental delivery is encouraged, but every increment must fit a coherent production architecture and must not rely on avoidable throwaway work or lowered quality standards.
- Before implementation begins, the team must review and approve the Phase 2 UX/product flows and the Phase 3 design system to a production-handoff standard, including accessibility and responsive/adaptive behavior.
- Before implementation begins, the team must document and accept the Phase 4 technical architecture, including module boundaries and interfaces, authentication/authorization, backend/API/database design, data protection, observability and recovery considerations.
- Implementation and later increments must preserve production-quality expectations for validation and concurrency correctness, auditability where required, automated testing, CI/CD, environment management, performance, reliability, maintainability and operational readiness.
- The accepted Phase 1 specifications remain the functional baseline unless explicitly reopened. Open UEMF policy questions must remain open or configurable rather than being invented.

## Current rule
Do not start Flutter implementation until the Phase 2 UX/product flows, Phase 3 design system and Phase 4 architecture gates are documented, reviewed and accepted.
