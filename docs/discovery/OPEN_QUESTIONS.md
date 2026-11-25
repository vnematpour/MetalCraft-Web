# Open Questions and Pending Decisions

## 1. Purpose

This document is the authoritative discovery backlog for unresolved product, domain, operational, and technical questions for the MetalCraft web edition.

An unanswered question must remain open. AI agents and developers must not silently convert assumptions, legacy implementation details, or technical preferences into requirements or implementation decisions.

This backlog records questions and their current status. It does not itself approve answers. Approved decisions must be recorded through the project decision protocol.

## 2. Domain and Workflow Questions

| ID | Question | Why it matters | Status |
|---|---|---|---|
| OQ-001 | Can a project receive a new order after its previous order is `Completed`? | Defines project order lifecycle and recurrence rules | Open |
| OQ-002 | Which exact order statuses count as active for the intended limit of at most one active order per project? | Required to define and enforce the project-order constraint | Open |
| OQ-003 | Can a project have an order in a payment-pending, paused, draft, or other non-production state while another order is created? | Clarifies how the active-order rule applies across lifecycle states | Open |
| OQ-004 | Can customers be added to or removed from a project after orders have been created? | Affects membership history, existing orders, and access to project work | Open |
| OQ-005 | Can one customer belong to multiple projects at the same time? | Affects membership constraints and workflows | Open |
| OQ-006 | What happens to a project when all member customers are removed? | Defines project lifecycle and whether an empty project can remain usable | Open |
| OQ-007 | Can customers or projects be deleted, or should they only be archived/deactivated? | Affects referential integrity, historical records, and auditability | Open |
| OQ-008 | What are the complete order types, and how do their workflows differ? | Affects order behavior, validation, and UI | Open |
| OQ-009 | What are the valid order status transitions, including who or what may trigger each transition? | Required for reliable lifecycle implementation and authorization | Open |
| OQ-010 | What exactly happens when an order is cancelled? | May affect payments, production, materials, inventory, and documents | Open |
| OQ-011 | Can an order be reopened after completion or cancellation? If so, under what conditions? | Affects lifecycle rules and audit history | Open |
| OQ-045 | Can a project-owned order be created when the project has no member customers? | Defines project-order eligibility and validation | Open |
| OQ-046 | When project membership changes, should existing project orders retain a historical snapshot of the participating customers, or reflect the current membership? | Determines how historical project work and documents are interpreted | Open |

## 3. Commercial and Payment Questions

| ID | Question | Why it matters | Status |
|---|---|---|---|
| OQ-012 | What is the exact order pricing model, including quantity, unit price, line totals, and order totals? | Defines commercial calculations and data requirements | Open |
| OQ-013 | Are discounts applied per item, per order, or both? | Defines calculation rules and presentation | Open |
| OQ-014 | How is tax calculated and configured? | Defines financial calculations, rounding, and historical accuracy | Open |
| OQ-015 | What is the prepayment/deposit workflow, and how does it affect order processing? | Clarifies payment expectations and lifecycle dependencies | Open |
| OQ-016 | Can an order have multiple partial payments? | Determines payment cardinality and balance calculations | Open |
| OQ-017 | Are refunds, payment reversals, and multiple payment methods required? | Defines accounting behavior and transaction history | Open |
| OQ-018 | Can payment status change independently of production status? | Determines whether payment and production lifecycle states must be modeled separately | Open |
| OQ-019 | How are proforma and invoice numbers generated, and must they be unique, sequential, or legally sequential? | Affects numbering rules, concurrency, and document integrity | Open |
| OQ-047 | Are payments associated with an order, an invoice, or another financial record? Can one payment cover multiple records? | Determines payment relationships and allocation behavior | Open |

## 4. Production Questions

| ID | Question | Why it matters | Status |
|---|---|---|---|
| OQ-020 | Are production routes fixed, configurable, or selected per order? | Defines routing model and configuration requirements | Open |
| OQ-021 | Can an order skip, repeat, or return to a production station? | Defines production state transitions and traceability | Open |
| OQ-022 | Who can update production progress, and which updates require authorization? | Affects authorization and operational accountability | Open |
| OQ-023 | Is production progress tracked by order, item, quantity, or batch? | Affects data model, workflow, and reporting | Open |
| OQ-024 | What should happen when a route has no stations? | Existing source behavior may complete an order immediately; intended business behavior is not confirmed | Open |
| OQ-025 | Are materials, waste, and production performance required in the web edition? | Determines whether legacy manufacturing data and workflows must be included or expanded | Open |

## 5. Users, Access, and Audit Questions

| ID | Question | Why it matters | Status |
|---|---|---|---|
| OQ-026 | What user roles are required? | No clear legacy role/permission model was identified | Open |
| OQ-027 | Which actions require fine-grained permissions, and at what level should access be restricted? | Defines authorization boundaries | Open |
| OQ-028 | Is an audit trail required for changes to customers, projects, orders, payments, and production? Which events and details must be retained? | Affects traceability, accountability, and storage requirements | Open |
| OQ-029 | Are users internal to one organization, or is multi-tenancy required? | Affects security and data isolation | Open |
| OQ-030 | Are password reset, account lockout, session expiry, and multi-factor authentication (MFA) required? | Defines authentication and account-security requirements | Open |

