# Current System Analysis

## 1. Purpose and Scope

This document describes the existing MetalCraft desktop application based on the inspected WinForms source archive and its included SQLite databases. It is a baseline for discovery and planning of the web edition.

It does **not** define the target web architecture. Existing implementation details must not automatically become requirements for the new system.

## 2. Existing Application

| Area | Observed implementation |
|---|---|
| Application type | Windows desktop application |
| UI framework | Windows Forms |
| Target framework | .NET 10 (`net10.0-windows`) |
| Runtime identifier | `win-x64` |
| Database | SQLite |
| Data access | `Microsoft.Data.Sqlite` 10.0.14 |
| Application version | 2.0.0 |
| Main project | `src/MetalCraftPro/MetalCraftPro.csproj` |
| Solution | `MetalCraftPro.sln` |

The source is organized into areas including `Core`, `Data`, `Models`, `UI`, and `DevTools`. The UI contains pages, forms, and controls. Data access is implemented through static repository classes and direct SQL rather than Entity Framework Core or a generic Unit of Work.

## 3. Existing Functional Areas

The inspected source indicates the following functional areas:

- Dashboard
- Customer management
- Project management and customer membership
- Order management, including production and service order types
- Order items
- Discounts, tax, and prepayment-related fields
- Payments and payment-related status handling
- Production stations and order production routes
- User records and login
- Theme and UI formatting
- Invoice/proforma printing
- Database backup and restore utilities
- SQL/database diagnostic tools
- Developer self-tests and snapshot-related utilities

This is a source-derived inventory, not a statement that every area is complete, actively used, or required in the web edition.

## 4. Database and Initialization Findings

Two SQLite database files were present in the inspected archive:

- Debug database: `src/MetalCraftPro/bin/Debug/net10.0-windows/win-x64/Data/MetalCraft.db`
- Release database: `src/MetalCraftPro/bin/Release/net10.0-windows/win-x64/Data/MetalCraft.db`

Both databases passed SQLite integrity checks. Their schemas were not identical even though both reported `SchemaVersion = 1`.

The Debug database contained structures not present in the Release database, including:

- `Projects`
- `ProjectCustomers`
- `Orders.ProjectId`
- `Customers.Phone2`
- `OrderItems.Color`
- `OrderItems.Brand`
- `Orders.ProformaNumber`
- `Orders.InvoiceNumber`

The Debug database is therefore the more feature-complete schema among the two files inspected. This does not prove that it is the authoritative production database. The intended canonical schema and migration history still need confirmation.

The database initialization code uses an application-base `Data/MetalCraft.db` path by default, supports a `METALCRAFT_DB` environment variable, and has a Local Application Data fallback when the default location is not writable.

The inspected `Schema.sql` did not appear to be a complete schema-definition script containing all table/index/view DDL. The initialization path executes the embedded SQL text. No reliable, ordered schema-migration mechanism was identified in the inspected source. The build script also does not explicitly establish a complete database packaging and migration process. A clean-install bootstrap and upgrade path therefore require further verification.

The restore-validation path also needs review: the inspected validation did not establish that all structures required by the more feature-complete Debug schema, including `Projects` and `ProjectCustomers`, are mandatory for a restored database.

## 5. Important Existing Implementation Behaviors

- A desktop named mutex is used to prevent multiple ordinary application instances. This is not a web multi-user concurrency strategy.
- Repositories are static classes using direct SQL.
- The project/customer relationship is many-to-many in the inspected Debug database.
- The order data model supports customer and project ownership, and the inspected Debug database includes a constraint enforcing exclusive ownership.
- Project deletion is blocked when the project has orders.
- The project model/repository does not appear to make meaningful use of the `Projects.IsActive` database field.
- No database-level or repository-level guarantee for one active order per project was found.
- Order creation currently sets the initial status to `InProduction` and initializes the first production station. If no route is available, the order can become `Completed` immediately.
- Payment-related status members and confirmation logic exist, but the inspected order creation flow and developer self-test expectations are inconsistent: the creation flow uses `InProduction`, while a self-test expects `PendingPayment`.
- The inspected order dialog passes a null payment value when saving.
- A SQL console is present in developer/database tools.
- The query logger retains SQL and parameter values in memory.

These observations should be treated as findings to validate, not as desired web behavior. In particular, the observed order-creation flow and self-test disagreement must be resolved with the product owner before either behavior is adopted as a web requirement.

## 6. Security and Operational Observations

The desktop source includes a default administrative account with a weak hard-coded password. The password storage implementation uses a single SHA-256 hash with salt. No role/permission model was identified in the inspected source.

These mechanisms must not be copied into a network-accessible web application. The web edition requires a deliberate authentication, password-hashing, authorization, secret-management, and audit strategy. The specific technologies and design are decisions for the target-system architecture phase.

The SQL console is a privileged diagnostic feature and must not be exposed in the web application without a separate security design and explicit authorization.

Because the query logger retains SQL and parameter values in memory, its treatment of potentially sensitive values should also be assessed before any equivalent logging capability is designed for the web edition.

## 7. Known Limitations of This Analysis

- The analysis is based on the inspected source archive and the two database files included in it.
- Database contents may be development or sample data; record counts and sample records are intentionally not treated as business requirements.
- The two database files have schema drift.
- The Debug database is more feature-complete among the inspected files, but has not been established as the canonical production schema.
- The production database, actual deployment topology, user roles, and operational procedures have not yet been confirmed.
- Some behaviors are inferred from inspected code paths and may require runtime verification.
- No web implementation decisions are implied by this document.

## 8. Recommended Use

Use this document as the current-state baseline. Confirm business-critical behavior with the product owner, verify operational findings where necessary, and record target-system decisions separately. Do not treat the legacy implementation or either included database file as the automatic specification for the web edition.
