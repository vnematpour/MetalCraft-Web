# Decision Log

## 1. Purpose

This document records Product Owner decisions approved for the MetalCraft web edition during discovery.

A decision recorded here is authoritative within its stated scope. It does not resolve broader questions that are explicitly left open in `OPEN_QUESTIONS.md`.

## 2. Decision Records

### DEC-001 — Project order recurrence

- **Status:** Approved
- **Decision:** A project may receive a new order after its previous order reaches `Completed`.
- **Related questions:** OQ-001

### DEC-002 — Active project-order definition

- **Status:** Approved
- **Decision:** Every order status other than `Completed` and `Cancelled` counts as active for the one-active-order-per-project constraint.
- **Related questions:** OQ-002, OQ-003
- **Clarification:** Draft, payment-pending, and paused are examples of state categories. This decision does not assert that every named status is part of the final status catalogue.

### DEC-003 — Project membership changes

- **Status:** Approved
- **Decision:** Customers may be added to or removed from a project after orders have been created, including while the project has an active order.
- **Decision:** Membership history must be retained. Ending and later restarting membership creates a new membership period.
- **Decision:** A customer may belong to multiple projects simultaneously.
- **Decision:** The same customer cannot have duplicate current membership in the same project.
- **Related questions:** OQ-004, OQ-005, OQ-006, OQ-046
- **Open boundary:** The exact behavior of a project with no current members remains unresolved. The historical customer snapshot/current-membership interpretation for existing project orders also remains unresolved under OQ-046.

### DEC-004 — Customer and project archival

- **Status:** Approved, with residual deletion question open
- **Decision:** Customers and projects support archival and reactivation while preserving their records, identifiers, and history.
- **Decision:** A customer currently belonging to an active project cannot be archived until that membership is resolved.
- **Decision:** A project may be archived only when it has no active orders. An archived project cannot receive new orders; its memberships and history remain, and it may be reactivated.
- **Decision:** Archiving a project does not archive its customers.
- **Decision:** A customer with orders, operational history, or historical project membership must not be physically deleted.
- **Decision:** A customer may be physically deleted only when it has no orders, operational history, historical membership, or unresolved dependencies.
- **Decision:** A project may be physically deleted only when it has no orders and no operational history; current memberships must be removed atomically with the project.
- **Related questions:** OQ-006, OQ-007
- **Open boundary:** Whether historical membership records alone prevent physical project deletion remains unresolved.

### DEC-005 — Customer and project identifiers

- **Status:** Approved
- **Decision:** Customer and project codes are generated automatically, unique, immutable, and never reused, including after archival or deletion.
- **Decision:** Customer and project names do not need to be unique.
- **Related questions:** OQ-007
- **Open boundary:** Exact code format and numbering sequence remain unresolved.

### DEC-006 — Project-owned orders

- **Status:** Approved
- **Decision:** A project-owned order belongs to the project as a whole. The requesting member customer is not recorded as the owner/requester of that order.
- **Decision:** Customers may have both direct customer-owned orders and project-owned orders.
- **Decision:** Customer-owned and project-owned orders follow the same approved lifecycle rules.
- **Decision:** The one-active-order constraint applies to project-owned orders only. No equivalent one-active-order-per-customer constraint has been approved.
- **Related questions:** OQ-001, OQ-002, OQ-003, OQ-045, OQ-046

### DEC-007 — Production start boundary

- **Status:** Approved
- **Decision:** Production starts when the first production operation is recorded as started.
- **Clarification:** Order creation and production planning alone do not count as production start.
- **Related questions:** OQ-009, OQ-010

### DEC-008 — Production pause and resume

- **Status:** Approved
- **Decision:** A production pause requires a reason.
- **Decision:** Every pause and resume event must be retained with its timestamp and responsible user.
- **Clarification:** A paused order remains active for the project-order constraint.
- **Related questions:** OQ-002, OQ-003, OQ-009, OQ-028

### DEC-009 — Order cancellation

- **Status:** Approved, with residual consequences open
- **Decision:** Normal cancellation is allowed only before the first production operation starts.
- **Related questions:** OQ-009, OQ-010, OQ-011
- **Open boundary:** Effects on payments, production records, materials/inventory, numbering, and documents remain unresolved.

