---
name: issue-task-scaffolder
description: "Framework and guidelines for scaffolding lean, high-clarity GitHub Issues for Backend, Frontend Web Admin, and Frontend POS engineers from PRD, DRA, and TRD documentation."
---

# Issue Task Scaffolder Skill (Spec-Driven / Issue-Driven Development)

Standardized framework for creating production-ready, lean **GitHub Issues** from the **Pharmacy POS & ERP System** documentation for Backend, Web Admin, and POS engineers.

---

## 1. Core Principles of Issue-Driven Development (IDD)

1. **Single Source of Truth (SSOT):**
   * Business specifications (`PRD`), database architecture (`DBML`), and API contracts (`TRD`) are the absolute sources of truth.
   * **Lean Scoping Rule:** Strictly forbidden to copy-paste 500 lines of SQL DDL or hundreds of lines of JSON payloads into the GitHub Issue body. Issue tickets contain only the task objective, an actionable checklist (*Acceptance Criteria*), and **direct clickable markdown links** to reference documents in the repository.
2. **Official Ticket Categories:**
   * `[BE]` — **Backend & Database:** PostgreSQL migrations, domain business logic, FEFO stock locking, and RESTful API endpoints.
   * `[FE-WEB]` — **Frontend Web Admin/ERP:** Backoffice management dashboard, master data, central warehouse, procurement, and financial reporting.
   * `[FE-POS]` — **Frontend POS Cashier & Prescriptions:** Rapid keyboard-first checkout, USB barcode scanner listener, compounding calculator, and thermal receipt & label printing.
3. **Traceability:** Every issue must explicitly link the specific business rule IDs (`BR-PHARM-XX-YY`) being implemented.
4. **Issue Output Language:** All GitHub Issue titles, task summaries, checklists, and acceptance criteria MUST be written in **Bahasa Indonesia** to ensure direct operational clarity for development and QA teams in Indonesia.

---

## 2. Standard Naming & Labeling Conventions

### Title Format:
```
[<ROLE>] <Module Name>: <Specific Action / Task Scope>
```
*Examples:*
* `[BE] Drug Master Data: Schema Migrations & Multi-Unit CRUD REST API`
* `[FE-POS] OTC Cashier: Keyboard-First Checkout, Barcode Scanning & Thermal Printing`
* `[FE-WEB] Procurement: Digital Defekta Interface & Purchase Order Generator`

### Label Conventions:
* **Role:** `backend`, `frontend-web`, `frontend-pos`
* **Domain:** `master-data`, `pos`, `inventory`, `procurement`, `finance`, `compliance`
* **Type:** `feature`, `enhancement`, `bug`

---

## 3. Standard Issue Body Templates

### A. Backend Format (`[BE]`)
```markdown
## 📌 Task Summary
Implement PostgreSQL database migrations and RESTful API endpoints for [Module Name].

## 📚 Reference Documents (SSOT)
- **Business PRD:** [Module PRD](URL_TO_PRD)
- **Database Architecture:** [DBML Schema](URL_TO_DBML#anchor)
- **API Contracts (TRD):** [TRD Specifications](URL_TO_TRD#anchor)

## 🎯 Scope & Checklist
- [ ] Create PostgreSQL migration files adhering to universal audit columns and `branch_id`.
- [ ] Implement Service & Repository layers with atomic database transaction handling (`DB Transaction`).
- [ ] Implement Controllers & DTO validation matching the standardized TRD envelope.
- [ ] Unit tests & API integration tests (Coverage > 80%).

## 🛡️ Business Rules to Enforce
- [ ] `BR-PHARM-XX-01`: ...
- [ ] `BR-PHARM-XX-02`: ...
```

### B. Frontend POS Cashier Format (`[FE-POS]`)
```markdown
## 📌 Task Summary
Build the front-office cashier interface for [Module Name] featuring keyboard-first navigation and thermal printer integration.

## 📚 Reference Documents (SSOT)
- **Business PRD:** [Module PRD](URL_TO_PRD)
- **API Contracts (TRD):** [TRD Specifications](URL_TO_TRD#anchor)

## 🎯 Scope & Checklist
- [ ] High-speed search interface components (barcode & brand/generic name).
- [ ] Keyboard shortcuts mapping (F1 - F12, Enter, Escape).
- [ ] Thermal receipt & label printer integration (ESC/POS).
- [ ] Cart state management (hold/recall cart buffers).
```

### C. Frontend Web Admin / ERP Format (`[FE-WEB]`)
```markdown
## 📌 Task Summary
Build the backoffice web management interface for [Module Name].

## 📚 Reference Documents (SSOT)
- **Business PRD:** [Module PRD](URL_TO_PRD)
- **API Contracts (TRD):** [TRD Specifications](URL_TO_TRD#anchor)

## 🎯 Scope & Checklist
- [ ] Data table components (sorting, pagination, multi-branch filtering).
- [ ] Form input & client-side validation schema.
- [ ] Report generation and exports (PDF & Excel).
```
