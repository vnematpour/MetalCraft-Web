# Feature Inventory

## 1. Purpose

This inventory records functional areas identified in the inspected
MetalCraft WinForms source and the business capabilities explicitly
confirmed for the web edition.

The inventory separates:

- **Legacy evidence** --- what was identified in the desktop source or database.
- **Web-edition status** --- whether a capability is confirmed, open, proposed, excluded, or implemented.

A feature's presence in the desktop application does not, by itself,
make it a web-edition requirement. Absence of evidence in the inspected
source also does not prove that the business has no such requirement.

## 2. Feature Catalogue

| ID | Functional area | Evidence in existing source | Web-edition status |
|---|---|---|---|
| F-001 | Dashboard | Dashboard page exists | Open |
| F-002 | Customer management | Customer page and repository exist | Confirmed |
| F-003 | Customer contact details | Customer fields include phone and secondary phone in Debug schema | Core capability; exact fields and validation open |
| F-004 | Project management | Project page/repository and project tables exist in Debug schema | Confirmed |
| F-005 | Project membership | `ProjectCustomers` relationship exists | Confirmed; membership history and lifecycle rules defined |
| F-006 | Customer-owned orders | Order model/repository supports customer ownership | Confirmed |
| F-007 | Project-owned orders | Order model/repository supports project ownership | Confirmed |
| F-008 | Order items | Order item model/data access exists | Open; scope and fields to confirm |
| F-009 | Production orders | Production order type/flow exists | Order lifecycle controls confirmed; complete workflow open |
| F-010 | Service orders | Service order type/flow exists | Open |
| F-011 | Discounts and tax | Related fields/logic exist | Open |
| F-012 | Prepayment | Related fields and payment concepts exist | Open |
| F-013 | Payments | Payment-related data and status logic exist | Open |
| F-014 | Production stations | Station repository and station data exist | Production lifecycle permissions/history confirmed; station model open |
| F-015 | Production routes | Order station/route data exists | Open; routing rules and configurability to confirm |
| F-016 | User login | User records and login flow exist | Authentication requirements open |
| F-017 | Roles and permissions | No clear role/permission model identified | Granular permission model confirmed; full catalogue open |
| F-018 | Invoice/proforma printing | A4 print-preview implementation exists | Open; web output format and templates to decide |
| F-019 | Theme and UI preferences | Desktop theme support exists | Open |
| F-020 | Database backup/restore | Desktop database tools exist | Open; web operational responsibility to design |
| F-021 | SQL console | Developer/database tool exists | Do not port without explicit security approval |
| F-022 | Diagnostics and self-tests | Developer tools and self-tests exist | Replace with appropriate web diagnostics/tests |
| F-023 | Audit/history | No durable business audit trail was established by the inspected source | Confirmed traceability for defined membership, production, technical-change, and project-note events; full audit scope open |
| F-024 | Attachments | Not established by inspected source | Open |
| F-025 | Notifications | Not established by inspected source | Open |
| F-026 | Multi-tenancy | Not established by inspected source | Not assumed; requirements open |
| F-027 | Project internal notes | Not established by inspected source | Confirmed; permission-controlled and traceable |
| F-028 | Customer/project archive and reactivation | Legacy project `IsActive` field exists but is inconsistently used | Confirmed lifecycle rules |
| F-029 | Permanent customer/project codes | Legacy identifiers exist; target generation rules not established | Confirmed automatic, permanent, unique codes; exact format open |

## 3. Confirmed Domain-Critical Capabilities

1.  Manage customers and their lifecycle.
2.  Manage projects and their lifecycle.
3.  Group multiple customers into projects, preserving membership
    history.
4.  Create customer-owned and project-owned orders.
5.  Enforce exclusive order ownership.
6.  Enforce no more than one active order per project.
7.  Track order production start, pause/resume, and completion with
    permission checks.
8.  Restrict cancellation to before the first production operation
    starts.
9.  Control post-start order edits and retain technical change history.
10. Manage internal project notes with permission-controlled access and
    traceability.
11. Generate permanent unique customer and project codes.

## 4. Features That Must Not Be Ported Blindly

- Desktop single-instance mutex behavior.
- Desktop-local database path assumptions.
- Weak default credentials or the legacy SHA-256 password scheme.
- The developer SQL console.
- Desktop-specific PrintPreview and UI code.
- Existing order status transitions where source behavior and tests
    disagree.
- Database initialization assumptions that do not provide a verified
    migration path.

## 5. Suggested Discovery Priorities

### Priority 1 --- Domain and workflow

- Complete order status catalogue and transitions.
- Order items, product/service definitions, and technical
    specifications.
- Pricing, discounts, tax, prepayment, and payment lifecycle.
- Production stations, routes, and progress tracking.
- Cancellation consequences and reopening completed orders.

### Priority 2 --- Operational behavior

- Invoice/proforma numbering and document output.
- Backup, restore, and deployment responsibilities.
- Required reports and dashboards.

### Priority 3 --- Cross-cutting capabilities

- Authentication and full permission catalogue.
- Audit-history scope beyond the events already confirmed.
- Attachments and notifications.
- User preferences and integrations.

## 6. Status Vocabulary

Use these labels when updating this inventory:

- **Observed** --- present in inspected legacy source.
- **Confirmed** --- explicitly accepted as a web-edition requirement.
- **Proposed** --- suggested but not yet accepted.
- **Open** --- requires a Product Owner decision.
- **Excluded** --- explicitly out of scope.
- **Implemented** --- delivered and accepted in the web edition.

Do not mark a feature Confirmed solely because it exists in WinForms.
