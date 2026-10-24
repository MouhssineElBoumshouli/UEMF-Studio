# UEMF Studio — Equipment, Projects, Maintenance, Notifications and QR

- Status: Proposed functional specification
- Owner: Yassine-Benchekroun
- Related GitHub Issue: #7
- Phase: Phase 1 — Functional specification

## 1. Purpose

This document defines the V1 functional behavior owned by Issue #7:

- equipment information, requests and availability;
- audiovisual project and document workflow;
- maintenance and incident reporting;
- notification requirements;
- temporary QR access behavior;
- supporting administration needs.

It is a product specification only. It does not select Flutter architecture, backend services, databases, notification providers, QR libraries, storage providers or security mechanisms.

## 2. Sources and ownership boundaries

### 2.1 Authoritative sources used

- GitHub Issue #7 and its comments, including the Cahier des Charges addendum;
- `AGENTS.md`;
- `CONTRIBUTING.md`;
- `PLANS.md`;
- `docs/status/CURRENT.md`.

The Issue #7 addendum is the source for the required project types, production pipeline, temporary QR behavior, end-of-session reporting, real-time occupancy visibility and reservation-notification integration.

Labels follow the convention in the Issue #4 specification: `[REQ]` identifies an explicit source requirement, `[REC]` identifies a proposed product rule that is not explicit in the sources, `[DECIDED]` identifies a team-accepted V1 rule, and `[DECISION]` identifies an unresolved follow-up question. Recommendations are not binding until accepted by the team.

### 2.2 Related work owned elsewhere

- Issue #4 owns product scope, actors, roles and permissions. This document refers to actors by function and must not redefine their permission matrix.
- Issue #5 owns studios, availability and reservation lifecycle rules. This document adds equipment and maintenance constraints to the final technical validation; it does not redefine reservation states, quotas, cancellation or conflict rules.
- Issue #9 explores UX/UI separately. Any screen or interaction described here is functional behavior, not an approved visual design.

## 3. Scope and non-goals

### 3.1 In scope

The Issue #7 addendum explicitly requires the capabilities marked `[REQ]`. Other details below are proposed behavior and remain subject to review.

1. `[REC]` identify equipment and its current operational availability;
2. `[REQ]` allow a reservation to request additional mobile equipment;
3. `[REQ]` make equipment/maintenance availability capable of affecting final technical reservation validation;
4. `[REQ]` support the required audiovisual project pipeline through secure final delivery and archiving;
5. `[REQ]` let a project owner invite collaborators;
6. `[REQ]` report defective equipment or furniture through an end-of-session form;
7. `[REC]` track maintenance and incident impact on future studio or equipment availability;
8. `[REQ]` support reservation status and rejection notifications;
9. `[REQ]` issue temporary QR access only after final Level-2 approval and only during the reserved slot;
10. `[REQ]` give the Régisseur/technician function a real-time view of physical studio occupancy.

### 3.2 Out of scope for this Issue

- Flutter implementation or application code;
- final actor/role permissions;
- reservation quotas, approval states, cancellation and general availability rules;
- notification technology, channel providers and delivery infrastructure;
- QR encoding, cryptographic implementation and credential storage;
- cloud-storage vendor or digital-archiving technology;
- final data schema, API contracts and architecture;
- visual design, navigation and animation decisions.
- a full maintenance-management system; this Issue covers incident reporting and maintenance-related availability behavior only;
- IoT occupancy sensing, which Issue #4 lists as outside V1.

Items requiring an explicit team decision are marked `[DECISION]`. The eight Issue #7 V1 decisions recorded in the latest Issue #7 comments are marked `[DECIDED]`; they are approved functional rules, not implementation choices.

## 4. Actors and terms

The names below describe functional participation only. The final permission matrix remains owned by Issue #4.

