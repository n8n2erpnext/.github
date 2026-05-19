# n8n2erpnext Ecosystem Coverage

This document records tested cross-module ERPNext workflows and practical ERP pain points covered by the `n8n2erpnext` node ecosystem.

Keep exact module implementation notes in each package `SESSION_LOG.md`. Keep cross-module decisions in `ECOSYSTEM_LOG.md`. Use this file as the human-readable coverage map.

## Current Position

The ecosystem is no longer a set of isolated community nodes. Buying, Selling, Stock, and Accounting have been tested together through live ERPNext documents, inventory ledger movement, and accounting ledger verification.

Live test environment:

- ERPNext/Frappe v16 behavior.
- Self-hosted n8n runtime.
- ERPNext LXD test site using traceable demo data.
- API v1 read workflows and API v2 document workflows.

Early public adoption signals:

- HRMS: 429 npm downloads.
- Accounting: 648 npm downloads.
- Buying: 219 npm downloads.
- Stock and newer modules: not enough public download data yet.
- No public issues reported in the first five days after the first HRMS release.

These are early usage signals, not proof of product-market fit.

## Covered Business Workflows

### Standard Product Lifecycle

Flow:

```text
Supplier
-> Purchase
-> Inventory Receipt
-> Warehouse Storage
-> Customer Sale
-> Inventory Reduction
-> Accounting Completion
```

Verified behavior:

- Purchase Receipt increases inventory.
- Purchase Invoice links Buying to Accounting.
- Sales Invoice with `update_stock = 1` decreases inventory.
- Bin quantities match expected warehouse balances.
- Stock Ledger Entry rows are created for inventory-impacting documents.
- GL Entry rows are created for accounting-impacting documents.
- Linked accounting documents prevent unsafe stock document cancellation.

### Retail And After-Sales Lifecycle

Flow:

```text
Sale
-> Customer Return / Credit Note
-> Warranty Warehouse
-> Defective Warehouse
-> Repair / Virtual Workshop
-> Disposal
```

Verified behavior:

- Sales Invoice reduces sellable inventory.
- Return credit note increases return/warranty inventory.
- Warranty and defective movement are tracked by Stock Entry.
- Disposal reduces inventory.
- Non-stock return fee/rebate invoices do not create Stock Ledger Entry rows.

### Buying To Stock To Accounting Locks

Verified behavior:

- Purchase Receipt submit increases inventory.
- Purchase Invoice linked to Purchase Receipt creates accounting impact.
- ERPNext blocks unsafe Purchase Receipt cancellation after linked invoicing.
- Document `docstatus`, Bin, Stock Ledger Entry, and GL Entry are verified after submit/cancel attempts.

### Selling To Stock To Accounting Locks

Verified behavior:

- Delivery Note and Sales Invoice paths reduce inventory.
- ERPNext blocks unsafe Delivery Note cancellation when linked Sales Invoice exists.
- Sales Invoice with `update_stock = 1` is verified directly through Bin, Stock Ledger Entry, and GL Entry.

## Stock-Specific Coverage

Stock is tested more deeply than Buying and Selling because submitted stock documents mutate inventory balances and valuation ledgers.

Covered resources:

- Item Group
- Warehouse
- Item
- Stock Entry
- Stock Reconciliation
- Delivery Note
- Batch
- Serial No
- Bin
- Stock Ledger Entry
- UOM
- UOM Conversion Detail
- Price List
- Item Price
- Material Request

Verified Stock flows:

- Material Receipt increases stock.
- Material Transfer moves stock between source and target warehouses.
- Stock Reconciliation adjusts quantity and valuation.
- Delivery Note reduces stock.
- Batch item receipt and issue.
- Serial item receipt and issue.
- Disabled item negative case.
- Wrong warehouse negative case.
- Missing warehouse negative case.
- Over-issue negative case.
- Duplicate Batch and Serial negative cases.

## Manufacturing Basics

The project intentionally covers basic operational manufacturing, not enterprise MRP.

Verified behavior:

- BOM maps raw materials to finished goods.
- Work Order can be created and moved through basic lifecycle behavior.
- WIP transfer moves raw material out of raw/storage warehouse.
- Manufacture Stock Entry moves finished goods into Finished Goods warehouse.
- Finished goods can continue into Sales Invoice `update_stock = 1`.
- Bin and Stock Ledger Entry rows confirm the movement.

Intentionally excluded:

- Capacity planning.
- Workstation scheduling.
- Multi-level routing optimization.
- Forecasting.
- Machine utilization analytics.
- Advanced subcontract costing.

## FMCG And Fresh Basics

The project covers common FMCG/Fresh inventory pain points without simulating large retail-chain optimization.

Verified behavior:

- Batch item with expiry date.
- Batch receipt into inventory.
- Batch sale/issue.
- Remaining batch stock verification.
- Spoilage/damage Material Issue.
- Vendor claim warehouse movement.
- Claim closure / return-to-supplier style stock correction.
- Non-stock fee/rebate invoice with no Stock Ledger Entry.

