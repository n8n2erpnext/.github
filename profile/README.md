<p align="center">
  <img src="https://raw.githubusercontent.com/n8n2erpnext/.github/main/profile/assets/erpnext-core.svg" width="84" alt="n8n2erpnext ERPNext n8n community nodes logo">
</p>

<h1 align="center">n8n2erpnext</h1>

<p align="center">
  Open-source engineering for ERPNext/Frappe operations, Vietnam localization, evidence-governed BI, and self-hosted automation.
  <br>
  Built from real operational workflows, with explicit boundaries, auditable evidence, and production-minded deployment patterns.
</p>

<p align="center">
  <a href="https://github.com/n8n2erpnext/lightbi"><img alt="LightBI" src="https://img.shields.io/badge/LightBI-public%20beta-2563EB"></a>
  <a href="https://github.com/n8n2erpnext/erpnext-vietnam"><img alt="ERPNext Vietnam" src="https://img.shields.io/badge/ERPNext%20Vietnam-0.1.0--rc1-2E7D5F"></a>
  <a href="https://github.com/n8n2erpnext/erpnext2vi"><img alt="Vietnamese localization" src="https://img.shields.io/badge/Frappe%20%2F%20ERPNext-Vietnamese%20localization-C62828"></a>
  <a href="https://www.npmjs.com/search?q=n8n-nodes-erpnext"><img alt="npm n8n ERPNext nodes" src="https://img.shields.io/badge/npm-n8n--nodes--erpnext-2490EF"></a>
  <img alt="ERPNext and Frappe" src="https://img.shields.io/badge/ERPNext%20%2F%20Frappe-self--hosted-171717">
</p>

## What Lives Here

`n8n2erpnext` has grown beyond its original n8n node packages. The organization now hosts a small open-source stack around ERPNext/Frappe operations, Vietnamese localization, business analysis, edge integration, and zero-trust infrastructure.

### Flagship and Vietnam-focused projects