| Term | Functional meaning |
| --- | --- |
| Project owner | Person responsible for an audiovisual project and its submitted material. |
| Collaborator | Person invited to participate in a project, such as a professor, student monitor or technical-team member. |
| Requester | Person who submits or owns a reservation request. |
| Approver | Actor participating in the reservation approval workflow defined by Issue #5. |
| Technical team | Technical Staff responsible for equipment status, maintenance and operational follow-up; this term does not itself grant Level-2 approval authority. |
| Responsable des Studios / Studio Approver | Function responsible for final Level-2 technical validation under Issue #4. The relationship to the Administrator's separately sourced final-slot-validation capability remains an Issue #4 decision. |
| Administrator | Operational role with capabilities defined by Issue #4, including global planning, equipment-maintenance management, project supervision and final-slot-validation capability. |
| Reservation slot | The approved studio date/time interval to which equipment and project production activity may be linked. |
| Incident | A reported defect, damage, abnormal event or operational problem affecting equipment, furniture or a studio. |
| Maintenance block | A studio or equipment unavailability interval created or maintained because of maintenance or administration. |

## 5. Equipment management

### 5.1 Equipment catalog

`[REC]` Provide a catalog of equipment that can be associated with studio use. A user viewing an equipment item should be able to distinguish the item and understand whether it can currently be requested.

`[REC]` The catalog should support, at minimum:

- a human-readable equipment name;
- an equipment category or type;
- a description sufficient to identify the item;
- an operational availability state;
- an indication when the item is unavailable because of maintenance or an administrative block.

`[DECIDED]` V1 equipment requests use catalog items or kits with an explicit quantity. The catalog/reference format and any accessories must identify what is being requested well enough for technical validation; the implementation may represent physical units and kits without changing the functional rule.

### 5.2 Equipment request in a reservation

- A reservation may include additional mobile equipment such as cameras, microphones and lighting kits.
- `[REQ]` Store requested mobile equipment references with the reservation that needs them, consistent with Issue #5.
- `[REC]` Make the request visible to the technical team and the Level-2 approver during final technical validation.
- `[REC]` Do not treat a requested item as confirmed merely because the reservation was submitted.
- `[REQ]` Equipment/maintenance availability must be able to affect final Level-2 technical validation, consistent with Issue #5.

`[DECIDED]` A request records the requested catalog item or kit, quantity, and whether it is required or optional. Substitutions must be explicit and approved through the technical validation outcome; silent substitution is not allowed. An unavailable required request blocks final technical validation. An optional request may remain unfulfilled only through an explicit outcome; it must not be silently removed or replaced.

### 5.3 Availability behavior

`[REC]` The functional availability result for a requested item should account for at least:

- existing usage commitments for overlapping approved reservation slots;
- maintenance or administrative unavailability;
- the item’s current operational status.

`[REQ]` When equipment availability is used as a Level-2 validation condition, the final validation operation must re-evaluate each relevant requested item for the full reservation interval and atomically or equivalently serialize that check with the equipment claim and the `PendingStudio -> Validated` transition. The invariant is keyed by the exclusive equipment unit or kit and overlapping reservation interval, and applies across reservations for different studios. Concurrent validations must have one winner for each conflicting item; a validation that cannot claim a required item follows the Issue #5 Level-2 availability outcome and must not be reported as a technical commit failure. This rule applies once an item is treated as a validation-blocking requirement; the open decision about optional-item fulfillment does not authorize two overlapping claims. Detailed inventory, quantity, allocation, substitution and claim-release mechanics remain open under this Issue and Issue #5.

`[REC]` If the request cannot be satisfied, the reservation should not appear fully technically validated without an explicit outcome. The decision belongs to the `Responsable des Studios` Level-2 function, with any Administrator authority handled as specified by Issue #4. Follow Issue #5 reservation outcomes and Section 8 notification requirements.

`[DECIDED]` An unavailable optional item does not automatically invalidate the studio reservation, but the validation result must explicitly record that the item is unavailable or that an approved alternative was selected. The requester must not be given an apparently complete equipment request when the optional item was silently dropped.

### 5.4 Equipment handoff and return

Issue #7 requires equipment availability to affect technical validation, but the authoritative sources do not yet define the physical handoff, return, inspection or responsibility procedure.

`[DECISION]` Follow-up details such as checkout/return records, handoff confirmation, and treatment of unresolved returns remain outside the eight accepted Issue #7 decisions.

## 6. Audiovisual projects and documents

### 6.1 Project types

`[REQ]` V1 projects must support these project types:

- Enseignement / MOOC;
- Recherche;
- Culture;
- Communication institutionnelle.

`[REC]` Select a project type when the project is created and show it in project information and filtering.

`[DECISION]` Confirm whether a project may have more than one type or whether one primary type is mandatory.

