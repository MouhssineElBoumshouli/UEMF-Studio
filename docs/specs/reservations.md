# Studios, availability and reservation rules

Status: Proposed for Issue #5

## Scope

This document defines the V1 behavior for:
- studio information;
- availability;
- reservation requests;
- approval/rejection;
- cancellations;
- conflicts and edge cases.

Role names and permissions are defined separately in Issue #4. This document refers to the person allowed to approve reservations as the **authorized reviewer**.

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

A studio that is unavailable for maintenance or administration must not appear bookable for the affected period.

## Availability

Availability is based on the campus-local date and time.

A time slot is bookable only when:
- the studio is open for that period;
- the current campus-local time is before the requested start time;
- the requested start time is before the end time;
- the slot does not overlap an existing active reservation;
- the studio is not blocked for maintenance or another administrative reason.

V1 should prevent double booking rather than trying to resolve it after submission.

## Reservation lifecycle

The V1 reservation states are:

1. **Requested** — submitted by a user and waiting for review.
2. **Approved** — accepted by an authorized reviewer.
3. **Rejected** — refused by an authorized reviewer.
4. **Cancelled** — cancelled before completion.
5. **Completed** — an approved reservation whose reserved period has ended.
6. **Expired** — a Requested reservation that reached its start time without being approved, rejected or cancelled.

Allowed transitions:
- Requested -> Approved
- Requested -> Rejected
- Requested -> Cancelled
- Requested -> Expired (automatically when the start time is reached while the request is still Requested)
- Approved -> Cancelled
- Approved -> Completed

Rejected, Cancelled, Expired and Completed are terminal states.

The system must automatically move a Requested reservation to **Expired** no later than its start time if it has not already reached another terminal state. Once the start time is reached, the reservation cannot remain pending and cannot later be approved. The mechanism used to perform this transition is an implementation detail; its externally visible outcome is not.

## Creating a reservation

A reservation request must include:
- studio;
- date;
- start time;
- end time;
- requester;
- purpose or short reason when required by the final product scope.

The requested start time must be strictly in the future when submission begins. A request with a start time at or before the current campus-local time must be rejected.

The future-time check, availability check and creation of the Requested reservation must be one atomic operation or an equivalently serialized operation. The system must serialize competing submissions for the same studio and time range (or use an equivalent invariant-enforcing mechanism), check for conflicts, and create the Requested reservation as one indivisible operation. If a conflict is found, the start time is no longer in the future, or the operation cannot commit, no reservation is created and the user is told that the slot is unavailable.

A successful submission becomes **Requested**.

## Approval and rejection

An authorized reviewer can:
- approve a Requested reservation;
- reject a Requested reservation;
- optionally provide a short reason when rejecting.

The exact role or roles allowed to review reservations come from Issue #4.

Approval is allowed only while the requested start time is still in the future. If the start time has been reached, approval must fail and the Requested reservation must be treated as **Expired** under the automatic-expiry rule.

The time check, final conflict check and Requested -> Approved transition must be one atomic operation or an equivalently serialized operation. A reservation must not become Approved if its start time has passed or if another active reservation overlaps it.

## Cancellation

A requester can cancel their own Requested reservation before its start time.

A requester can cancel their own Approved reservation before its start time.

The exact rules for administrative cancellation and any cancellation cutoff stricter than the start time can be added later if the product scope requires them.

## Conflict handling

Two active reservations must never overlap for the same studio.

Reservation intervals use a start-inclusive, end-exclusive rule: a reservation ending at 10:00 does not conflict with one starting at 10:00, but any shared time within both intervals is a conflict.

For V1, both **Requested** and **Approved** reservations block the same time slot. This keeps availability predictable and avoids multiple people competing for the same slot during approval.

Conflict checking and the state change that creates or approves a reservation must be atomic or serialized. A check performed before the state change is not sufficient, because two concurrent operations could otherwise both observe a free slot and both succeed.

If a conflict appears because data changed between viewing availability and submitting:
- the new request is rejected;
- the user is told that the slot is no longer available;
- the user can choose another slot.

## Edge cases

The product must handle:
- a studio becoming unavailable after a user opened its page;
- two users submitting the same slot nearly simultaneously;
- a Requested reservation reaching its start time without review and becoming Expired;
- a reservation crossing outside studio opening hours;
- invalid or zero-length time ranges;
- the start time passing between validation and submission or approval;
- cancellation immediately before the start time;
- a reviewer attempting to approve a reservation whose slot is no longer valid;
- a studio blocked for maintenance;
- unavailable network/offline state without pretending a reservation succeeded.

## UX states

The reservation flow must include:
- loading;
- empty availability;
- validation errors;
- conflict/no-longer-available feedback;
- expired or past-start reservation feedback;
- permission denied;
- offline/network failure;
- success confirmation.

## Out of scope for this Issue

This Issue does not decide:
- exact user role names or global permissions;
- Flutter architecture;
- backend/database technology;
- notification technology;
- QR implementation;
- payment or billing;
- recurring reservations.

Those decisions belong to their own Issues or ADRs.

## Acceptance

This specification becomes authoritative once the related Pull Request is reviewed and merged.
