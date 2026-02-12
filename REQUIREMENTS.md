# ERP-V1: Requirements & Architecture Document

**Custom ERP System — Manager.io-Style Experience Built on ERPNext/Frappe**

- **Version:** 1.0 | February 5, 2025
- **Author:** Muhammad Rayyan + Claude AI
- **Status:** FINALIZED

---

## 1. Purpose & Vision

ERP-V1 is a custom ERP solution built on top of ERPNext v16 (Frappe Framework) that provides a Manager.io-style user experience while leveraging ERPNext's superior technical capabilities. The software is designed to serve UAE-based businesses, starting with technical services, and will later be sold to businesses across various industries.

### 1.1 Core Goals

- **Familiarity:** Provide a clean, minimal interface that feels natural to Manager.io users, reducing the learning curve for transition.
- **Customizability:** Solve Manager.io's limitations around form layouts, custom fields, print formats, and theming by leveraging ERPNext's flexible architecture.
- **UAE-First:** Built-in support for UAE VAT (5%), AED currency, multi-currency transactions, and FTA e-invoicing (DCTCE/Peppol) mandatory from July 2026.
- **Sellable Product:** Designed from day one to be packaged and sold to other UAE businesses (technical services, automotive, electrical, trading, etc.).
- **Multi-Tenancy:** Manager Cloud-style multi-business capability where each business is completely isolated.
- **Upgrade-Safe:** All customizations live in a custom Frappe app. ERPNext core is never modified, ensuring smooth upgrades.

### 1.2 What This Is NOT

ERP-V1 is not a replica of Manager.io. It uses Manager.io as a UX reference while incorporating ERPNext features that are genuinely superior. Where ERPNext offers better workflows, reports, or capabilities, those are used instead. The goal is a comfortable transition for Manager.io users, not a carbon copy.

---

## 2. Current Situation & Pain Points

### 2.1 Current System

The business currently operates on Manager.io (desktop edition). While functional and familiar, several limitations prevent scaling and customization.

### 2.2 Pain Points with Manager.io

**Custom Fields Nightmare:**
Custom fields in Manager.io are appended as a long vertical list. There is no ability to organize them into sections, place them beside related fields, or control their layout. When multiple custom fields are needed (e.g., stamp areas, received-by fields, special conditions, reference numbers), the form becomes an unusable scrolling list.

**Print/PDF Instability:**
PDF and print layout customization in Manager.io is fragile. Changes frequently break, requiring repeated fixes. Theme and branding customization is not stable or easy to maintain.

**Theme Limitations:**
Updating the visual theme (colors, branding, layout) to match business needs is a hassle. There is no straightforward way to create a professional, branded experience.

**No API or Integration:**
Manager.io has no REST API, making integration with other systems (e-invoicing, CRM, banking APIs) impossible without workarounds.

### 2.3 Solution

Build on ERPNext/Frappe, which solves all of the above: flexible form customization with sections and columns, robust print format builder, full theming control, REST API, and a mature plugin/app architecture.

---

## 3. Technology Stack

| Component | Technology | Notes |
|-----------|-----------|-------|
| Backend Framework | Frappe Framework v16 | Python-based, ORM, REST API |
| ERP Base | ERPNext v16 | Full ERP modules, accounting, inventory |
| Database | MariaDB 10.6 | Via Docker, managed by Frappe |
| Cache/Queue | Redis | Session cache, background jobs |
| Web Server | Nginx | Reverse proxy in production |
| Containerization | Docker + Docker Compose | Development and deployment |
| Frontend | Frappe UI (Vue.js) + Custom CSS/JS | Manager.io-like theme overlay |
| Development OS | Windows 11 | Docker Desktop |
| IDE | VS Code | With Dev Containers extension |
| Version Control | Git + GitHub (private) | github.com/MuhammadRayyan/erp-v1 |
| ERPNext Version | v16 (latest) | Long-term support branch |

---

## 4. Architecture

### 4.1 Custom App Model (Upgrade-Safe)

All customizations are built as a Frappe custom app called `erp_v1_core`. This app sits alongside ERPNext and modifies behavior through Frappe's hooks, overrides, and client scripts. This ensures that when ERPNext releases updates, the core codebase can be upgraded without breaking our customizations.

> **RULE:** Never edit ERPNext core. All changes belong to the Core app and add-on apps.