### 6.2 Project ownership and collaboration

- `[REQ]` Each project has an owner who can invite collaborators, including the examples named in the Cahier des Charges: a professor, student monitor and technical-team member.
- `[REC]` Show an invited collaborator's project relationship and participation status.
- `[REC]` Do not let project invitation silently grant unrelated reservation, administration or system permissions; apply the Issue #4 role model.

`[DECIDED]` Invitations target identified users and support accept/decline. The project owner controls collaborator removal/revocation. Access is granted explicitly per project or document and may be limited to the relevant view/contribute actions, including upload, edit or correction-request actions where granted. Collaboration does not grant ownership, reservation approval, archival/finalization authority or unrelated Issue #4 permissions.

### 6.3 Required production pipeline

`[REQ]` Every project follows this functional pipeline:

#### A. Pré-production

- `[REQ]` Pré-production includes mandatory preparatory material such as a script, conducteur d’émission or storyboard.
- `[REQ]` Record the project brief and do not treat the project as ready for production until all required preparatory material is present. An exception is not part of the proposed V1 behavior.

`[DECISION]` Confirm the mandatory document set by project type. Any exception policy remains unresolved and must not be allowed in the V1 workflow without a separate team decision.

#### B. Production

- `[REQ]` Production activity is linked automatically to reservation slots in the final `Validated` state defined by Issue #5.
- `[REC]` Show the related reservation context to the project owner and relevant collaborators, and do not present production as linked to a reservation that is not `Validated`.

The reservation system remains the source of truth for slot approval and timing. The project workflow must consume that result rather than duplicate reservation rules.

#### C. Post-production

- `[REQ]` Post-production includes an editing status and correction/modification requests.
- `[REC]` Each correction/modification request should identify the requested action and remain visible until it is resolved or otherwise closed under the final product decision.

`[DECISION]` Confirm the editing-status values, who can create or resolve correction requests, whether revisions require version history, and the maximum file-size/type rules.

#### D. Validation and archiving

- `[REQ]` The technical team performs quality control before final delivery, followed by secure final delivery through a cloud link or another digital environment.
- `[REC]` Do not consider a project finally delivered until its quality-control outcome is recorded. Keep archived projects discoverable according to the final permissions and retention policy.

The specification does not choose the cloud provider, file-storage architecture or retention technology.

`[DECIDED]` Successful technical quality control moves the project to `Archived` and makes the final deliverable securely available. A failed quality-control result returns the project to correction. Archived material is not automatically deleted before UEMF defines an institutional retention policy; delivery-link expiration may be managed independently of project retention.

### 6.4 Documents

`[REC]` Attach documents to the project and associate them with the relevant production phase. The workflow should distinguish preparatory files, production-related material, post-production material, correction requests and final delivery material.

`[DECISION]` Confirm document metadata, versioning, duplicate handling, deletion/replacement rules, preview requirements, virus/content checks and access audit requirements.

## 7. Maintenance and incident management

### 7.1 End-of-session reporting

- `[REQ]` After a session, the user can report defective equipment or furniture through an end-of-session form.
- `[REC]` Where known, the report should identify the affected equipment, furniture or studio area, describe the problem, reference the relevant session/reservation and be visible to the Technical Team or responsible Administrator for follow-up.

`[DECISION]` Confirm whether photos, severity, safety impact, location, reporter confirmation and offline submission are required in V1.

### 7.2 Incident triage and resolution

`[DECIDED]` V1 uses this incident flow:

1. `Reported`;
2. `Under Review`;
3. `In Progress`;
4. `Resolved`;
5. `Closed`.

`[DECIDED]` Incidents use `Low`, `Medium` or `High` severity. A high-severity issue affecting operational safety or usability makes the affected resource unavailable pending authorized review/resolution. The availability outcome must follow the Issue #5 rules.

This is a limited incident follow-up rule, not a full maintenance-management system. Issue #4 excludes a full maintenance-management system from V1. Assignment, target dates, reopening and service-level rules remain follow-up details.

`[DECISION]` Follow-up details such as assignment, target dates, reopening and service-level escalation remain outside the accepted V1 incident decision.

### 7.3 Availability impact

