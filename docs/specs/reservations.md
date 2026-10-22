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

Allowed transitions:
- Requested -> Approved
- Requested -> Rejected
- Requested -> Cancelled
- Approved -> Cancelled
- Approved -> Completed

Rejected, Cancelled and Completed are terminal states.

## Creating a reservation

A reservation request must include:
- studio;
- date;
- start time;
- end time;
- requester;
- purpose or short reason when required by the final product scope.

Before accepting the request, the system must re-check availability so two users cannot successfully reserve the same slot at the same time.

A successful submission becomes **Requested**.

## Approval and rejection

An authorized reviewer can:
- approve a Requested reservation;
- reject a Requested reservation;
- optionally provide a short reason when rejecting.

The exact role or roles allowed to review reservations come from Issue #4.

Approval must perform one final conflict check before the reservation becomes Approved.

## Cancellation

A requester can cancel their own Requested reservation.

A requester can cancel their own Approved reservation before its start time.

The exact rules for administrative cancellation and any cancellation cutoff stricter than the start time can be added later if the product scope requires them.

## Conflict handling

Two active reservations must never overlap for the same studio.

For V1, both **Requested** and **Approved** reservations block the same time slot. This keeps availability predictable and avoids multiple people competing for the same slot during approval.

If a conflict appears because data changed between viewing availability and submitting:
- the new request is rejected;
- the user is told that the slot is no longer available;
- the user can choose another slot.

## Edge cases

The product must handle:
- a studio becoming unavailable after a user opened its page;
- two users submitting the same slot nearly simultaneously;
- a reservation crossing outside studio opening hours;
- invalid or zero-length time ranges;
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
