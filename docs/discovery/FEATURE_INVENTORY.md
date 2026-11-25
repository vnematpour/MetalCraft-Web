# Feature Inventory

## 1. Purpose

This inventory records functional areas identified in the inspected MetalCraft WinForms source. It supports discovery, scope definition, and migration planning for the web edition.

The inventory separates:

- **Legacy evidence** — what was identified in the existing desktop source or its database.
- **Web-edition status** — whether the capability has been accepted, proposed, excluded, or remains open for the web edition.

A feature's presence in the desktop application does not, by itself, make it a web-edition requirement. Absence of evidence in the inspected source also does not prove that the business has no such requirement.

## 2. Feature Catalogue

| ID | Functional area | Evidence in existing source | Web-edition status |
|---|---|---|---|
| F-001 | Dashboard | A dashboard page exists in the desktop application. | Open — web scope and required metrics are not defined. |
| F-002 | Customer management | Customer page, model, and repository exist. | Confirmed — core business capability; detailed workflows and fields remain open. |
| F-003 | Customer contact details | Customer data includes a primary phone and a secondary phone in the inspected Debug database schema. The inspected Release database schema differs. | Open — confirm the canonical fields and validation rules. |
| F-004 | Project management | Project page/repository and project tables exist in the inspected Debug database. | Confirmed — core business capability; lifecycle and detailed workflows remain open. |
| F-005 | Project membership | A `ProjectCustomers` relationship exists in the inspected Debug database. | Confirmed — a project can group multiple customers. Open — whether one customer may belong to multiple projects is not confirmed. |
| F-006 | Customer-owned orders | The order model and repository support orders owned by a customer. | Confirmed — a customer may own an order directly. |
| F-007 | Project-owned orders | The order model and repository support orders owned by a project. | Confirmed — a project may own an order as a whole; the order is not assigned to one of its member customers. |
| F-008 | Order items | Order-item model and data-access code exist. | Open — confirm whether order items and commercial amounts are in web scope; detailed fields and pricing rules also remain open. |
| F-009 | Production orders | Production-related order type/flow is present in the inspected desktop source. | Open — confirm the web scope and the required lifecycle. |
| F-010 | Service orders | Service-related order type/flow is present in the inspected desktop source. | Open — confirm the web scope and the required lifecycle. |
| F-011 | Discounts and tax | Related data fields and calculation logic exist in the inspected source. | Open — confirm applicable rules, calculation order, rounding, and document presentation. |
| F-012 | Prepayment | Prepayment-related fields and payment concepts exist in the inspected source. | Open — confirm meaning, timing, and relationship to orders and payments. |
| F-013 | Payments | Payment-related data and status logic exist. The complete payment lifecycle and cardinality are not established. | Open — define payment states, partial/advance payments, reversals, and order/invoice relationships. |
| F-014 | Production stations | Station data and a station repository exist. | Open — confirm station management, permissions, and operational workflows. |
| F-015 | Production routes | Order-station/route data exists in the inspected source. | Open — confirm route rules, configurability, progression, and handling of orders with no route. |
| F-016 | User login | User records and a desktop login flow exist. The legacy password-storage approach is not suitable to carry forward as-is. | Open — design web authentication and session management using an appropriate security approach. |
| F-017 | Roles and permissions | No clear role/permission model was identified in the inspected source. | Open — determine whether roles and fine-grained permissions are required. |
| F-018 | Invoice/proforma printing | An A4 print-preview implementation exists in the desktop application. | Open — decide web document formats, templates, numbering, and print/download behavior. |
| F-019 | Theme and UI preferences | Desktop theme support exists. | Open — optional web preference; confirm whether it is required. |
| F-020 | Database backup/restore | Desktop database backup/restore tools exist. | Open — define web-environment backup, restore, retention, and operational ownership. |
| F-021 | SQL console | A developer/database SQL tool exists in the desktop application. | Open — no web requirement is approved; do not expose or port without explicit product and security approval. |
| F-022 | Diagnostics and self-tests | Developer diagnostics and self-test code exist. | Open — assess which diagnostics and tests are needed for web operations; redesign only where required. |
| F-023 | Audit history | A durable business audit trail was not established by the inspected source. | Open — investigate audit requirements, events, retention, and access. |
| F-024 | Attachments | Attachment capability was not established by the inspected source. | Open — investigate whether files must be attached to customers, projects, or orders. |
| F-025 | Notifications | Notification capability was not established by the inspected source. | Open — investigate required events, channels, recipients, and delivery expectations. |
| F-026 | Multi-tenancy | Multi-tenant behavior was not established by the inspected source. | Open — do not assume tenant isolation or a multi-tenant deployment model. |