| Project | Purpose | Current posture |
| --- | --- | --- |
| [**LightBI**](https://github.com/n8n2erpnext/lightbi) | Evidence-governed business analysis for spreadsheets, online sheets, and databases, with local-first execution and explicit source/evidence boundaries. | Public beta |
| [**ERPNext Vietnam**](https://github.com/n8n2erpnext/erpnext-vietnam) | Vietnam localization and compliance layer for Frappe/ERPNext v16: TT99 accounting references, VAT, PIT, BHXH/BHYT/BHTN, statutory reporting, and provider-neutral e-invoice workflows. | `0.1.0-rc1` engineering RC |
| [**ERPNext / Frappe Vietnamese Localization**](https://github.com/n8n2erpnext/erpnext2vi) | Semantic Vietnamese translation catalogs for Frappe, ERPNext, HRMS, CRM, Insights, and additional business-domain apps. | Semantic v3 QA |

### Infrastructure and edge projects

| Project | Purpose |
| --- | --- |
| [**Zero Trust Syncd**](https://github.com/n8n2erpnext/erpnext-netbird-bridge) | Governed ERPNext identity-to-NetBird synchronization with preview, reconciliation, drift detection, audit history, and an operator console. |
| [**Edge Attendance Gateway**](https://github.com/n8n2erpnext/edge-attendance-gateway) | Offline-first attendance capture at the branch/site edge with cryptographic request verification, durable local queueing, and ERPNext synchronization. |

### ERPNext automation packages for n8n

The original project family remains active as modular community nodes rather than a single monolithic connector:

| Repository | Scope |
| --- | --- |
| [n8n-nodes-frappe-core](https://github.com/n8n2erpnext/n8n-nodes-frappe-core) | Shared Frappe API foundation and generic integration behavior |
| [n8n-nodes-erpnext-accounting](https://github.com/n8n2erpnext/n8n-nodes-erpnext-accounting) | Accounting documents and ledger-facing workflows |
| [n8n-nodes-erpnext-buying](https://github.com/n8n2erpnext/n8n-nodes-erpnext-buying) | Supplier and procurement workflows |
| [n8n-nodes-erpnext-selling](https://github.com/n8n2erpnext/n8n-nodes-erpnext-selling) | Customer and sales workflows |
| [n8n-nodes-erpnext-stock](https://github.com/n8n2erpnext/n8n-nodes-erpnext-stock) | Warehouse, inventory, batch, serial, and stock-ledger workflows |
| [n8n-nodes-erpnext-hrms](https://github.com/n8n2erpnext/n8n-nodes-erpnext-hrms) | Employee, attendance, leave, and HRMS workflows |
| [n8n-nodes-erpnext-crm](https://github.com/n8n2erpnext/n8n-nodes-erpnext-crm) | Public CRM package scaffold; not presented as a released module |

## Engineering Principles

The repositories are different products, but they share the same engineering posture:

- **Preserve upstream boundaries.** Prefer Frappe/ERPNext extension points, companion apps, adapters, and explicit contracts over unnecessary core forks.
- **Fail closed when evidence is weak.** LightBI does not speculate joins or silently substitute measures when source identity, grain, or analytical authorization is uncertain.
- **Version rules that change over time.** ERPNext Vietnam keeps legal rules effective-dated and auditable, with preview/apply separation and certification gates for external transports.
- **Make risky infrastructure changes reviewable.** Zero Trust Syncd defaults to preview/dry-run patterns, explicit guardrails, drift detection, snapshots, and documented failure behavior.
- **Keep the edge resilient.** Edge Attendance Gateway validates requests locally, queues durably when upstream ERPNext is unavailable, and synchronizes only across an explicit trust boundary.
- **Treat localization as domain work.** `erpnext2vi` translates for business meaning and runtime context across ERP, HRMS, CRM, Healthcare, Hospitality, and other Frappe applications instead of word-for-word substitution.
- **Self-host where it improves control.** Local-first execution, operator diagnostics, upgradeability, privacy boundaries, and recoverability are treated as product features rather than deployment afterthoughts.

## ERPNext Automation For n8n

The n8n package family is the organization's original ERP automation layer. It provides open-source community nodes for ERPNext and Frappe and helps teams automate real operations such as accounting, procurement, sales, warehouse movement, inventory validation, and cross-module document integrity.

The project is designed for:

- ERPNext automation with n8n.
- Frappe REST API integrations.
- Self-hosted ERP operations.
- SME and mid-market business workflow automation.
- Retail, distribution, stock, buying, selling, accounting, HRMS, and ERP operations.
- AI-searchable ERPNext integration examples with production-style README documentation.

## Connected ERP Ecosystem

The core business modules are now validated as one connected ERP system:

```text
Buying -> Stock -> Selling -> Accounting
```

The Stock validation suite includes two live-tested enterprise workflows that prove cross-module behavior end to end:

```text
Standard Product Lifecycle
Supplier -> Purchase Receipt -> Inventory Increase -> Purchase Invoice
-> Customer Sale -> Sales Invoice update_stock=1 -> Inventory Decrease
-> Bin, Stock Ledger Entry, and Accounting verification
```

```text
Exception / After-Sales Lifecycle
Sale -> Return Credit Note -> Warranty Warehouse -> Defective Warehouse
-> Repair / Virtual Workshop -> Disposal
-> Bin, Stock Ledger Entry, and Accounting verification
```

These workflows prove:

- Inventory can safely enter the business.
- Inventory can safely move through warehouses.
- Inventory can safely leave through sales.
- Return, warranty, defect, repair, and disposal flows are covered.
- Internal trade can move stock and accounting across two ERPNext companies.
- Purchase Invoice locks protect Purchase Receipt cancellation.
- Sales Invoice locks protect Delivery Note cancellation.
- Linked inter-company Purchase Invoice records protect Sales Invoice cancellation.
- Valid inter-company reversal works when the receiving Purchase Invoice is cancelled before the source Sales Invoice.
- Non-stock fee and rebate invoices do not create Stock Ledger Entry rows.
- Public webhook responses are allowlisted and checked for credential leaks.

Detailed coverage notes, tested ERPNext pain points, and live document references are maintained in [`docs/ecosystem-coverage.md`](../docs/ecosystem-coverage.md).

## Module Packages

| Module | Package | npm | Scope | Status |
| --- | --- | --- | --- | --- |
| <img src="https://raw.githubusercontent.com/n8n2erpnext/.github/main/profile/assets/erpnext-core.svg" width="32" alt="Frappe Core node"> Core | [`n8n-nodes-frappe-core`](https://github.com/n8n2erpnext/n8n-nodes-frappe-core) | Foundation package | Generic Frappe API, shared credential pattern, core API behavior | In progress |
| <img src="https://raw.githubusercontent.com/n8n2erpnext/.github/main/profile/assets/erpnext-hrms.svg" width="32" alt="ERPNext HRMS node"> HRMS | [`n8n-nodes-erpnext-hrms`](https://github.com/n8n2erpnext/n8n-nodes-erpnext-hrms) | [`npm`](https://www.npmjs.com/package/n8n-nodes-erpnext-hrms) | Employees, attendance, leave, payroll-adjacent workflows | Live |
| <img src="https://raw.githubusercontent.com/n8n2erpnext/.github/main/profile/assets/erpnext-accounting.svg" width="32" alt="ERPNext Accounting node"> Accounting | [`n8n-nodes-erpnext-accounting`](https://github.com/n8n2erpnext/n8n-nodes-erpnext-accounting) | [`npm`](https://www.npmjs.com/package/n8n-nodes-erpnext-accounting) | Sales Invoice, Purchase Invoice, Payment Entry, Journal Entry, GL Entry | Live |
| <img src="https://raw.githubusercontent.com/n8n2erpnext/.github/main/profile/assets/erpnext-buying.svg" width="32" alt="ERPNext Buying node"> Buying | [`n8n-nodes-erpnext-buying`](https://github.com/n8n2erpnext/n8n-nodes-erpnext-buying) | [`npm`](https://www.npmjs.com/package/n8n-nodes-erpnext-buying) | Supplier, RFQ, Supplier Quotation, Purchase Order, Purchase Receipt | Live |
| <img src="https://raw.githubusercontent.com/n8n2erpnext/.github/main/profile/assets/erpnext-selling.svg" width="32" alt="ERPNext Selling node"> Selling | [`n8n-nodes-erpnext-selling`](https://github.com/n8n2erpnext/n8n-nodes-erpnext-selling) | [`npm`](https://www.npmjs.com/package/n8n-nodes-erpnext-selling) | Customer, Lead, Opportunity, Quotation, Sales Order | Live |
| <img src="https://raw.githubusercontent.com/n8n2erpnext/.github/main/profile/assets/erpnext-stock.svg" width="32" alt="ERPNext Stock node"> Stock | [`n8n-nodes-erpnext-stock`](https://github.com/n8n2erpnext/n8n-nodes-erpnext-stock) | [`npm`](https://www.npmjs.com/package/n8n-nodes-erpnext-stock) | Item, Warehouse, Stock Entry, Delivery Note, Batch, Serial No, Bin, Stock Ledger Entry | Live |

## Live-Tested Coverage

The active ecosystem has been tested against ERPNext/Frappe v16 behavior on a self-hosted n8n runtime with API v1 and API v2 document workflows.

| Area | Verified behavior |
| --- | --- |
| ERPNext/Frappe | v16 behavior, Frappe REST API v1 and v2 |
| n8n | Self-hosted n8n community node runtime |
| Credentials | Shared ERPNext API credential with optional host header support |
| Buying | Supplier, Purchase Receipt, Purchase Invoice link, cancellation lock |
| Stock | Material Receipt, Material Transfer, Stock Reconciliation, Delivery Note, Batch, Serial No, Bin, Stock Ledger Entry |
| Selling | Customer, Quotation, Sales Order, Sales Invoice bridge, return credit note |
| Accounting | Sales Invoice, Purchase Invoice, Payment Entry, Journal Entry, GL Entry verification |
| Retail | Sale, exchange, return fee, warranty, defect, repair, disposal |
| FMCG/Fresh | Batch expiry, spoilage, damage, vendor claim warehouse, non-stock rebate/fee |
| Manufacturing basics | BOM, Work Order, WIP transfer, Manufacture Entry, Finished Goods sale |
| Multi-company | Internal Customer/Supplier mapping, allowed-company setup, internal sale/purchase, linked cancellation lock, valid reversal order |
| Negative cases | Over-issue, wrong warehouse, disabled item, missing warehouse, duplicate Batch/Serial |
| Security | Allowlisted webhook summaries, no API keys or secrets in responses |

## Why The n8n Node Family Exists

Many ERPNext integrations stop at generic HTTP requests. `n8n2erpnext` goes further by packaging repeatable, module-aware n8n nodes with:

- ERPNext-specific resources and operations.
- Frappe API v1 and v2 support.
- Submit and cancel helper behavior.
- Read-only ledger verification resources where safety matters.
- Importable workflow artifacts for real ERP validation.
- Production security notes for credentials, webhooks, and response design.
- Cross-module workflows that mirror actual business operations.

## Install From n8n

In n8n, install the package you need from the Community Nodes interface:

```text
n8n-nodes-erpnext-accounting
n8n-nodes-erpnext-buying
n8n-nodes-erpnext-selling
n8n-nodes-erpnext-stock
n8n-nodes-erpnext-hrms
```

For manual installation in a custom n8n nodes environment:

```bash
npm install n8n-nodes-erpnext-stock
npm install n8n-nodes-erpnext-accounting
npm install n8n-nodes-erpnext-buying
npm install n8n-nodes-erpnext-selling
```

## Credential Pattern

All module packages use the shared `ERPNext API` credential pattern:

```text
Site URL: https://erp.example.com
Site Host Header: erp.example.com
API Key: your ERPNext API key
API Secret: your ERPNext API secret
Ignore SSL Issues: false
```

For private VPS or container deployments, n8n can call ERPNext through an internal URL while still sending the public Frappe site host header:

```text
Site URL: http://erpnext.internal:8001
Site Host Header: erp.example.com
```

## Production Posture

The project favors operational safety over broad, unchecked API exposure:

- Use dedicated ERPNext API users.
- Scope roles to the exact DocTypes used by each workflow.
- Prefer explicit fields for public webhook responses.
- Treat Stock and Accounting workflows as high-impact operations.
- Deactivate temporary write-test workflows after verification.
- Do not expose generic Custom DocType or Frappe Method workflows publicly.
- Use VPN, reverse proxy auth, header auth, IP allowlists, or shared secrets for public webhooks.
- Reduce saved n8n execution data for workflows that process financial, inventory, payroll, or customer records.

## SEO Keywords

ERPNext n8n integration, Frappe n8n nodes, n8n community nodes ERPNext, ERPNext automation, Frappe automation, ERPNext Accounting n8n, ERPNext Buying n8n, ERPNext Selling n8n, ERPNext Stock n8n, ERPNext HRMS n8n, ERPNext API v2 n8n, Frappe REST API n8n, ERPNext workflow automation, n8n ERP integration, self-hosted ERPNext automation, warehouse automation ERPNext, stock ledger ERPNext n8n, purchase receipt ERPNext n8n, sales invoice ERPNext n8n, business lifecycle ERPNext automation.

## n8n Node Family Roadmap

Planned direction:

- Keep module READMEs concise and production-focused.
- Add focused workflow documentation for retail, manufacturing basics, FMCG/Fresh, multi-company, and coverage matrices.
- Keep V1 focused on maintenance, issue response, documentation fixes, and dependency/security updates.
- Build Core V2 only when enough real issues and custom ERPNext examples arrive.
- Position Core V2 as a Frappe/ERPNext Customization Bridge: Custom DocTypes, custom fields, dynamic metadata, child tables, method helpers, and schema translation for real ERPNext/Frappe customization pain points.
- Add additional modules only when they can follow the same live-tested standard.
- Expand automated tests for request construction, endpoint selection, and credential redaction.
- Keep the ecosystem modular so teams can install only the ERPNext domain they need.

## References

- [ERPNext](https://docs.frappe.io/erpnext)
- [Frappe Framework](https://docs.frappe.io/framework)
- [Frappe REST API](https://docs.frappe.io/framework/user/en/api/rest)
- [n8n Community Nodes](https://docs.n8n.io/integrations/community-nodes/)
- [n8n Creating Nodes](https://docs.n8n.io/integrations/creating-nodes/)

## License

Each package is released under its repository license. Core module packages are MIT unless noted otherwise in the package repository.