### DEC-010 — Reactivating a cancelled project order

- **Status:** Approved, with residual lifecycle question open
- **Decision:** A cancelled project-owned order may be reactivated only if the project has no other active order.
- **Related questions:** OQ-002, OQ-009, OQ-011
- **Open boundary:** Reopening a completed order remains undecided. The complete order status catalogue and transition matrix also remain open.

### DEC-011 — Order completion authorization

- **Status:** Approved
- **Decision:** Completing an order requires the System Administrator role or a user with the dedicated completion permission.
- **Decision:** Completion is allowed only after all required production operations have completed.
- **Related questions:** OQ-009, OQ-026, OQ-027

### DEC-012 — Editing orders after production starts

- **Status:** Approved
- **Decision:** After production starts, non-operational order information may be edited subject to the user's permissions.
- **Decision:** Technical specifications may be changed only with a dedicated permission and only if the related production operation has not started.
- **Decision:** Technical changes must retain the previous value, new value, reason, timestamp, and responsible user.
- **Related questions:** OQ-009, OQ-027, OQ-028

### DEC-013 — Project-order creation authorization

- **Status:** Approved
- **Decision:** Creating a project-owned order requires the System Administrator role or a dedicated project-order creation permission.
- **Related questions:** OQ-026, OQ-027

### DEC-014 — Project-order visibility

- **Status:** Approved
- **Decision:** Project-owned orders are available to internal users who have the relevant permissions.
- **Decision:** Direct customer access and a customer portal are outside the currently approved scope.
- **Related questions:** OQ-026, OQ-027, OQ-029

### DEC-015 — Permission model

- **Status:** Approved, with catalogue open
- **Decision:** Fine-grained permissions are required. Create, edit, archive, and delete permissions may be granted independently.
- **Decision:** The System Administrator can grant granular permissions.
- **Related questions:** OQ-026, OQ-027
- **Open boundary:** The complete role catalogue and permission catalogue remain unresolved.

### DEC-016 — Traceability for selected business changes

- **Status:** Approved, with audit scope open
- **Decision:** Preserve traceability for project membership changes, production pause/resume events, technical order changes, customer/project changes requiring history, and project notes.
- **Decision:** Project notes are internal-only; access is permission-controlled and changes are traceable.
- **Related questions:** OQ-028
- **Open boundary:** The complete audit-trail scope, retention requirements, and event details remain unresolved.

### DEC-017 — Customer and project information changes

- **Status:** Approved
- **Decision:** Customer information may be edited. Changes affecting historical records must not silently rewrite historical information; relevant previous/new values, timestamp, and responsible user must be traceable.
- **Related questions:** OQ-027, OQ-028
- **Open boundary:** Exact customer/project fields and validation rules remain unresolved.

## 3. Questions Still Open or Partially Answered

The following areas must not be treated as fully resolved by the decisions above:

- OQ-006: Exact behavior when a project has no current member customers.
- OQ-007: Whether historical membership records alone prevent physical project deletion.
- OQ-008: Complete order types and their distinct workflows.
- OQ-009: Complete order status catalogue and all allowed transitions.
- OQ-010: Cancellation effects on payments, production records, materials/inventory, numbering, and documents.
- OQ-011: Reopening a completed order.
- OQ-026 and OQ-027: Complete role and permission catalogues.
- OQ-028: Complete audit scope and retention requirements.
- OQ-045: Whether a project-owned order can be created when the project has no member customers.
- OQ-046: Whether existing project orders retain a historical snapshot of participating customers or reflect current membership.
- Exact customer/project fields, validation rules, and code format.
- Commercial rules, payment relationships, production routing, station behavior, materials, waste, reports, documents, backup/restore, hosting, workload, database, migration, browser support, integrations, and deployment.

## 4. Change Control

- Do not silently alter an approved decision.
- If a Product Owner decision changes, add a new decision record that supersedes the previous record; retain the historical record.
- Update `BUSINESS_RULES.md`, `DOMAIN_MODEL.md`, `FEATURE_INVENTORY.md`, and `OPEN_QUESTIONS.md` when a decision materially changes their content.
- Architecture and implementation must follow approved decisions only. Unresolved topics require an explicitly approved temporary assumption if work cannot avoid them.