Intentionally excluded:

- Chain-wide stock balancing algorithms.
- Demand planning.
- Dynamic pricing.
- Shelf-space optimization.
- Waste prediction.
- Vendor rebate settlement engines.

## Multi-Company And Inter-Company Coverage

Live companies:

- `Thái Duy Digital` (`TDD`)
- `Thái Duy Store` (`TDS`)

ERPNext-generated setup verified for `Thái Duy Store`:

- Receivable account.
- Payable account.
- Income account.
- COGS/expense account.
- Cash account.
- Inventory account.
- Stock Received But Not Billed account.
- Stock Adjustment account.
- Cost Center.
- Default warehouses.
- Perpetual inventory.

Internal party mapping verified:

- Internal Customer and Supplier records for both directions.
- `is_internal_customer`.
- `is_internal_supplier`.
- `represents_company`.
- `Allowed To Transact With` child rows.

Important pain point discovered:

- ERPNext blocks internal transactions unless the party is explicitly allowed to transact with the document company.
- Internal Customer/Supplier flags alone are not enough.

### Inter-Company Flow: TDD To TDS

Documents:

- Stock Entry `MAT-STE-2026-00066`: Material Receipt into `Stores - TDD`, qty `10`.
- Sales Invoice `ACC-SINV-2026-00029`: `Thái Duy Digital` sells internally to `Thái Duy Store`, qty `3`, `update_stock = 1`.
- Purchase Invoice `ACC-PINV-2026-00005`: `Thái Duy Store` receives internally from `Thái Duy Digital`, qty `3`, `update_stock = 1`, linked to `ACC-SINV-2026-00029`.

Verified result:

- `Stores - TDD`: `7`
- `Stores - TDS`: `3`
- Stock Ledger Entry rows: `+10`, `-3`, `+3`
- GL Entry rows posted separately under TDD and TDS accounts.
- Attempting to cancel linked Sales Invoice was blocked by ERPNext.
- DB remained unchanged after the blocked cancellation attempt.

### Inter-Company Flow: TDS To TDD

Documents:

- Sales Invoice `ACC-SINV-2026-00030`: `Thái Duy Store` sells internally to `Thái Duy Digital`, qty `1`, `update_stock = 1`.
- Purchase Invoice `ACC-PINV-2026-00006`: `Thái Duy Digital` receives internally from `Thái Duy Store`, qty `1`, `update_stock = 1`, linked to `ACC-SINV-2026-00030`.

Verified result:

- `Stores - TDD`: `8`
- `Stores - TDS`: `2`
- Stock Ledger Entry rows correctly show `TDS -1` and `TDD +1`.
- GL Entry rows posted separately under TDD and TDS accounts.
- Attempting to cancel linked Sales Invoice was blocked by ERPNext.
- DB remained unchanged after the blocked cancellation attempt.

### Valid Inter-Company Reversal

Documents:

- Sales Invoice `ACC-SINV-2026-00031`
- Purchase Invoice `ACC-PINV-2026-00007`

Test:

1. Submit internal sale from `Thái Duy Digital` to `Thái Duy Store`.
2. Submit linked internal purchase in `Thái Duy Store`.
3. Cancel receiving Purchase Invoice first.
4. Cancel source Sales Invoice second.

Verified result:

- Both documents reached `docstatus = 2`.
- Bin quantities returned to baseline:
  - `Stores - TDD`: `8`
  - `Stores - TDS`: `2`
- Stock Ledger Entry reverse rows were created.
- GL Entry reverse rows were created.

Operational conclusion:

- Unsafe source-document cancellation is blocked while linked receiving documents exist.
- Proper reversal order works: cancel receiving Purchase Invoice first, then cancel source Sales Invoice.
- This reflects real ERPNext behavior instead of a synthetic API-only success path.

## Core V2 Direction

Core should not become a rushed checklist module. The practical V2 direction is to solve ERPNext/Frappe customization pain points.

V1 closure:

- HRMS, Accounting, Buying, Selling, and Stock are considered closed for broad speculative feature expansion.
- Future V1 work should be maintenance, issue response, documentation fixes, and security/dependency updates.
- New features should be driven by real GitHub issues, real custom ERPNext examples, or repeated integration pain points.

Core V2 positioning:

- Frappe/ERPNext Customization Bridge.
- Custom DocType Adapter.
- Schema translation layer for ERPNext/Frappe custom reality.

Core V2 should focus on:

- Custom DocTypes.
- Custom fields.
- Dynamic metadata discovery.
- Schema translation.
- Converting custom ERPNext documents into shapes that domain nodes can understand.
- Child table helpers.
- Safer custom method execution patterns.
- Submit/cancel/amend helpers shared across packages.
- Schema snapshots for workflow debugging.

Domain nodes should stay focused on stable business surfaces. Core should become the advanced customization bridge so the ecosystem does not duplicate custom-doc translation logic across every package.

Do not build Core V2 as a generic HTTP wrapper. Its value is understanding and translating custom ERPNext/Frappe schema safely.
