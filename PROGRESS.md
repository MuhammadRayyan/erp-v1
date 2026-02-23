# Progress Log

## Latest Update (2026-02-23)

### What We Completed
- Created Manager.io UI theme CSS (erp_v1_core/public/css/erp_v1_theme.css) — deferred to post-Phase 1f
- Registered fixtures in hooks.py for Property Setter and Custom Field
- Cleaned up Sales Invoice form — hidden ~50 irrelevant fields across all 5 pages
- Exported fixtures (property_setter.json, custom_field.json) to capture changes
- Committed and pushed all changes to GitHub

### Decisions Made
- UI theme deferred to post-Phase 1f (see DECISIONS.md)
- Quotation form layout deferred until after Purchase Cycle
- update_stock field override via Client Script deferred to theme phase

### Next Step (SINGLE MOST IMPORTANT)
- Begin Phase 1c: Purchase Cycle — start with Supplier management

### Blockers / Questions
- None

### Commands / References
- Export fixtures: `bench --site erp-v1.localhost export-fixtures`
- Clear cache: `bench --site erp-v1.localhost clear-cache`

---

## Update (2026-02-15)

### What We Completed
- **Constitutional Audit:** Verified DOCX alignment with repo implementation
- **Git Structure Clarification:** Confirmed custom app only (not full Docker project) is correct approach
- **README.md Update:** Added complete project structure documentation showing Docker location
- **Phase 1a Complete:** Accounting Foundation fully configured
  - Company: Test Technical Services LLC with UAE settings (AED, VAT, TRN-ready)
  - UAE Chart of Accounts verified (Assets, Liabilities, Equity, Revenue, Expenses)
  - Bank & Cash accounts structure confirmed
  - VAT 5% tax template verified (Account Head: VAT 5% - TTSL)
  - Test journal entry created and submitted successfully
- **Phase 1b (Sales Cycle) Complete:** Sales workflow is functional end-to-end.
- **Phase 1b (UI Theme + Layout) Pending:** Manager.io-style UI theme and form layout   organization still in progress.
  - Customer created: Al Noor Trading LLC
  - Complete workflow tested: Quotation → Sales Order → Delivery Note → Sales Invoice → Payment
  - Credit Note (return/refund) process verified
  - All documents correctly calculate 5% UAE VAT
  - Payment Entry links invoice to cash account

### Decisions Made
- Keep Docker files separate from Git repo (documented in README.md instead of moving to Git)
- Document Docker structure in README.md Project Structure section
- Use standard ERPNext sales workflow before applying Manager.io theme (functionality first, cosmetics later)
- Test customer: Al Noor Trading LLC (UAE-based company)
- Test service item for initial transactions

### Next Step (SINGLE MOST IMPORTANT)
- Complete Phase 1b: Form layout organization for Sales Invoice and Quotation (sections, tabs, collapsible areas via Customize Form → export as fixtures)

### Blockers / Questions
- None

### Commands / References
- Start server: `bench start` (from frappe-bench directory)
- Login: http://erp-v1.localhost:8000 | Administrator / admin
- Stop server: Ctrl+C
- Enter container: VS Code > Ctrl+Shift+P > "Dev Containers: Reopen in Container"
- Search in ERPNext: Ctrl+K
- Create documents: Search for doctype, click "+ New"

---

## Previous Update (2026-02-06)

### What We Completed
- Phase 0: Full environment setup
- Docker Dev Container running with Frappe Bench
- Frappe v16.5.0 initialized
- ERPNext v16 downloaded and installed
- Site created: erp-v1.localhost (admin password: admin)
- Custom app erp_v1_core created and installed on site
- GitHub repo connected: https://github.com/MuhammadRayyan/erp-v1
- VS Code extensions installed (Python, Jinja, GitLens)

### Decisions Made
- Project path: D:\Projects\erp-v1-docker (no spaces)
- Site name: erp-v1.localhost
- App license: GPL-3.0 (compatible with ERPNext)
- Git branch: version-16
- GitHub auth: HTTPS with Personal Access Token
- No GitHub workflow for now (added later in Phase 1f)

### Next Step (SINGLE MOST IMPORTANT)
- Begin Phase 1a: Accounting Foundation — Run the ERPNext Setup Wizard to create Test Technical Services LLC with UAE settings

### Blockers / Questions
- None

### Commands / References
- Start server: bench start (from frappe-bench directory)
- Login: http://erp-v1.localhost:8000 | Administrator / admin
- Stop server: Ctrl+C
- Enter container: VS Code > Ctrl+Shift+P > "Dev Containers: Reopen in Container"