**The Core App Contains:**
- Manager-like navigation (Workspaces, sidebar configuration)
- UI theme overrides (CSS, client scripts)
- Configuration defaults (UAE Chart of Accounts, VAT, currency)
- Print format templates (version-controlled, stable)
- Custom fields and any extra DocTypes
- Business rules, server scripts, and workflow customizations
- Module label overrides (LPO, GRN, etc.)

### 4.2 Core + Edition Model

To avoid maintaining separate forks for different industries, the architecture follows a Core + Edition pattern:

| App | Purpose | When Installed |
|-----|---------|---------------|
| `erp_v1_core` | Manager-like UI, UAE localization, base config | Always (every customer) |
| `erp_v1_technical` | Technical services: projects, job costing, timesheets | Technical services businesses |
| `erp_v1_garage` | Automotive: job cards, parts, vehicle tracking | Future: automotive businesses |
| `erp_v1_trading` | General trading: inventory focus, import/export | Future: trading businesses |
| `erp_v1_electrical` | Electrical: contract management, BOQ | Future: electrical businesses |

This means one shared foundation with industry-specific add-ons. When selling to a car manufacturer, install Core + Garage edition. For an accountant handling mixed clients, install Core + relevant editions. No forking, no maintaining 10 different codebases.

### 4.3 Multi-Tenancy Model

> **DECISION:** 1 Business = 1 Frappe Site (separate database, complete data isolation). This matches how Manager.io works — each business is a completely separate file.

**How It Works:**
- Each business gets its own Frappe site with its own database
- Sites share the same codebase (ERPNext + Core app + any edition apps)
- Data is completely isolated: customers, invoices, settings never leak between businesses
- Example: business1.localhost, business2.localhost, business3.localhost

**Business Switcher Portal (Deferred):**
A Manager Cloud-style dashboard that lists all businesses and allows one-click switching will be built as a separate lightweight portal in a later phase. This portal will use SSO/OAuth for seamless login across sites. For Phase 1, users simply navigate to the URL of the business they want to access.

**Subscription Tiers (Future):**

| Tier | Max Businesses | Target User |
|------|---------------|-------------|
| Basic | 1 | Single business owner |
| Professional | 3 | Business owner with multiple companies |
| Accountant | 10 | Accountant managing client businesses |
| Enterprise | Unlimited | Large firms |

---

## 5. Modules & Functional Requirements

All modules below are included in Phase 1. ERPNext modules are never deleted; unused modules are hidden from the navigation and can be revealed when needed.

> **RULE:** Hide modules, never delete them. Advanced features are tucked away in collapsible sections, not removed. Users can promote frequently-used features to their main navigation.

### 5.1 Module Mapping (Manager.io to ERP-V1)

| What You Call It | ERPNext DocType | ERP-V1 Label | Status |
|-----------------|----------------|-------------|--------|
| Quotation | Quotation | Quotation | Phase 1b |
| Sales Order | Sales Order | Sales Order | Phase 1b |
| Sales Invoice | Sales Invoice | Sales Invoice | Phase 1b |
| Delivery Note | Delivery Note | Delivery Note | Phase 1b |
| Credit Note | Sales Invoice (is_return) | Credit Note | Phase 1b |
| LPO (Local Purchase Order) | Purchase Order | LPO | Phase 1c |
| GRN (Goods Received Note) | Purchase Receipt | GRN | Phase 1c |
| Purchase Invoice | Purchase Invoice | Purchase Invoice | Phase 1c |
| Debit Note | Purchase Invoice (is_return) | Debit Note | Phase 1c |
| Receive/Spend Money | Payment Entry | Payment Entry | Phase 1a |
| Bank & Cash Accounts | Account (Bank/Cash) | Bank & Cash | Phase 1a |
| Customer | Customer | Customer | Phase 1b |
| Supplier | Supplier | Supplier | Phase 1c |
| Dashboard | Custom Page/Workspace | Dashboard | Phase 1d |
| (not in Manager) | Bank Reconciliation | Bank Reconciliation | Phase 1a |
| (not in Manager) | Project | Projects | Phase 1d |

### 5.2 Standard Workflows

**Sales Cycle:**
Quotation → Sales Order → Delivery Note → Sales Invoice → Payment Entry

Steps can be skipped where appropriate (e.g., direct invoice without Sales Order). Credit Notes are created as return Sales Invoices linked to the original.

**Purchase Cycle:**
LPO (Purchase Order) → GRN (Purchase Receipt) → Purchase Invoice → Payment Entry

Debit Notes are created as return Purchase Invoices linked to the original.

### 5.3 ERPNext Features Superior to Manager.io

