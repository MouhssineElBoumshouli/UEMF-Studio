# Studios, availability and reservation rules

Status: Proposed for Issue #5

## Scope

This document defines the V1 behavior for:
- studio information;
- availability;
- reservation submission;
- sequential approval and rejection;
- cancellations;
- conflicts, quotas and edge cases.

Role and permission mapping is owned by Issue #4. Detailed equipment allocation, maintenance workflow and notification technology are owned by Issue #7. This document defines only the reservation behavior that depends on those areas.

## Studio information

A studio should expose the information a user needs before booking:
- name;
- short description;
- location;
- capacity;
- opening/available hours;
- main equipment or amenities;
- photos when available;
- current availability;
- any booking restrictions relevant to the user.

The definitive V1 list of bookable studios is intentionally not fixed here. Issue #8 records an unresolved discrepancy between the Cahier des Charges and the supplied technical matrix; this specification does not choose which named spaces are bookable studios and which are supporting areas.

A studio that is unavailable for maintenance or administration must not appear bookable for the affected period.

## Availability

Availability is based on the campus-local date and time.

A time slot is bookable only when:
- the studio is open for that period;
- the current campus-local time is before the requested start time;
- the requested start time is before the end time;
- the interval does not overlap another blocking reservation;
- the studio is not blocked for maintenance or another administrative reason.

V1 must pre-lock a submitted interval while it is pending approval, so availability is not offered to competing requests during review.

## Reservation lifecycle

The canonical V1 reservation states are:

1. **PendingEntity** — submitted and waiting for the requester-dependent entity/business approval. User-visible meaning: **En attente entité**.
2. **PendingStudio** — the entity/business approval is complete and the request is waiting for final Studios technical validation. User-visible meaning: **En attente studio**.
3. **Validated** — both approval levels are complete and the reservation is fully validated. User-visible meaning: **Validée**.
4. **Rejected** — refused at either approval level with a mandatory reason.
5. **Cancelled** — cancelled by the requester before the reservation start time.
6. **Expired** — a PendingEntity or PendingStudio reservation that reached its start time before the required approvals were complete.
7. **Completed** — a Validated reservation that automatically reaches this state when its reserved period ends.

The user-visible progression for a successful reservation is:

**En attente entité -> En attente studio -> Validée**

There is no standalone generic single-reviewer Approved state in this workflow. `Validated` is the fully approved state.

Allowed transitions:
- PendingEntity -> PendingStudio after successful Level-1 approval;
- PendingEntity -> Rejected after a Level-1 refusal;
- PendingEntity -> Cancelled before the start time;
- PendingEntity -> Expired automatically when the start time is reached;
- PendingStudio -> Validated after successful Level-2 approval;
- PendingStudio -> Rejected after a Level-2 refusal;
- PendingStudio -> Cancelled before the start time;
- PendingStudio -> Expired automatically when the start time is reached;
- Validated -> Cancelled before the start time;
- Validated -> Completed automatically when the end time is reached.

Rejected, Cancelled, Expired and Completed are terminal states. No reservation may move directly from PendingEntity to Validated.

The system must automatically move a PendingEntity or PendingStudio reservation to **Expired** no later than its start time if the required approvals are not complete. Once the start time is reached, the reservation cannot remain pending and cannot later be approved. The mechanism used to perform this transition is an implementation detail; its externally visible outcome is not.

The system must automatically move a Validated reservation to **Completed** once the current campus-local time reaches its end time. From that point onward, it is no longer a blocking reservation. The mechanism used to perform this transition is an implementation detail; the externally visible state change is required.

## Creating a reservation

A reservation request must include:
- studio;
- date;
- start time;
- end time;
- requester;
- activity/usage type, which must be one of **Enregistrement**, **Captation d'image**, **Montage** or **Mixage audio**;
- purpose or short reason when required by the final product scope.

A request may also include additional mobile equipment requests, such as cameras, microphones or lighting kits. The reservation stores these as requested equipment references for the Studios/equipment workflow. Detailed allocation, quantities, assignment, maintenance handling and fulfilment rules remain in Issue #7 and are not defined here.

Before submission, the requester must digitally accept or sign the responsibility charter and applicable internal rules. A request cannot be submitted without that acceptance/signature step.

