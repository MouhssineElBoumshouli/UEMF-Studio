# V1 product scope, actors and roles

Status: Proposed for Issue #4

## 1. Purpose and scope discipline

**[REQ]** This specification defines the V1 product boundary, actors, functional roles, permissions and role-related business rules for Euromed Studios. It gives Issues #5 and #7 a shared, high-level role model to reference.

**[REQ]** This is functional specification work only. It does not decide Flutter architecture, state management, backend, database, API design, authentication implementation, storage provider, notification technology, QR implementation, or deployment and infrastructure. Those topics belong to later planning or their owning workstreams.

**[REC]** Treat the labels below as source discipline: `[REQ]` is explicitly required by the Cahier des Charges or Issue #4 addendum; `[REC]` is a product recommendation; `[DECISION]` identifies an open question requiring team/UEMF validation. A recommendation is not an approved requirement until the team accepts it.

## 2. V1 product boundary

**[REQ]** V1 is a role-based operational application for Euromed Studios that supports, at a high level:

- discovery of studio spaces and their functions;
- studio reservation, including requester-dependent Level-1 approval followed by final technical Level-2 approval;
- audiovisual project support;
- equipment and maintenance support;
- temporary QR access associated with a validated studio slot;
- operational and administrative supervision.

The source names these studio areas:

| Area | Source-listed spaces/functions |
| --- | --- |
| Production | Plateau TV; Studio Podcast; Euromed Radio; Chroma Key Studio |
| Digital pedagogy | Studio Pédagogique; Speed MOOC |
| Post-production | Video/audio editing stations; centralized technical control rooms (régies) |

**[DECISION]** This list reflects the Cahier des Charges at a product level; it does not establish which spaces are bookable studios versus supporting areas. Issue #8 records additional names in the supplied technical matrix. The definitive inventory and bookable status belong to Issue #5 and remain open there.