| Feature | Manager.io | ERPNext (Our Advantage) |
|---------|-----------|----------------------|
| Form Customization | Custom fields stacked vertically | Sections, columns, tabs, collapsible groups |
| Workflow Engine | Not available | Approval chains for quotes/invoices/POs |
| Bank Reconciliation | Manual matching only | Auto-matching with bank statements |
| Print Format Builder | Fragile, breaks often | Visual builder, version-controlled, Jinja templates |
| Role Permissions | Basic (admin vs restricted) | Granular per-document-type, per-field permissions |
| Project Management | Not available | Job costing, timesheets, profitability tracking |
| Auto-Recurring Invoices | Workaround needed | Built-in recurring invoice engine |
| Customer Portal | Not available | Clients view their own invoices/quotes online |
| REST API | Not available | Full REST API for any integration |
| Dashboard & Charts | Basic account balances | Real-time charts, custom widgets, drill-down |
| Document Linking | Limited | Full chain: Quote to Order to Invoice to Payment |
| Multi-Currency | Basic support | Real-time exchange rates, per-transaction currency |

---

## 6. User Experience & Interface Design

### 6.1 Manager.io-Like Simplicity

The interface will replicate the clean, minimal feel of Manager.io: a clear sidebar navigation, uncluttered forms, and a blue/white color scheme. The goal is to reduce visual noise so that common tasks require minimal clicks and no training.

### 6.2 Theme

- **Color scheme:** Manager.io blue/white clean theme (customizable later)
- **Font:** Clean sans-serif (system default or Inter/Roboto)
- **Sidebar:** Clean module list with icons, collapsible sections
- **Navbar:** Simplified, with business name and user menu
- **Forms:** Logical sections with clear labels, not a wall of fields
- **Lists:** Clean table views with relevant columns, quick filters

### 6.3 Navigation Model

The sidebar shows commonly-used modules by default. An expandable "More" or "Advanced" section provides access to additional ERPNext modules. Users can customize which modules appear in their main navigation through a Settings page, similar to how Manager.io allows enabling/disabling sidebar items.

### 6.4 Form Layout Strategy (Key Differentiator)

> This is the #1 improvement over Manager.io. Forms will be organized, not just a list of fields.

**Step 1: Use Existing ERPNext Fields**
Before adding any custom field, first check if ERPNext already provides the concept. ERPNext already includes: addresses (with address templates), contacts, terms & conditions (multiple templates), letterhead/branding, project links, tax templates, payment terms, and much more. These should be used and rearranged, not duplicated as custom fields.

**Step 2: Organize with Sections and Tabs**
ERPNext's Customize Form tool allows adding section breaks, column breaks, and tab breaks to organize fields into logical groups. For example, a Sales Invoice can have tabs for: Details, Items, Taxes, Payment, Terms, and Additional Info.

**Step 3: Collapsible Advanced Sections**
Advanced or rarely-used fields are placed in collapsible sections that are closed by default. Users can expand them when needed, and can "pin" them open if they use them regularly. This prevents the long-list problem of Manager.io while keeping all data accessible.

**Step 4: Print-Only Elements Stay in Print Formats**
Elements that only need to appear on printed documents (stamps, signature areas, static text blocks, company seals) are handled entirely in Print Format templates, NOT as data entry fields. This keeps the data entry form clean and focused on actual data.

**Step 5: Custom Fields Only When Truly Needed**
After exhausting Steps 1-4, if genuinely new data needs to be captured, custom fields are added as part of the Core app (so they deploy consistently to all sites). These are organized into named sections, never appended to a flat list.

### 6.5 Dashboard

The dashboard will show at a glance: bank and cash account balances, outstanding receivables (who owes you), outstanding payables (what you owe), monthly revenue trend, recent transactions, and project status summaries. Card-based layout, clean and readable.

### 6.6 Users & Permissions

Manager.io-style permissions as a baseline. Roles include: Administrator (full access), Standard User/Accountant (transaction entry, reports), and Read-Only/Viewer (view only, no editing). Permissions can be refined per module as needed. ERPNext's granular permission system is available for advanced setups but is hidden from basic users.

### 6.7 Document Numbering

ERPNext's auto-naming with configurable prefixes (e.g., QTN-2025-0001, INV-2025-0001). Each site/company can configure its own numbering format. Naming series are managed through ERPNext's built-in Naming Series tool.

### 6.8 Terms & Conditions

ERPNext's built-in Terms and Conditions templates are used. Multiple templates can be created and selected per document. Different templates for different document types or customers. Managed through a simple list view.

