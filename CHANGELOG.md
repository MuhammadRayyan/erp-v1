# Changelog

## 2026-02-23 — Theme Created & Deferred

### UI Theme
- Created erp_v1_core/public/css/erp_v1_theme.css (Manager.io blue/white theme)
- Theme tested and working but disabled in hooks.py — deferred to post-Phase 1f
- Default ERPNext theme restored for all development phases


## 2026-02-15 — Phase 1a & 1b Complete

### Phase 1a: Accounting Foundation
- Company configured: Test Technical Services LLC (AED, UAE)
- UAE Chart of Accounts verified and operational
- Bank & Cash accounts hierarchy confirmed
- VAT 5% tax template configured (VAT 5% - TTSL account)
- Additional tax templates available: UAE VAT Zero, Exempt, Excise 50%, Excise 100%
- Test journal entry: 10,000 AED capital contribution submitted successfully

### Phase 1b: Sales Cycle
- Customer management: Al Noor Trading LLC created
- Complete sales workflow operational:
  - Quotation (QTN-TTSL-2026-00001)
  - Sales Order (SAL-ORD-2026-00001) with delivery date
  - Delivery Note (DN-TTSL-2026-00001)
  - Sales Invoice (ACC-SINV-2026-00001) with 5% VAT
  - Payment Entry (5,250 AED received)
  - Credit Note (return invoice) tested
- All documents correctly calculate UAE VAT at 5%
- Payment linking verified (invoice marked as Paid)

### Documentation
- README.md updated with complete project structure section
- Docker location documented (D:\Projects\erp-v1-docker)
- Git structure clarified (custom app only, not full project)

## 2026-02-06 — Phase 0 Complete
- Initialized Frappe Bench with Frappe v16.5.0
- Downloaded and installed ERPNext v16
- Created site: erp-v1.localhost
- Created custom app: erp_v1_core
- Connected to GitHub (public repo)
- Development environment fully operational

