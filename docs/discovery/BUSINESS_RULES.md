# Business Rules

## 1. Purpose

This document records business rules that are explicitly confirmed for MetalCraft and separates them from behaviors merely observed in the legacy WinForms implementation.

Each rule has an identifier and a status. Only rules marked **Confirmed** should be treated as binding requirements for implementation.

## 2. Confirmed Rules

### BR-001 — Customer-Owned Orders

A customer may own an order directly.

**Status:** Confirmed

### BR-002 — Project Membership

A project may contain multiple customers.

**Status:** Confirmed

### BR-003 — Project-Owned Orders

An order may be owned by a project.

**Status:** Confirmed

### BR-004 — Project Order Ownership

A project-owned order belongs to the project as a whole. It does not belong to an individual member customer.

**Status:** Confirmed

### BR-005 — Exclusive Order Ownership

Every order must have exactly one owner:
- One customer, or
- One project.

An order cannot belong to both a customer and a project, and cannot have no owner.

**Status:** Confirmed

### BR-006 — Historical Project Orders

A project may have multiple historical orders.

**Status:** Confirmed

### BR-007 — One Active Project Order

A project is intended to have no more than one active order at any given time. Cancelled orders are excluded from this intended limit.

**Status:** Confirmed at the business-intent level; exact status predicate is open.

## 3. Open Rules — Do Not Infer

### BR-008 — New Order After Completion

It is not yet decided whether a new order may be created for a project after its previous order has reached `Completed`.

**Status:** Open

### BR-009 — Definition of Active

The exact set of order statuses that count as active has not been approved.

**Status:** Open

### BR-010 — Cancelled Order Semantics

The effects of cancellation on payments, production records, inventory/materials, numbering, and audit history have not been fully specified.

**Status:** Open

### BR-011 — Project Membership Changes

Rules for adding/removing customers after orders exist are not defined.

**Status:** Open

### BR-012 — Customer and Project Deletion

The legacy repository blocks deletion of projects that have orders. The target web policy for deleting, archiving, or deactivating customers and projects requires approval.

**Status:** Open for the web edition

## 4. Legacy Behaviors That Are Not Approved Business Rules

The following behaviors were observed in the WinForms implementation but must not be treated as approved requirements without product-owner confirmation:

- New orders are currently initialized as `InProduction`.
- The first production station is initialized during order creation.
- An order may be set to `Completed` immediately if no production route exists.
- Payment-related status members and confirmation logic exist, while the order creation flow and self-test expectations disagree.
- The order dialog passes a null payment value when saving.
- Project deletion is blocked if any order exists.
- A project `IsActive` database field exists but is not consistently represented in the inspected project model/repository.

## 5. Rule Enforcement Expectations

For each confirmed rule, the implementation design should identify:
- Domain-level validation.
- Application/use-case validation.
- Database constraints or transaction/concurrency protections where appropriate.
- User-facing validation behavior.
- Automated tests covering valid and invalid cases.

For BR-005, persistence must prevent invalid owner combinations even if requests bypass the UI. The ownership invariant must also remain valid during concurrent or partially completed operations.

For BR-007, implementation must wait until BR-008 and BR-009 are clarified. A database uniqueness constraint should not be introduced using an unapproved interpretation of "active".

## 6. Change Control

Changes to confirmed rules must be:
1. Proposed by the Product Owner.
2. Assessed by the System Architect for data and workflow impact.
3. Implemented only after approval.
4. Covered by QA tests.
5. Recorded in the project's decision log and relevant task artifacts.
