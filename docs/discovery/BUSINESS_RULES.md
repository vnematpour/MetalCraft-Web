# Business Rules

## 1. Purpose and Status

This document records business rules explicitly confirmed by the Product
Owner for the MetalCraft web edition. Legacy implementation details are
evidence only and do not automatically become target requirements.

Only rules marked **Confirmed** are binding requirements. These rules
describe business behavior; implementation details such as database
technology, table design, and application architecture remain separate
decisions.

## 2. Order Ownership and Project Membership

### BR-001 --- Customer-Owned Orders

A customer may own an order directly.

**Status:** Confirmed

### BR-002 --- Project Membership

A project may contain multiple customers. A customer may belong to zero,
one, or multiple projects at the same time.

**Status:** Confirmed

### BR-003 --- Project-Owned Orders

An order may be owned by a project.

**Status:** Confirmed

### BR-004 --- Project Order Ownership

A project-owned order belongs to the project as a whole, not to an
individual member customer. The system does not need to record which
member customer requested a project order.

**Status:** Confirmed

### BR-005 --- Exclusive Order Ownership

Every order must have exactly one owner:

- One customer, or
- One project.

An order cannot belong to both a customer and a project, and cannot have
no owner.

**Status:** Confirmed

### BR-006 --- Customer and Project Order Types

A customer who is a member of a project may have both:

- Orders owned directly by that customer.
- Orders owned by the project.

The ownership of a project order remains with the project.

**Status:** Confirmed

## 3. Project Order Lifecycle

### BR-007 --- One Active Project Order

A project may have no more than one active order at a time.

For this rule, every order status other than `Completed` and `Cancelled`
is active. A draft, payment-pending, or paused order is therefore active
if such a status is used by the product.

**Status:** Confirmed

### BR-008 --- New Order After Completion

A new order may be created for a project after its previous order
reaches `Completed`.

A project may therefore have multiple completed historical orders.

**Status:** Confirmed

### BR-009 --- Reactivating a Cancelled Project Order

A cancelled project order may be reactivated only when the project has
no other active order. Reactivation makes the order active again and
subjects it to BR-007.

**Status:** Confirmed

### BR-010 --- Cancellation Before Production

An order may be cancelled through the normal cancellation workflow only
before the first production operation has started.

Once the first production operation has started, normal cancellation is
not permitted.

**Status:** Confirmed

### BR-011 --- Meaning of Production Started

An order is considered to have entered production when the first
production operation is recorded as started.

Creating the order or planning/scheduling its production does not, by
itself, mean production has started.

**Status:** Confirmed

### BR-012 --- Pausing and Resuming Production

Production may be paused and later resumed. A paused order remains
active; pausing does not mean cancellation or completion.

Both pausing and resuming production require the System Administrator or
a user who has been granted the relevant permission.

**Status:** Confirmed

### BR-013 --- Mandatory Pause Reason and History

A reason is required whenever production is paused.

The system must retain the history of every pause and resume event,
including:

- Pause reason.
- Pause date and time.
- User who recorded the pause.
- Resume date and time.
- User who resumed production.

Multiple pause/resume cycles must be retained; a later cycle must not
overwrite an earlier one.

**Status:** Confirmed

### BR-014 --- Completing an Order

Only the System Administrator or a user with the specific completion
permission may mark an order as `Completed`.

An order may be completed only after all production operations required
for that order have been successfully completed.

**Status:** Confirmed

### BR-015 --- Order Lifecycle Rules Apply to Both Ownership Types

The confirmed lifecycle rules for cancellation, production pause/resume,
completion, and controlled changes apply to both customer-owned orders
and project-owned orders.

The one-active-order limit in BR-007 is specifically a project-level
rule. No equivalent one-active-order limit has been approved for
customer-owned orders.

**Status:** Confirmed

## 4. Order Changes and Traceability

### BR-016 --- Non-Operational Order Information

After production has started, non-operational information such as
descriptions may still be edited, subject to the applicable user
permissions.

**Status:** Confirmed

### BR-017 --- Technical Changes After Production Starts

After production has started, technical order specifications may be
changed only by a user with the special permission for that change, and
only if the production operation related to the affected specification
has not yet started.

A technical specification associated with an operation that has already
started cannot be changed through the normal order-editing workflow.

**Status:** Confirmed

### BR-018 --- Technical Change History

Every permitted technical specification change must retain:

- Previous value.
- New value.
- Reason for the change.
- Date and time.
- User who made the change.

**Status:** Confirmed

## 5. Project Order Permissions and Visibility

### BR-019 --- Creating Project Orders

Only the System Administrator and users granted the specific
project-order creation permission may create project-owned orders.

**Status:** Confirmed

### BR-020 --- Project Order Visibility

Project orders are visible only to internal system users who have the
relevant permissions. Project-member customers do not receive direct
access by default.