- `[REQ]` A maintenance or administrative block must prevent affected studio time from being treated as available during the blocked interval.
- `[REQ]` Creation or update of a studio maintenance/administrative block must use the same serialization or equivalent exclusion invariant as reservation submission and Level-2 validation for that studio and interval, as required by Issue #5.
- `[REC]` Do not offer equipment as technically available while it is under an active maintenance or administrative block.
- `[REC]` When a newly recorded incident or block affects an existing reservation, identify the conflict and trigger the applicable status/notification process; the exact resolution follows Issue #5 and remains to be specified by the team.

Issue #7 does not redefine the reservation conflict, cancellation or approval behavior. Those outcomes must remain consistent with Issue #5.

### 7.4 Operational occupancy

`[REQ]` The Régisseur/technician function must have a dashboard showing physical studio occupancy in real time.

`[DECIDED]` Authorized Technical Staff/Régisseur maintain `Free`, `Occupied` or `Unknown` and expose the last-updated time. Information older than the agreed operational freshness threshold is displayed as stale/`Unknown`; the threshold is configurable or subject to UEMF operational validation and is not hard-coded to 15 minutes. Access events may support the manual signal, but IoT sensing remains outside V1. Occupancy views must avoid unnecessary personal data.

## 8. Notifications

### 8.1 Required notification behavior

- `[REQ]` Reservation status changes must support the reservation workflow.
- `[REQ]` Every successfully committed Level-1 or Level-2 refusal requires a reason and immediately notifies the requester, as specified by Issue #5.
- `[REC]` Notifications should reflect only committed business outcomes; an uncommitted or unknown result should not produce a misleading success notification.
- `[REC]` A notification should identify the affected reservation or project context sufficiently for the recipient to understand what changed.

The exact notification technology and delivery channel are out of scope.

### 8.2 Event categories

The event catalog must cover the following sourced reservation events and proposed cross-module events:

- `[REQ]` reservation status or approval changes;
- `[REQ]` reservation refusal, its mandatory reason and immediate requester notification after the refusal commits;
- `[REC]` equipment availability impact that changes a technical reservation outcome;
- `[REC]` maintenance/incident impact when an affected reservation or equipment request requires user action;
- `[REC]` project collaboration invitation and project workflow changes;
- `[REC]` correction/modification requests and their resolution;
- `[REC]` quality-control or final-delivery outcome when a project participant must act.

The cross-module event categories remain functional notifications only; delivery technology stays out of scope.

`[DECIDED]` Requesters receive reservation status, approval/rejection, cancellation/expiry and action-required notifications. Approvers receive requests in their scope; project participants receive relevant collaboration, correction, quality-control and delivery events; Technical/Admin users receive operational incident notifications. Notifications expose read/unread state, use stable operation/event identity for deduplication, retry delivery as appropriate, and must not duplicate a committed refusal after reconciliation. Mandatory reservation outcome notifications cannot be muted; optional project events may be configurable. Delivery technology remains out of scope.

### 8.3 Notification boundaries

- This document does not select push, email, SMS, in-app or any other delivery technology.
- `[REC]` The system must not expose sensitive project files, private links or unnecessary personal data in notification previews.
- A notification is an outcome communication, not a replacement for the authoritative reservation, project or incident record.

## 9. QR access behavior

### 9.1 Issuance

- `[REQ]` After final Level-2 reservation approval, the system generates a secure temporary QR code associated with the approved reservation slot.
- `[REC]` QR access should not become active before the final `Validated` state defined by Issue #5.

### 9.2 Validity window

- `[REQ]` The QR code is active only during the reserved slot.
- `[REQ]` Before the slot starts or after it ends, the QR code must not grant access.
- `[REC]` If the reservation is cancelled, rejected, expired or otherwise no longer valid, the QR code must not grant access.

`[DECIDED]` The default QR validity interval is `[start, end)` in the campus timezone, with no early or late grace period. Any grace period or early-setup behavior requires explicit UEMF approval. Multiple valid scans during the active interval are allowed; a QR code is not freely shareable and access remains attributable to an authorized reservation participant.

### 9.3 Scan outcome

- `[REC]` A valid scan during the validity window should permit the approved access behavior for that reservation.
- `[REC]` An invalid, expired, cancelled or out-of-window scan should be refused without revealing unnecessary reservation or personal data.
- `[REC]` The user-facing result should clearly distinguish access granted from access refused without exposing implementation details.
- `[REC]` QR access should not bypass the reservation approval workflow or create a reservation by itself.