Before submission, the system must enforce the applicable weekly booking-hour quota for the requester's role/profile for the relevant campus-local week. For this V1 policy, **PendingEntity**, **PendingStudio**, **Validated** and **Completed** reservations count toward the applicable quota; **Rejected**, **Cancelled** and **Expired** reservations do not count. A Completed reservation continues to count for the campus-local week in which the reserved usage occurred. The quota check is part of the same atomic/serialized submission operation as the availability check and pending pre-lock. The quota check and reservation creation must also be serialized or protected by an equivalent invariant keyed by the relevant quota subject and campus-local week. This quota protection must apply across different studios and non-overlapping intervals, so concurrent submissions by the same quota subject cannot collectively exceed the applicable weekly quota. The exact quota configuration, including the final numeric quota for a Student profile, remains to be confirmed; the Cahier's “2h/week for a Student profile” is an example and must not be hard-coded until confirmed.

Quota accounting must use the reservation's resulting lifecycle state and the applicable campus-local week. Any transition between a counting state and a non-counting state must update the reservation state and its quota accounting atomically or equivalently serialized under the same quota-subject/week invariant. This includes refusal, cancellation and automatic expiry. Transitions between counting states, including the PendingEntity -> PendingStudio -> Validated -> Completed progression, must preserve quota accounting, with Completed usage remaining attributed to the campus-local week in which the reserved usage occurred. Concurrent submissions or state transitions must not observe stale quota usage, collectively exceed the applicable quota, or bypass quota accounting by racing a counting-state change.

The requested start time must be strictly in the future when submission begins. A request with a start time at or before the current campus-local time must be rejected.

The future-time check, quota check, availability check and creation of the PendingEntity reservation must be one atomic operation or an equivalently serialized operation. The system must serialize competing submissions for the same studio and overlapping time range (or use an equivalent invariant-enforcing mechanism), while also enforcing the quota-subject/week invariant across all studios and intervals. It must check the conditions and create the pending reservation as one indivisible operation. A successful submission immediately blocks the interval for competing reservations.

If that operation proves that the request is not bookable—for example, because of a conflict, a quota violation, a start time that is no longer in the future, a closed studio, or an administrative/maintenance block—no reservation is created and the user is told which availability or quota condition prevented booking. If the checks pass but the operation cannot commit because of a network, timeout, backend or similar technical failure, the user receives a retryable system/network error instead. The system must not describe a technical failure as an availability failure.

A successful submission becomes **PendingEntity** and is shown as **En attente entité**.

## Sequential approval and rejection

The reservation requires two sequential approval levels. A Level-1 approval is not final approval; it only advances the reservation to PendingStudio.

### Level 1 — entity/business approval

The appropriate entity/business approver validates first. The requester-to-approver mapping and the permissions for this level are defined by Issue #4 and are not duplicated or invented here.

When Level 1 succeeds, the state changes from **PendingEntity** to **PendingStudio**, and the user-visible status changes from **En attente entité** to **En attente studio**.

The Level-1 operation must be atomic or equivalently serialized and, at its commit point, must verify all of the following together:
- the reservation is still **PendingEntity**;
- the current campus-local time is strictly before the reservation start time;
- the Level-1 approval decision and the **PendingEntity -> PendingStudio** state transition can be committed together.

Automatic expiry of a PendingEntity reservation competes with Level-1 approval under the same state-transition serialization. If expiry commits first, or if the start time has been reached when the Level-1 operation is resolved, Level 1 must not succeed or advance the reservation; the operation must leave or observe the resulting state **Expired**. If Level 1 commits first while the start time is still in the future, the reservation may enter **PendingStudio**, after which the normal start-time expiry rule still applies if Level 2 has not completed. Only one of these transitions may win the race.

A Level-1 refusal must be atomic or equivalently serialized with Level-1 approval, cancellation and automatic expiry. At its commit point, it must verify that the reservation is still **PendingEntity** and commit **PendingEntity -> Rejected**, including the mandatory reason and the resulting quota-accounting change. If cancellation, expiry or another valid state transition has already won, the refusal must not overwrite that state and must not send a rejection notification. A successfully committed refusal triggers immediate requester notification. Notification technology remains outside Issue #5 and is handled under Issue #7.

### Level 2 — final Studios technical validation

Final technical validation belongs to the Studios responsible function. The Level-2 operation must be atomic or equivalently serialized with the state change to Validated.

During the same final approval operation, the Studios responsible function must revalidate all of the following:
- the requested start time is still in the future;
- the studio is currently open and available for the entire reserved interval;
- no other blocking reservation conflicts with the interval;
- no maintenance or administrative block prevents booking the interval;
- required centralized technical/control-room resources are available;
- required human technical resources, such as technicians or sound engineers, are available when applicable.