A customer portal and external customer access are outside the currently
approved scope.

**Status:** Confirmed

## 6. Customer Lifecycle

### BR-021 --- Customer Membership

A customer may belong to multiple projects simultaneously. The same
customer may be a member of a given project only once at a time.

**Status:** Confirmed

### BR-022 --- Rejoining a Project

A customer whose membership has ended may be added to the same project
again. The new membership is a new historical membership period; the
previous membership history must be retained.

**Status:** Confirmed

### BR-023 --- Membership Changes During Active Project Orders

Customers may be added to or removed from a project even while that
project has an active order. Membership changes must be recorded in
project history.

Removing a customer from a project must not delete or alter the
project's historical orders.

**Status:** Confirmed

### BR-024 --- Membership Transfer

A transfer from one project to another ends the membership in the source
project and creates a new membership in the destination project. The
previous membership history is retained.

A transfer is not mandatory: a customer may remain a member of both
projects if that is the intended business relationship.

**Status:** Confirmed

### BR-025 --- Customer Archiving

A customer with orders or operational history cannot be physically
deleted; the customer must be archived instead.

A customer with historical project membership cannot be physically
deleted, even if the customer has no orders.

A customer who is currently a member of an active project cannot be
archived until that project membership has been resolved.

An archived customer cannot be selected for new orders unless
reactivated.

**Status:** Confirmed

### BR-026 --- Customer Reactivation

An archived customer may be reactivated using the same record and the
same permanent customer code. Historical orders, memberships, and change
history remain intact.

**Status:** Confirmed

### BR-027 --- Physical Customer Deletion

A customer may be physically deleted only if the customer has no orders,
no operational history, no historical project membership, and no
unresolved dependencies.

**Status:** Confirmed

### BR-028 --- Customer Information Changes

Customer information may be edited. Changes to information used by
historical records must not silently rewrite or corrupt those historical
records.

Changes requiring traceability must retain the previous value, new
value, date/time, and responsible user.

**Status:** Confirmed

## 7. Project Lifecycle

### BR-029 --- Project Archiving

A project may be archived only when it has no active orders.

Archiving preserves the project's orders, customer membership history,
and other historical records. An archived project cannot receive new
orders.

**Status:** Confirmed

### BR-030 --- Project Reactivation

An archived project may be reactivated using the same record and
permanent project code. Its previous members and history remain
associated with it. New orders may be created after reactivation,
subject to the project-order lifecycle rules.

**Status:** Confirmed

### BR-031 --- Project Archiving Does Not Archive Customers

Archiving a project does not archive its member customers. Project
membership records are retained. When the project is reactivated, its
previous membership relationships remain in place.

**Status:** Confirmed

### BR-032 --- Physical Project Deletion

A project may be physically deleted only if it has no orders and no
operational history.

If the project has active membership rows but no order or operational
history, deletion of the project and those dependent active membership
rows must occur atomically: either all changes succeed or none do.

**Status:** Confirmed

## 8. Customer and Project Identifiers

### BR-033 --- Permanent Unique Codes

Every customer and every project receives a unique code generated
automatically by the system.

Customer codes and project codes are separate identifier
sequences/domains. Codes are permanent, cannot be changed by any user
(including the System Administrator), and must never be reused after
deletion or archiving.

**Status:** Confirmed

### BR-034 --- Non-Unique Display Names

Customer names and project names do not need to be unique. The permanent
code is the unique business identifier.

**Status:** Confirmed

## 9. Project Notes

### BR-035 --- Internal Project Notes

A project may have internal notes and descriptions for operational
context, agreements, and execution details.

These notes are for internal users only. Access to view and manage them
is permission-controlled. Changes to notes must be traceable.

**Status:** Confirmed

## 10. Rule Enforcement Expectations

For each confirmed rule, the implementation design must identify:

- Domain-level validation.
- Application/use-case validation.
- Database constraints or transaction/concurrency protections where appropriate.
- User-facing validation behavior.
- Automated tests covering valid and invalid cases.

In particular:

- BR-005 must be protected at the persistence boundary, not only in the UI.
- BR-007 must be protected against concurrent project-order creation and reactivation.
- Membership uniqueness must be protected against duplicate membership creation.
- Atomic project deletion must be implemented as one transaction.
- Permanent codes must be protected against modification and reuse.
- Permission checks must be enforced server-side.

## 11. Still Open

The rules above do not settle:

- The complete list of order statuses and all allowed status transitions.
- Payment, refund, inventory, material, and document consequences of cancellation.
- Whether reopening a completed order is allowed.
- The exact definition and scope of every technical specification and its related production operation.
- The full customer/project field catalogue and validation requirements.
- The exact code format and numbering sequence.
- Whether historical membership records alone prevent physical deletion of a project.
- Complete role definitions and the full permission catalogue.

These matters remain in `OPEN_QUESTIONS.md` until explicitly decided.