`[DECIDED]` Scans by authorized reservation participants may be accepted repeatedly during the active interval. A non-participant, invalid, expired or out-of-window scan is refused. Record only the minimum information necessary for access and audit purposes, such as the reservation, result and scan time, and avoid unnecessary personal data. A false refusal follows the authorized technical support path and must not bypass reservation validation.

### 9.4 Security and privacy boundary

`[REQ]` The product requirement is secure, temporary, reservation-scoped access. The implementation must later choose an approved secure mechanism, but Issue #7 does not prescribe an algorithm, token format, cryptographic library or storage system.

## 10. Supporting administration requirements

Issue #4 assigns Administrators global planning, equipment-maintenance management, final-slot validation and global project supervision. The Issue #7 supporting capabilities are:

- `[REQ]` maintain equipment and its maintenance status, and support final-slot validation consistent with the Issue #4 role model;
- `[REQ]` see current physical studio occupancy through the Régisseur/technician function;
- `[REC]` review end-of-session incident reports and support maintenance/administrative blocks;
- `[REC]` support project quality control, final-delivery handling, collaboration and correction follow-up within approved permissions;
- `[REC]` preserve links between reservations, equipment requests, incidents and projects.

This supporting scope does not constitute a full maintenance-management system, which Issue #4 lists outside V1.

`[DECIDED]` V1 administration provides search/filter across reservations, projects, equipment and incidents, basic operational reports, and audit history for important privileged actions. Advanced analytics/BI, bulk mutations and broad exports are deferred unless separately required. Delegation and retention rules remain governed by the Issue #4 role model and institutional policy.

Administrative capabilities must follow the role and permission definitions accepted under Issue #4. This document must not grant access by implication.

## 11. Cross-module invariants

The following sourced requirements and proposed integration rules apply across workflows:

1. `[REQ]` An equipment request is part of the reservation’s final technical context.
2. `[REQ]` Studio maintenance/administrative blocks affect the availability decision under Issue #5; `[REC]` apply equipment blocks to equipment availability as well.
3. `[REQ]` A project’s production activity references reservation slots in `Validated` state; it does not define a separate reservation state.
4. `[REQ]` Final QR access depends on final reservation approval and the reserved time window.
5. `[REC]` Incident and availability changes should be visible to the responsible technical/administrative workflow.
6. `[REC]` Notifications should communicate committed outcomes and point to, rather than replace, authoritative records.
7. `[REQ]` Role permissions come from Issue #4 and must not be duplicated or contradicted here.

## 12. Acceptance-criteria traceability

| Issue #7 criterion | Covered by |
| --- | --- |
| Equipment workflow is defined | Section 5: catalog, requests, availability and open decisions |
| Project/document workflow is defined | Section 6: types, collaboration, four-phase pipeline and documents |
| Maintenance/incident workflow is defined | Section 7: end-of-session reporting, triage, resolution and availability impact |
| Notification requirements are defined | Section 8: required events, outcomes and boundaries |
| QR behavior is defined at product level only | Section 9: issuance, validity window, scan outcomes and security boundary |
| No Flutter implementation is added | This document contains functional requirements only; no code or technology choice |

## 13. Decisions recorded for Issue #7

The latest Issue #7 comments record team agreement on the eight remaining V1 decisions. The accepted rules are incorporated above and are marked `[DECIDED]`:

- equipment quantity/kit, required-versus-optional and explicit substitution behavior;
- identified-user collaboration invitations, accept/decline, revocation and explicit project/document permissions;
- incident status flow, severity and high-impact availability blocking;
- occupancy states, last-updated information and configurable freshness;
- notification recipients, read/unread state, deduplication, retries and mandatory reservation outcomes;
- QR validity window, no default grace period, multiple scans, participant validation and minimum audit data;
- project quality control, archival, secure final delivery and retention boundary;
- administration search/filtering, basic reporting and privileged-action audit history.

Additional implementation-level questions that are not needed to change these V1 rules remain marked `[DECISION]` as follow-up work. Issue #7 should remain open until this updated specification is reviewed and accepted through the PR workflow; no Flutter implementation is authorized by these decisions.