Requested mobile equipment references may be considered through the equipment/maintenance rules owned by Issue #7, but this specification does not define staffing algorithms, allocation algorithms or exact resource quantities.

Whenever final validation requires an exclusive shared technical resource, its availability check and claim/reservation must occur atomically or be equivalently serialized with the Level-2 state transition. This includes a centralized control room or régie, a technician, a sound engineer, or another exclusive technical resource defined later by Issue #7. The invariant must be keyed by the resource and overlapping reservation interval, and must prevent the same resource from being claimed for overlapping reservations even when those reservations use different studios. A failed claim because the resource is already required by an overlapping reservation is a Level-2 availability refusal, not a technical commit failure.

If every revalidated condition holds, the state changes from **PendingStudio** to **Validated**, and the user-visible status becomes **Validée**. If any availability or resource condition is disproven, the Level-2 refusal operation must be atomic or equivalently serialized with Level-2 approval, cancellation and automatic expiry. At its commit point, it must verify that the reservation is still **PendingStudio** and commit **PendingStudio -> Rejected**, including the mandatory reason and the resulting quota-accounting change. If cancellation, expiry or another valid state transition has already won, the refusal must not overwrite that state and must not send a rejection notification. A successfully committed refusal triggers immediate notification to the requester. A technical failure that prevents the operation from completing is not a refusal and must not be converted into Rejected.

Every user-visible state-changing reservation operation must use an operation identifier or an equivalent idempotency/reconciliation invariant. This includes submission, Level-1 approval, Level-1 refusal, Level-2 approval, Level-2 refusal and cancellation. If a response is lost after the server may have committed one of these operations, the client must reconcile the operation's outcome before retrying. It must not blindly retry, claim that the operation definitely failed, or present a technical failure as proof that the slot is unavailable. A retry or reconciliation of a committed refusal must not create a second state transition or duplicate the immediate requester notification; one committed refusal operation produces at most one such notification.

## Cancellation

A requester can cancel their own PendingEntity, PendingStudio or Validated reservation only before its start time. The cancellation operation must be atomic or equivalently serialized and, at its commit point, must verify all of the following together:
- the reservation is still in one of the cancellable source states: **PendingEntity**, **PendingStudio** or **Validated**;
- the current campus-local time is strictly before the reservation start time;
- if the source state is **Validated**, every exclusive shared technical-resource claim associated with the reservation is released or deactivated;
- the state transition to **Cancelled** is committed.

For a Validated reservation, cancellation must not complete with an active claim left behind. Once the transition commits, the Cancelled reservation must not block a centralized control room or régie, technician, sound engineer, or any other exclusive shared resource defined by Issue #7. Detailed resource models and release mechanisms remain owned by Issue #7.

Automatic expiry of PendingEntity or PendingStudio competes with cancellation under the same state-transition serialization. If expiry commits first, or if the start time has been reached when cancellation is resolved, cancellation must not succeed: a pending reservation must leave or remain in **Expired**, while a **Validated** reservation remains **Validated** and non-cancellable under these V1 rules. If cancellation commits first while the start time is still in the future, the reservation becomes **Cancelled** and any required Validated-resource claim release commits with it. Only one of cancellation, pending expiry, or the start-time cutoff outcome may win the race.

After the start time, a pending reservation is Expired and a Validated reservation cannot be cancelled under this V1 specification. Administrative cancellation rules and any stricter cancellation cutoff can be added later if the product scope requires them.

## Conflict handling

Two blocking reservations must never overlap for the same studio.

Reservation intervals use a start-inclusive, end-exclusive rule: a reservation ending at 10:00 does not conflict with one starting at 10:00, but any shared time within both intervals is a conflict.

For V1, **PendingEntity**, **PendingStudio** and **Validated** reservations block the same time slot. Rejected, Cancelled, Expired and Completed reservations do not block availability. PendingEntity and PendingStudio therefore continue to pre-lock the interval throughout both approval stages.

Conflict checking and the state change that creates or validates a reservation must be atomic or serialized. A check performed before the state change is not sufficient, because two concurrent operations could otherwise both observe a free slot and both succeed.

Creation or update of a maintenance or administrative block for a studio and affected time interval must participate in the same serialization or equivalent exclusion invariant as reservation submission and Level-2 final validation for that studio and interval. A maintenance/administrative block and a conflicting booking decision must not both successfully commit. This is a reservation-level consistency rule only; the detailed maintenance model and workflow remain outside Issue #5.