---

## 7. UAE Localization

### 7.1 Currency & Tax

- **Default currency:** AED (UAE Dirham)
- **Multi-currency enabled:** transactions can be in any currency with exchange rate tracking
- **VAT:** 5% pre-configured as default tax template
- **TRN (Tax Registration Number):** fields added on Company, Customer, and Supplier DocTypes
- **UAE Chart of Accounts:** standard UAE chart pre-loaded

### 7.2 Emirates Support

Emirate selection field on Company and Address records. Initially configured for Dubai and Sharjah, expandable to all seven Emirates.

### 7.3 E-Invoicing (DCTCE / Peppol)

**Phase 1 (Architecture-Ready):**
- Data model supports e-invoice identifiers and status fields
- Audit logging for invoice submissions
- Print formats include all fields required by FTA

**Phase 3 (Full Integration):**
- Generate Peppol-compliant XML from Sales Invoices
- API integration with Accredited Service Provider (ASP)
- Submission status tracking (Pending, Submitted, Accepted, Rejected)
- Monitoring, retry logic, and error handling

### 7.4 Future International Expansion

The architecture supports adding other country localizations (Saudi Arabia, Bahrain, Oman, etc.) as separate configuration modules within the Core app. International expansion is planned but deferred until the UAE version is stable and in production.

---

## 8. Development Roadmap

### 8.1 Phase 0: Environment Setup

| Task | Description |
|------|-------------|
| Project folder structure | Create project folder, clone frappe_docker |
| Docker containers | Configure and start ERPNext v16 development containers |
| Create Frappe site | New site with MariaDB, install ERPNext |
| Create custom app | Scaffold erp_v1_core Frappe app |
| GitHub integration | Link app to github.com/MuhammadRayyan/erp-v1 |
| VS Code setup | Dev Containers extension, workspace config |
| **TOTAL** | **1-2 sessions** |

### 8.2 Phase 1a: Accounting Foundation

| Task | Description |
|------|-------------|
| UAE Company | Create Test Technical Services LLC with AED, VAT, TRN |
| Chart of Accounts | UAE standard chart, configured for technical services |
| Bank & Cash Accounts | Set up bank and cash accounts |
| VAT Tax Template | 5% VAT template, input and output tax accounts |
| Payment Entry | Configure receive money / spend money |
| Bank Reconciliation | Set up auto-matching bank reconciliation |
| Journal Entry | Basic journal entry for adjustments |
| **TOTAL** | **2-3 sessions** |

### 8.3 Phase 1b: Sales Cycle + UI Theme

| Task | Description |
|------|-------------|
| Customer Management | Customer list, creation form, contact linking |
| Quotation | Create, send, convert to Sales Order |
| Sales Order | From quotation or direct creation |
| Delivery Note | Linked to Sales Order, stock movement |
| Sales Invoice | With VAT, multi-currency, from SO or direct |
| Credit Note | Return Sales Invoice linked to original |
| UI Theme Start | Manager.io blue/white sidebar, clean forms |
| Form Layout | Organize invoice/quotation fields into sections |
| **TOTAL** | **3-4 sessions** |

### 8.4 Phase 1c: Purchase Cycle

| Task | Description |
|------|-------------|
| Supplier Management | Supplier list, creation, contacts |
| LPO (Purchase Order) | Local Purchase Order with label override |
| GRN (Purchase Receipt) | Goods Received Note with label override |
| Purchase Invoice | Supplier invoices with VAT |
| Debit Note | Return Purchase Invoice |
| **TOTAL** | **2-3 sessions** |

### 8.5 Phase 1d: Projects, Dashboard & Reports

| Task | Description |
|------|-------------|
| Project Management | Job tracking, cost tracking, profitability |
| Dashboard | Card-based overview: balances, receivables, payables |
| Reports | P&L, Balance Sheet, Trial Balance, Aged Receivables/Payables, VAT Report |
| **TOTAL** | **2-3 sessions** |

### 8.6 Phase 1e: Print Formats

| Task | Description |
|------|-------------|
| Sales Invoice Print | Professional template with VAT, TRN, branding |
| Quotation Print | Clean quotation template |
| Delivery Note Print | Delivery note with item details |
| LPO Print | Purchase order template |
| GRN Print | Goods received template |
| Payment Receipt Print | Payment confirmation template |
| **TOTAL** | **1-2 sessions** |

### 8.7 Phase 1f: Testing & Stabilization