## 6. Documents, Reporting, and Operations

| ID | Question | Why it matters | Status |
|---|---|---|---|
| OQ-031 | Which documents must the web edition generate (for example, proforma, invoice, work order, and delivery note)? | Defines document templates and output scope | Open |
| OQ-032 | Must documents be printable in Persian with right-to-left layout? Are there required fonts, paper sizes, or print formats? | Affects typography, layout, PDF generation, and printing | Open |
| OQ-033 | Are file attachments required? If so, which business records can own them and what file types or limits apply? | Affects storage, validation, and access control | Open |
| OQ-034 | Which dashboards and reports are essential, and which data filters are required? | Defines reporting scope and query requirements | Open |
| OQ-035 | Are notifications required? If so, which events, recipients, and delivery channels are needed? | Affects background processing and integrations | Open |
| OQ-036 | What backup frequency, retention period, restore procedure, and operational ownership are required? | Defines operational architecture and recovery expectations | Open |

## 7. Technical and Deployment Decisions

| ID | Question | Why it matters | Status |
|---|---|---|---|
| OQ-037 | Which database engine will the web edition use? | Core persistence and deployment decision | Open |
| OQ-038 | Where will the application be hosted (LAN server, cloud, or both)? | Affects networking, security, and operations | Open |
| OQ-039 | What are the expected concurrent users, peak workload, and data volume? | Informs capacity and concurrency design | Open |
| OQ-040 | Is migration of legacy SQLite data required? If so, which database file or source is authoritative? | The inspected Debug and Release schemas differ | Open |
| OQ-041 | What is the canonical source of schema definitions and migrations for the web edition? | Legacy initialization and migration process is unclear | Open |
| OQ-042 | Which browsers, screen sizes, and devices must be supported? | Defines UI compatibility and testing scope | Open |
| OQ-043 | Are external integrations required? If so, which systems and data flows are involved? | Affects API, security, and operational design | Open |
| OQ-044 | What environments and deployment process are required, including configuration, release, rollback, and updates? | Defines delivery and operational strategy | Open |
| OQ-048 | What measurable service and recovery targets apply, including availability, response time, peak throughput, Recovery Time Objective (RTO), and Recovery Point Objective (RPO)? | Provides measurable non-functional requirements for architecture and QA | Open |

## 8. Legacy Findings Requiring Validation

The following are observations from the inspected legacy source and database files. They are not automatically approved web-edition requirements or decisions. Validate them with the Product Owner or against actual operational usage before relying on them.

- The inspected Debug and Release SQLite files have different schemas despite both reporting `SchemaVersion = 1`.
- The Debug database appears more feature-complete, but it is not confirmed as the production source of truth.
- The order creation flow sets `InProduction`, while a self-test expects `PendingPayment`.
- The order dialog passes null for payment when saving.
- An order can become `Completed` immediately when no production route exists.
- The project `IsActive` database field is not consistently represented in the inspected model/repository.
- Project deletion is blocked by the legacy repository if any order exists.
- No persistence-level rule for one active order per project was identified.
- The legacy login implementation uses a password-storage approach that must not be copied into the web edition without a new security design.

These findings should be linked to the relevant discovery documents or source evidence when they are investigated. A legacy behavior must not be treated as the intended business rule merely because it is implemented.

## 9. Decision Protocol

For each question:

1. The Product Owner records the business answer, explicitly marks the question out of scope, or defers it with a reason.
2. The System Architect evaluates the approved answer's data, security, workflow, and operational impact.
3. The approved decision is recorded in `DECISION_LOG.md` and in any relevant task artifacts.
4. The Developer implements only approved behavior and documents any explicitly approved temporary assumption.
5. QA verifies the accepted rule and relevant edge cases.
6. The Product Owner reviews the result and records acceptance before the task is considered complete.

Until a question is resolved, implementation must either avoid depending on it or use an explicitly documented temporary assumption approved for that task. A temporary assumption is not a permanent product decision.

## 10. Status Vocabulary

Use these statuses consistently:

- **Open** — no approved answer has been recorded.
- **Answered** — the Product Owner has provided an answer, but the decision may still require architectural assessment or formal recording.
- **Approved** — the decision has been accepted by the Product Owner and recorded in `DECISION_LOG.md`.
- **Deferred** — the decision is intentionally postponed; include the reason and the condition or milestone for revisiting it.
- **Out of Scope** — the Product Owner has explicitly excluded the capability or question from the web edition.

Do not change a question's status based only on an AI recommendation, developer assumption, or legacy implementation detail. Preserve question IDs when editing existing entries. Assign new IDs only to genuinely new questions, and update related documents when a decision changes their content.