The studio conflict invariant and the shared-resource invariant are separate and cumulative: a reservation must have both a free studio interval and successful claims for every required exclusive shared resource. The same resource must never be allocated to overlapping reservations, including reservations for different studios.

Cancelled reservations have no active exclusive shared-resource claims. Releasing or deactivating those claims is part of the Validated -> Cancelled transition, so a Cancelled reservation cannot continue to block an overlapping reservation for another studio.

Availability failures and technical failures must be presented differently:
- if an atomic availability check proves that a request or final approval is no longer allowed, the operation fails with a clear availability message;
- if the atomic quota check proves that a new request would exceed the applicable quota, submission fails with a clear quota message;
- if the final technical validation proves that the studio, required resources or interval is no longer bookable, the reservation is refused with its mandatory reason and the requester is notified;
- if creation, an approval or refusal at either level, or cancellation cannot commit because of a network, timeout, backend or similar technical failure, the operation fails with a retryable system/network error;
- a slot-unavailable or quota message must never be used for a technical failure.

If the result of any user-visible state-changing operation is unknown after a timeout or network interruption, including a refusal or cancellation, the UI must show a temporary **Checking outcome** or **Outcome unknown** state. The system must reconcile the operation identifier and current reservation status before allowing a retry. Once reconciled, the UI must show the actual resulting state or allow a retry only when the original operation is confirmed not to have committed. A reconciled refusal must not send another immediate requester notification.

## Edge cases

The product must handle:
- a studio becoming unavailable after a user opened its page;
- two users submitting the same interval nearly simultaneously;
- concurrent submissions by the same quota subject for different studios in the same campus-local week;
- concurrent Level-2 validations for different studios requiring the same exclusive shared technical resource;
- a maintenance or administrative block racing reservation submission or Level-2 validation for the same studio and interval;
- a Level-1 approval reaching the start cutoff at the same time as automatic PendingEntity expiry;
- a submission without the required activity type;
- a submission without digital responsibility-charter acceptance/signature;
- a request that would exceed the applicable weekly role quota;
- a PendingEntity reservation reaching its start time without Level-1 approval and becoming Expired;
- a PendingStudio reservation reaching its start time without Level-2 approval and becoming Expired;
- a Validated reservation reaching its end time and becoming Completed;
- a reservation crossing outside studio opening hours;
- invalid or zero-length time ranges;
- the start time passing between validation and submission, Level-1 approval or Level-2 approval;
- cancellation immediately before the start time;
- cancellation racing the start-time cutoff or automatic pending expiry;
- a maintenance or administrative block added before final validation;
- required technical or human resources becoming unavailable before final validation;
- a refusal at either level without a reason, which must be rejected as invalid reviewer input;
- a Level-1 or Level-2 refusal racing cancellation or automatic expiry without overwriting the winning terminal state or sending a duplicate rejection notification;
- a lost response after the server may have committed creation, approval, refusal or cancellation;
- cancellation of a Validated reservation releasing its exclusive shared-resource claims;
- a reviewer attempting to validate a reservation whose interval is no longer bookable;
- unavailable network/offline state without pretending a reservation succeeded;
- an unknown inventory item or supporting space until the V1 bookable studio inventory is confirmed.

## UX states

The reservation flow must include:
- loading;
- empty availability;
- validation errors;
- missing activity type or responsibility-charter acceptance feedback;
- quota-exceeded feedback;
- **En attente entité**;
- **En attente studio**;
- **Validée**;
- rejection reason and notification feedback;
- conflict/no-longer-available feedback;
- expired or past-start reservation feedback;
- retryable system/network failure feedback that is distinct from slot-unavailable feedback;
- **Checking outcome** or **Outcome unknown** after an interrupted operation;
- permission denied;
- offline/network failure;
- success confirmation.

## Out of scope for this Issue

This Issue does not decide:
- exact requester-to-approver role mapping or global permissions, which belong to Issue #4;
- the definitive V1 studio inventory while the source discrepancy in Issue #8 remains unresolved;
- the final numeric weekly quota for Students or other roles;
- detailed mobile-equipment allocation, quantities, assignment, maintenance workflow or staffing algorithms, which belong to Issue #7;
- notification technology, which belongs to Issue #7;
- Flutter architecture;
- backend/database technology;
- QR implementation;
- payment or billing;
- recurring reservations.

Those decisions belong to their own Issues or ADRs.

## Acceptance

This specification becomes authoritative once the related Pull Request is reviewed and merged.