**[REQ]** Reservation creation, availability, cancellation, collisions, quotas and reservation state transitions are detailed by [Issue #5](https://github.com/MouhssineElBoumshouli/UEMF-Studio/issues/5). Equipment allocation, project/document workflow, maintenance/incident handling, notifications and functional QR behavior are detailed by [Issue #7](https://github.com/MouhssineElBoumshouli/UEMF-Studio/issues/7). This document defines only their relationship to V1 actors and authority.

## 3. Role-model principles

The following is a **[REC] product model**, not a technical architecture decision:

- **Organizational profile** describes a person's institutional relationship, such as Student, Professor / Enseignant-Chercheur, Communication Team member or External Client.
- **Privileged authorization role** describes a scoped decision or operational function, such as Entity Approver, Studio Approver or Administrator.
- **Contextual role** describes what someone is doing in a specific group, project or activity, such as an authorized Club Representative or a project collaborator.
- **Organization/group** is the entity an action may represent, such as an establishment or Student Club.

**[REC]** One individually identified user may hold multiple compatible roles. Privileged roles should be limited to their defined scope and should not imply unlimited authority. Visibility should follow least privilege.

**[REC] [DECISION]** Self-approval should be prohibited as a safeguard. The Cahier des Charges does not state this rule, so the team/UEMF must validate it before treating it as binding.

**[REC]** A Student Club should be represented as an organization/group, not a shared credential. Individually identified, authorized Club Representatives would act on its behalf, with actions attributable to the person and the club. Representative assignment and revocation remain unresolved below.

## 4. Required actors and functional roles

### Student

**[REQ]** Students have limited access by default, can view studio spaces and functions, have limited project access, and are subject to hierarchical validation for reservation/project requests. The source maps Students to establishment / **Responsable de la Vie Étudiante** for Level-1 reservation approval.

**[DECISION]** The exact limits on student project access and any additional reservation eligibility conditions need validation.

### Professor / Teacher-Researcher

**[REQ]** Professors / **Enseignants-Chercheurs** receive priority for Studio Pédagogique and Speed MOOC. Approved online-course projects require follow-up. Their Level-1 reservation approval is by the Dean or Director of their establishment.

**[DECISION]** The mechanism for applying “priority” is unspecified. It must not be assumed to bypass approval or automatically displace an existing valid reservation.

### Communication Team

**[REQ]** The Communication Team supports institutional communication projects, notably those using Studio Podcast and Plateau TV. Level-1 reservation approval is by the **Directeur de la Communication**.

### Student Club / Club Representative

**[REQ]** Clubs have a shared group profile for validated cultural activities, with responsibility linked to the **Direction des Affaires Estudiantines**. The Level-1 mapping for Students & Clubs is establishment / **Responsable de la Vie Étudiante**.

**[REC]** An individually identified Club Representative acts for the club; the club remains the represented organization.

**[DECISION]** Confirm who designates and revokes representatives, the exact Student Life versus establishment approval routing, and what qualifies as an eligible cultural activity.

### External Client

**[REQ]** External Clients use a commercial profile, require specific authorization, and have pricing/quotation follow-up. Level-1 reservation approval is by the **Vice-Présidence Exécutive**.

**[REC]** V1 may track a commercial/quotation context without implying billing, invoicing, payment or accounting functionality.

**[DECISION]** Define the exact quotation scope and workflow.

### Technical Team / Technical Staff

**[REQ]** The Technical Team handles equipment maintenance, receives and follows planning, and participates in technical operations. The Cahier also requires a Régisseur/technician dashboard or function for monitoring physical studio occupancy.

**[REC]** Do not create a separate Régisseur authorization role unless the team confirms that it carries authority beyond the technician dashboard/function.

**[DECISION]** Reservation-submission and approval rights for Technical Staff are not defined by the source.

### Entity Approver

**[REC]** Entity Approver is a generic functional role for the source's Level-1 authorities, scoped to the relevant establishment, requester category or organizational function. It is a role concept, not a new institution-wide office.

Required Level-1 mapping **[REQ]**:

| Requester category | Level-1 authority |
| --- | --- |
| Students & Clubs | Establishment / Responsable de la Vie Étudiante |
| Professors & Researchers | Dean or Director of the establishment |
| Communication | Directeur de la Communication |
| External Clients | Vice-Présidence Exécutive |

**[REQ]** Level-1 approval advances the request to the next approval level; Level-1 rejection requires a reason. **[REC]** Entity Approvers see only requests within their authorized scope. They do not receive global administrative permissions by virtue of this role.

### Studio Approver / Responsable des Studios

**[REQ]** Final Level-2 technical validation belongs to the **Responsable des Studios** function.

**[REC]** Model Studio Approver separately from Administrator so the final technical function and operational administration remain distinguishable. The Cahier also gives Administrators final-slot-validation capability; this specification does not assume the two functions are held by the same organizational position.

### Administrator

**[REQ]** Administrators have a global planning view, manage equipment maintenance, can validate final slots, and supervise projects globally.

**[REC]** “Administrator” here means an operational/business Studio Administrator. It does not, by itself, mean an unrestricted technical super-administrator or grant every possible business action.

### Régisseur

**[REQ]** The Régisseur/technician function includes a dashboard for monitoring physical studio occupancy.

**[REC] [DECISION]** Keep Régisseur as a function/dashboard associated with Technical Staff unless UEMF confirms a distinct privileged role and its authority.

## 5. Permission matrix

`[REQ]` = sourced permission or responsibility; `[REC]` = recommended; `TBD / [DECISION]` = permission is not defined and requires validation. A sourced role responsibility does not imply other permissions in the same row.

| Actor / role | View eligible studio information | Submit reservation when eligible | View own / organizational requests | Level-1 approval | Level-2 approval | Project participation / management (high level) | Report incidents | Manage maintenance | Operational planning / Régisseur view | Global supervision |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Student | [REQ] | [REQ] Subject to eligibility and hierarchy | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | [REQ] Limited project access | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] |
| Professor / Researcher | TBD / [DECISION] | [REQ] Subject to hierarchy | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | [REQ] Follow-up for approved online-course projects | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] |
| Communication Team | TBD / [DECISION] | [REQ] Subject to requester routing | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | [REQ] Institutional communication projects | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] |
| Club Representative | TBD / [DECISION] | [REQ] For validated cultural activity, subject to routing | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | [REQ] Validated cultural activities for the club | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] |
| External Client | TBD / [DECISION] | [REQ] Only with specific authorization | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] |
| Technical Staff | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | [REQ] Participates in technical operations | TBD / [DECISION] | [REQ] Equipment maintenance | [REQ] Receives/follows planning; technician occupancy dashboard | TBD / [DECISION] |
| Entity Approver | TBD / [DECISION] | TBD / [DECISION] | [REC] Within authorized scope | [REQ] Approve and reject with mandatory reason | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] |
| Studio Approver | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | [REQ] Final technical validation | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] | TBD / [DECISION] |
| Administrator | TBD / [DECISION] | TBD / [DECISION] | [REQ] Global planning view; request visibility details TBD / [DECISION] | TBD / [DECISION] | [REQ] Final-slot validation capability | [REQ] Global project supervision | TBD / [DECISION] | [REQ] Equipment-maintenance management | [REQ] Global planning view | [REQ] Global operational supervision |

Where eligibility, visibility or operational rights are not established, they remain explicitly `TBD / [DECISION]`; the table does not imply a grant.

## 6. Cross-cutting business rules

### Source requirements

- **[REQ]** Role-based access control governs user journeys and rights.
- **[REQ]** Reservation requests follow sequential Level-1 entity/business approval, then final Level-2 Studios technical approval. Level-1 is not final approval.
- **[REQ]** Rejection at either reservation approval level requires a reason.
- **[REQ]** Student access is restricted by default.
- **[REQ]** Professors / Researchers have priority for Studio Pédagogique and Speed MOOC.
- **[REQ]** External Clients require specific commercial authorization and a pricing/quotation context.
- **[REQ]** The Technical Team has equipment-maintenance and planning follow-up responsibilities.
- **[REQ]** Administrators have global operational supervision, equipment-maintenance management and final-slot-validation capabilities.