| Task | Description |
|------|-------------|
| Sample Data Entry | Enter realistic test transactions for a full month |
| Workflow Validation | Test complete sales and purchase cycles end-to-end |
| Report Validation | Verify P&L, Balance Sheet, VAT report accuracy |
| Bug Fixes | Fix any issues discovered during testing |
| Structure Freeze | Lock down field structure, print formats, workflows |
| **TOTAL** | **1-2 sessions** |

### 8.8 Phase 2: Data Migration from Manager.io

- Export customer, supplier, item masters from Manager.io (CSV/JSON)
- Import into ERPNext using Data Import tool
- Set opening balances for accounts
- Import outstanding invoices and bills
- Estimated: 1 session

### 8.9 Phase 3: Multi-Site Setup

- Create additional Frappe sites for each business
- Install Core app on each site
- Validate consistent defaults and print formats across sites
- Estimated: 1 session

### 8.10 Phase 4: UAE E-Invoicing Integration

- Add e-invoicing fields, status tracking, audit logs to Core app
- Build Peppol XML generation from Sales Invoices
- Integrate with Accredited Service Provider (ASP) API
- Monitoring, retry logic, error handling
- Estimated: 2-3 sessions

### 8.11 Phase 5: Productization

- Packaging and installer documentation
- Industry edition add-on apps (Garage, Trading, Electrical)
- Cloud deployment templates
- Business switcher portal with SSO
- Onboarding wizard for new customers
- Estimated: 3-5 sessions

---

## 9. Design Rules (Non-Negotiable)

| # | Rule | Reason |
|---|------|--------|
| 1 | Never edit ERPNext core code | Ensures smooth upgrades and compatibility |
| 2 | All changes in Core app or add-on apps | Single source of truth, version-controlled |
| 3 | Hide modules, never delete them | Advanced features available when needed |
| 4 | Print formats version-controlled in Core app | No manual fixes, consistent across sites |
| 5 | Custom fields only when ERPNext lacks the concept | Prevents form bloat, uses existing infrastructure |
| 6 | Print-only elements in Print Formats, not forms | Keeps data entry clean and focused |
| 7 | Core + Edition model for industry variants | Avoids fork-per-customer maintenance hell |
| 8 | 1 Business = 1 Site for data isolation | Complete separation, no data leakage |
| 9 | Test with real-like data before expanding | Catches issues early, validates workflows |
| 10 | Document all decisions in DECISIONS.md | Remembers WHY choices were made |

---

## 10. Risks & Mitigations

| Risk | Impact | Mitigation |
|------|--------|-----------|
| Fork-per-customer maintenance overload | High | Core + Edition add-on architecture eliminates forks |
| Multi-business UX complexity | Medium | Business-per-site now; portal/SSO later only if needed |
| Overbuilding too early | Medium | Sub-phased approach; each module validated before next |
| ERPNext upgrade breaks customizations | Medium | All changes in custom app, never in core; test upgrades on dev first |
| E-invoicing spec changes before July 2026 | Low | Architecture-ready now, implementation deferred; modular design adapts |
| Print format fragility (like Manager) | Low | Jinja templates in version control; never manual edits in production |
| Learning curve for new developers | Medium | Comprehensive documentation, PROGRESS.md, decision log |

---

## 11. Test Company Configuration

| Setting | Value |
|---------|-------|
| Company Name | Test Technical Services LLC |
| Default Currency | AED (UAE Dirham) |
| Multi-Currency | Enabled |
| VAT Rate | 5% |
| Emirates | Dubai, Sharjah |
| Chart of Accounts | UAE Standard |
| Fiscal Year | January - December |
| Initial Users | 2-3 |
| Logo/Branding | To be configured later |

---

## 12. Future Roadmap (Post-MVP)

These features are planned but not part of the initial build:

- Arabic language support (RTL interface)
- Mobile app (Frappe Mobile or custom PWA)
- POS (Point of Sale) module
- HR & Payroll module
- Manufacturing module (for industrial clients)
- CRM module (lead tracking, sales pipeline)
- Inventory/Stock with serial and batch tracking
- API marketplace for third-party integrations
- White-labeling for resellers
- AI-powered insights and anomaly detection
- International expansion (Saudi Arabia, Bahrain, Oman, etc.)

---

## 13. Project Continuity & Session Management

> This section is critical. It ensures the project can be resumed from any point, even if the chat history is lost, a new developer joins, or you switch to a different AI assistant.

### 13.1 Repository Files

The following files must be maintained in the Git repository at all times:

| File | Purpose | Updated When |
|------|---------|-------------|
| README.md | Project overview, tech stack, how to run, quick links | When setup or structure changes |
| REQUIREMENTS.md | This document (full requirements and architecture) | When requirements change |
| PROGRESS.md | What was done last session + what is next (most important for resuming) | End of every session |
| CHANGELOG.md | Notable changes by date/version | When significant features are added |
| DECISIONS.md | Short decision log: what we chose, why, and alternatives considered | When decisions are made |
| TASKS.md | Backlog (to-do), in-progress, and completed tasks checklist | End of every session |

### 13.2 End-of-Session Protocol

When the user says "let's stop", "let's continue tomorrow", or any variation indicating the session is ending, the following steps must be performed:

1. **Step 1:** Update PROGRESS.md with: what was completed today, what was discovered/decided, the single most important next step, any blockers or questions, and relevant commands/references.
2. **Step 2:** Update CHANGELOG.md if anything significant changed (new features, configuration changes, bug fixes).
3. **Step 3:** Update TASKS.md checkboxes (move completed tasks to Done, update In Progress).
4. **Step 4:** Git commit all changes with a descriptive message.
5. **Step 5:** Git push to GitHub.
6. **Step 6:** Provide the user with downloadable copies of PROGRESS.md and TASKS.md as backup.

### 13.3 Resuming a Session

To resume work after a break (whether the next day, next week, or with a new AI assistant):

- **Option A (same chat):** Simply continue the conversation. The AI has full context.
- **Option B (new chat, same AI):** Share this REQUIREMENTS.md document plus PROGRESS.md and TASKS.md. The AI can pick up exactly where you left off.
- **Option C (different AI or developer):** Share this REQUIREMENTS.md document plus all repository files (README.md, PROGRESS.md, CHANGELOG.md, DECISIONS.md, TASKS.md). Any competent developer or AI assistant can understand the full project context and continue.

### 13.4 What Makes This Work

The combination of these files provides complete project memory:

- **REQUIREMENTS.md** (this file) = the "what" and "why"
- **PROGRESS.md** = the "where we are now"
- **DECISIONS.md** = the "why we chose this approach"
- **TASKS.md** = the "what is left to do"
- **CHANGELOG.md** = the "what has changed over time"
- **README.md** = the "how to set up and run everything"

Together, these six files mean the project is never "lost". Even if every chat conversation disappears, the repository contains everything needed to understand, run, and continue developing ERP-V1.

---

## 14. Open Items & Intentionally Flexible Areas

The following items are intentionally not fixed and will be refined during implementation:

- **Exact Manager-like UX:** Will be iterated based on actual usage during testing. Target: fewer clicks, clean navigation, fast data entry.
- **Hosting strategy for production:** Start local (Windows + Docker). Later: choose simplest managed option vs self-hosting based on cost and comfort. Hosting location can be UAE or EU.
- **Business switcher portal:** Deferred. Will only be built if multi-site usage makes it necessary.
- **Industry editions:** Start with Core app only. Add edition add-ons only when there is real demand from paying customers.
- **E-invoicing provider:** FTA has not finalized all ASP details. Architecture is ready; provider selection happens closer to July 2026.
- **Company logo and branding:** Placeholder for now. Will be configured when ready.
- **Exact report list:** Start with standard accounting reports. Custom reports added based on usage needs.
- **Project workflow details:** Fixed price vs time & materials, profitability tracking depth to be determined during Phase 1d.

---

## 15. Summary

ERP-V1 is a Manager.io-style ERP built on ERPNext/Frappe v16 as a custom app (`erp_v1_core`), ensuring all customizations are upgrade-safe and version-controlled. The system starts with comprehensive Accounting, Sales, Purchase, and Project modules for a UAE technical services company, with complete UAE localization including VAT, AED multi-currency, and architecture-ready e-invoicing support for the July 2026 FTA mandate.

Multiple unrelated businesses are represented as separate Frappe sites sharing the same codebase, providing complete data isolation matching Manager.io's model. The Core + Edition architecture enables selling to diverse industries without maintaining separate forks. The interface prioritizes clean, minimal design with organized forms, collapsible advanced sections, and stable version-controlled print formats that solve Manager.io's key pain points.

Development follows a sub-phased approach: environment setup, then accounting foundation, sales cycle, purchase cycle, projects, dashboard, print formats, and finally testing with realistic data. Data migration from Manager.io occurs only after the system is stable. A comprehensive project continuity system (six repository files) ensures the project can be resumed from any point, by any developer or AI assistant, regardless of chat history.