### Interpretation notes

1. The Debug and Release databases are not structurally identical, despite both reporting `SchemaVersion = 1`. The Debug database contains more of the project-related structures, but it has not been confirmed as the canonical production database.
2. The existence of `ProjectCustomers` confirms that the inspected Debug schema represents project membership. It does not, on its own, settle all membership rules, including whether a customer may belong to more than one project.
3. The order ownership invariant is that an order belongs to exactly one owner: either a customer or a project, but not both and not neither.
4. “Confirmed” in this catalogue means the capability or business rule has been confirmed for discovery purposes. It does not mean that its complete workflow, user interface, data model, or technical implementation has been approved.
5. “Open” means a product-owner decision or further requirements discovery is still needed. It must not be silently converted into an implementation assumption.

## 3. Domain-Critical Capabilities

The following are high-priority discovery areas based on the confirmed business needs and the legacy functionality. Items whose status is Open remain unapproved until the Product Owner makes a decision.

1. Managing customers.
2. Grouping multiple customers into a project.
3. Creating orders owned either by a customer or by a project, while enforcing exclusive ownership.
4. Confirming whether order items and commercial amounts are in web scope, and defining their rules.
5. Tracking production through stations/routes, if confirmed as part of the web scope.
6. Defining payment behavior and order lifecycle states, after resolving the existing desktop inconsistencies.

These priorities identify important discovery areas; they do not authorize unresolved workflows or technical design decisions.

## 4. Features That Must Not Be Ported Blindly

- Desktop single-instance mutex behavior.
- Desktop-local database path assumptions.
- Weak default credentials or the legacy SHA-256 password scheme.
- The developer SQL console.
- Desktop-specific PrintPreview and UI code.
- Existing order status transitions where source behavior and tests disagree.
- Database initialization assumptions that do not provide a verified migration path.
- Desktop backup/restore implementation without redesigning it for the web deployment environment.
- Developer diagnostics or self-test utilities that expose privileged operations or are unsuitable for production use.

## 5. Suggested Discovery Priorities

### Priority 1 — Domain and workflow

- Customer lifecycle.
- Project lifecycle and membership rules.
- Customer-owned versus project-owned order creation.
- Order lifecycle, cancellation, and project-order recurrence rules.
- Order items, discounts, and tax calculations.
- Payment and production interaction.

### Priority 2 — Operational behavior

- Production station configuration.
- Production route assignment and progression.
- Invoice/proforma numbering and document output.
- Backup, restore, retention, and deployment responsibilities.

### Priority 3 — Cross-cutting capabilities

- Authentication and authorization.
- Audit history.
- Reporting.
- Attachments and notifications.
- User preferences.
- Diagnostics and operational monitoring.

## 6. Status Vocabulary

Use the following labels when updating this inventory:

- **Observed** — present in the inspected legacy source; no web-edition commitment is implied.
- **Confirmed** — explicitly accepted as a web-edition capability or business requirement.
- **Proposed** — suggested for consideration but not yet accepted.
- **Open** — requires a product-owner decision or further investigation.
- **Excluded** — explicitly out of scope for the web edition.
- **Implemented** — delivered in the web edition and accepted through the project workflow.

The catalogue's status descriptions may include a short explanation after the status label. Do not mark a feature **Confirmed** solely because it exists in WinForms. Do not mark a feature **Implemented** until the required QA and product-owner acceptance steps are complete.