### Product recommendations

- **[REC]** A user may hold multiple compatible roles; each action is still evaluated against its applicable scope.
- **[REC] [DECISION]** Prohibit self-approval if validated by the team/UEMF.
- **[REC]** Keep authority scoped to the relevant establishment, requester category, project or operational function.
- **[REC]** A club remains the owner of its group activity while each representative's action is attributable to that individual.
- **[REC]** Global Administrator visibility does not imply unrestricted authority to perform every business action.
- **[REC]** Project ownership/collaboration and reservation requester identity are distinct concepts; project collaborator status alone should not imply reservation approval authority.

Detailed reservation behavior, quotas, eligibility, availability, lifecycle and cancellation rules belong to Issue #5. Detailed project/equipment/maintenance/incident/notification/QR workflows belong to Issue #7.

## 7. Explicitly out of V1 / Issue #4 scope

This Issue does not define or commit V1 to:

- billing, payment or accounting;
- a full CRM;
- a full club-management system;
- a full maintenance-management system;
- AI media processing;
- IoT occupancy sensing;
- public or anonymous booking;
- technical architecture choices.

**[REQ]** Detailed reservation workflow remains owned by Issue #5. Detailed equipment, project/document, maintenance/incident, notification and QR workflows remain owned by Issue #7. This scope document does not reproduce or supersede those specifications.

## 8. Decision register

| Open decision | Current treatment / coordination |
| --- | --- |
| Administrator vs Studio Approver organizational assignment | **[DECISION]** Both capabilities are sourced; confirm whether they belong to one position or separate positions. |
| Multiple roles per user | **[DECISION]** Recommended in the role model; confirm compatible combinations and handling. |
| Self-approval | **[DECISION]** Recommended prohibition; confirm policy. |
| Assignment and revocation of privileged roles | **[DECISION]** Authority and process are unspecified. |
| Student vs Club Level-1 routing | **[DECISION]** Preserve the sourced establishment / Responsable de la Vie Étudiante mapping; confirm exact routing and any delegation distinction. |
| Club Representative designation/revocation | **[DECISION]** Identify the responsible authority and validation process. |
| Eligible Club activities | **[DECISION]** Define what counts as a validated cultural activity. |
| Professor priority mechanism | **[DECISION]** Define how priority is applied without assuming approval bypass or displacement of a valid reservation. |
| Exact role quotas | **[DECISION]** Issue #5 owns quota mechanics. Confirm exact values and counting policy; the Cahier's “2h/week for a Student profile” is an example until confirmed. |
| External Client quotation scope | **[DECISION]** Define the quotation/pricing follow-up; do not infer billing, invoicing or payment. |
| Régisseur as separate privileged role | **[DECISION]** Confirm only if it has distinct authority beyond the technician occupancy dashboard/function. |
| Technical Staff reservation / approval rights | **[DECISION]** Not defined by the source. |
| Project creation eligibility by persona | **[DECISION]** Define who may create projects; sourced access/follow-up responsibilities do not settle creation rights. |
| Guest or external collaborator policy | **[DECISION]** Issue #7 requires project collaboration; confirm guest identity and access policy there. |
| V1 bookable studio inventory | **[DECISION]** Issue #8 records a discrepancy between the Cahier and technical matrix; Issue #5 owns bookable inventory and availability details. |
| Student project-access limits | **[DECISION]** The source requires limited access but does not define its exact boundaries. |

## 9. Acceptance checklist

- [x] V1 scope explicit
- [x] Each source-required user role and approval function defined
- [x] Permissions stated where sourced; undefined permissions marked `TBD / [DECISION]`
- [x] Main role-related cross-cutting business rules documented
- [x] Out-of-scope items listed
- [x] Unresolved items clearly marked
- [x] No Flutter implementation added
- [x] No architecture decisions made

## Source references

- [Issue #4 — Phase 1: define product scope, actors and roles](https://github.com/MouhssineElBoumshouli/UEMF-Studio/issues/4), including its authoritative Cahier des Charges addendum.
- [Issue #5 — studios, availability and reservation rules](https://github.com/MouhssineElBoumshouli/UEMF-Studio/issues/5) and [PR #6](https://github.com/MouhssineElBoumshouli/UEMF-Studio/pull/6).
- [Issue #7 — equipment, projects, maintenance and notifications](https://github.com/MouhssineElBoumshouli/UEMF-Studio/issues/7), including its Cahier des Charges addendum.
- [Issue #8 — Phase 1 coordination and unresolved source questions](https://github.com/MouhssineElBoumshouli/UEMF-Studio/issues/8).

