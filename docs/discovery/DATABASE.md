# Database Discovery

## 1. Purpose and Scope

This document records database facts observed in the existing MetalCraft WinForms source archive and the two SQLite database files included in that archive. It does not select the database engine or schema design for the web edition.

Observed legacy implementation details are evidence about the current desktop application, not automatic requirements for the target system.

## 2. Legacy Database Technology

- Database engine: SQLite
- Data access package: `Microsoft.Data.Sqlite` 10.0.14
- Database filename: `MetalCraft.db`
- Default location: application base directory under `Data`
- Optional override: `METALCRAFT_DB`
- Fallback: Local Application Data when the default location is not writable

The desktop application is designed around a local database file. This deployment model and its concurrency characteristics must not be assumed suitable for multiple concurrent web users.

## 3. Database Files Inspected

| File | Approximate size | Integrity check |
|---|---:|---|
| Debug `MetalCraft.db` | 128 KiB | `ok` |
| Release `MetalCraft.db` | 104 KiB | `ok` |

Both files reported `SchemaVersion = 1`, but their structures differed. Therefore, the schema-version value alone does not reliably identify the actual schema represented by these two database files.

The Debug database included structures absent from Release, including:

- `Projects`
- `ProjectCustomers`
- `Orders.ProjectId`
- `Customers.Phone2`
- `OrderItems.Color`
- `OrderItems.Brand`
- `Orders.ProformaNumber`
- `Orders.InvoiceNumber`

The Debug database is more feature-complete among the inspected files, but it has not been proven to be the canonical production schema. Do not treat either file as the authoritative source for a migration until the product owner confirms which database represents the intended business data and the canonical schema has been established.

The successful SQLite integrity checks establish file-level database integrity at the time of inspection. They do not prove that the application schema is complete, that business rules are satisfied, or that the data is suitable for production use.

## 4. Observed Data Model Areas

The legacy database contains or references the following areas:

- Customers
- Projects
- Project/customer membership
- Orders
- Order items
- Stations
- Order stations/routes
- Users
- Payment-related information

The exact target schema, field nullability, indexes, constraints, and migration sequence must be designed and verified separately.

## 5. Constraints and Integrity Findings

- The inspected Debug schema enforces exclusive order ownership through a check constraint requiring either customer or project ownership, but not both.
- No database-level guarantee for at most one active order per project was found.
- Both inspected databases passed `PRAGMA integrity_check`.
- No foreign-key check issues were reported in the inspected databases.

The foreign-key check result does not, by itself, establish whether foreign-key enforcement is enabled on every application database connection at runtime. That behavior requires separate verification.

The exclusive order-ownership invariant is a confirmed business rule and should be protected in the web edition. The active-order constraint requires an approved definition of active statuses before implementation.

## 6. Initialization, Schema Versioning, and Migration Risks

The inspected source defines `SchemaVersion = 1`. No ordered migration mechanism was identified in the inspected source.

The inspected `Schema.sql` appeared to contain operational SQL/query fragments rather than a complete set of `CREATE TABLE`, `CREATE INDEX`, and `CREATE VIEW` definitions. The database initialization code executes the embedded SQL text.

The build script publishes the application but does not clearly establish a complete database packaging and migration workflow.

The restore-validation path also requires further verification. The inspected validation did not establish that all structures needed by the more feature-complete Debug schema, including `Projects` and `ProjectCustomers`, are mandatory in a restored database.

Before relying on the legacy database for migration or production, verify:

- Which database and schema are canonical.
- The source of truth for schema definitions.
- How a clean database is created.
- Whether schema upgrades are ordered, repeatable, and safe.
- The upgrade path from every supported existing schema.
- Whether restore validation rejects databases missing required tables, columns, indexes, or constraints.
- Backup and restore consistency, including backups taken during application activity.
- Whether foreign-key enforcement is enabled for every connection.
- Transaction boundaries for multi-step writes.
- Concurrent-write behavior and handling of SQLite lock/contention errors.

The above are verification requirements, not a recommendation to retain SQLite in the web edition.

## 7. Legacy Data Access Characteristics

- Static repository classes.
- Direct SQL through `Microsoft.Data.Sqlite`.
- No Entity Framework Core identified.
- No generic Unit of Work identified.
- A query logger retains SQL and parameter values in memory.

These are legacy implementation choices, not requirements for the web edition.

## 8. Security and Privacy Considerations

The legacy SQL console is a privileged diagnostic feature and must not be exposed to ordinary web users.

SQL and parameter logging may capture sensitive business or personal data. The web edition needs explicit decisions for log redaction, retention, access control, and operational diagnostics.

The legacy password hashing approach (single SHA-256 with salt) is not suitable as a design to copy into the web edition. A modern adaptive password-hashing mechanism should be selected as part of a separately approved authentication design.

## 9. Web Database Decision

**Status: Open**

The web edition database engine, hosting model, backup strategy, migration tool, and data access approach have not been selected in the confirmed project decisions available at this stage.

Do not assume SQLite, SQL Server, PostgreSQL, EF Core, or any other technology until the System Architect evaluates the requirements and records an approved decision.
