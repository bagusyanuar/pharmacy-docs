---
name: trd-dra-scaffolder
description: "Framework and architectural standards for scaffolding Technical Requirements Documents (TRD) and Data Requirements Architecture (DRA) specifications for Pharmacy POS & ERP. Enforces PostgreSQL best practices, strict multi-branch isolation (branch_id), universal audit trails, precise decimal types, FEFO index strategies, RESTful API envelope standardization, and bidirectional traceability to PRD business rules."
---

# TRD & DRA Scaffolder Skill (Pharmacy POS & ERP)

Standardized framework for creating production-ready, enterprise-grade **Technical Requirements Documents (TRD)** and **Data Requirements Architecture (DRA)** for the **Pharmacy POS & ERP System (Apotek)**.

---

## 1. Core Principles of Technical Documentation

Technical specifications translate business requirements from the PRD into concrete, unambiguous engineering blueprints for Backend Engineers, DBAs, Frontend POS Engineers, and DevOps.

We divide technical architecture into a **Structured 3-Tier Technical Blueprint**:

1. **Global DBML (`technical/database/schema.dbml`):**
   * **Focus:** *Single Source of Truth (SSOT)* for the complete relational database schema intended for DBAs and Backend Leads.
   * **Format:** Pure `.dbml` file ready for direct import/rendering in [dbdiagram.io](https://dbdiagram.io) or `dbdocs.io`.
   * **Scoping:** Globalized per major milestone (encompassing all tables, `[delete: restrict]` foreign key relationships, and `TableGroups`).

2. **Visual Blueprints & System Analysis (`technical/diagrams/0X-diagrams-*.md`):**
   * **Focus:** Visual system analysis for UI/UX Designers, QA Testers, and Developers.
   * **Format:** Markdown document with interactive Mermaid visual diagrams:
     * **Usecase Diagram:** Authority boundaries of the 6 pharmacy staff personas across system features.
     * **Detailed System Flowcharts:** Decision-branching logic for complex workflows (login, supervisor override, cron license checks).
     * **Sequence Diagrams:** Technical message exchange across *Client POS $\leftrightarrow$ Gateway $\leftrightarrow$ Backend $\leftrightarrow$ Database*.
     * **State Lifecycle Diagrams:** Entity state lifecycles (staff user accounts, cashier shifts, purchase invoices).
     * **Scoped Sub-ERD:** Mermaid ERD rendering only 4–6 focused tables for that specific feature (preventing "spaghetti diagrams").
   * **Mermaid Syntax Rules:**
     * FORBIDDEN to use angle brackets `<` or `>` inside arrow/edge labels (e.g., always use `|include|`, NEVER `|"<<include>>"|` as it triggers fatal syntax errors in Mermaid parsers).
     * When embedding inside HTML preview files, always wrap in `<pre class="mermaid">` (instead of `<div>`) to prevent browser HTML newline compression.

3. **TRD API Contracts & Architecture (`technical/0X-trd-*.md`):**
   * **Focus:** Implementation specifications and inter-module data exchange contracts.
   * **Format:** Technical Markdown specifying RESTful API contracts, JSON DTO payloads, standardized response envelope `{ success, data, meta, error }`, normalized error codes, and hardware protocols (ESC/POS thermal printing).
   * **Scoping:** Modularly split per API domain cluster.

* **Document Output Language:** All generated TRDs (`technical/0X-trd-*.md`) and visual diagram documents (`technical/diagrams/0X-diagrams-*.md`) MUST be written in **Bahasa Indonesia** to align with Indonesian pharmacy operational terminology and local regulations.
* **Bidirectional Traceability:** Every database column, constraint, and API endpoint MUST explicitly trace back to a business rule in the corresponding PRD using its ID (e.g., `-- Implements BR-PHARM-WMS-02`).

---

## 2. Invariants & Standards for DRA (Database Architecture & ERD)

All database architectural specifications MUST comply with the following PostgreSQL 15+ standards:

### 2.1 Multi-Branch Ready from Day 1 (Absolute Invariant)
Every physical inventory, warehouse mutation, cashier transaction, shift session, and branch-specific procurement table MUST include:
```sql
branch_id UUID NOT NULL REFERENCES branches(id) ON DELETE RESTRICT
```
*Global master data tables (e.g., `products`, `generic_names`, `manufacturers`, `pbf_suppliers`) do not use `branch_id`, but all operational transaction tables MUST be tied to a branch.*

### 2.2 Universal Audit Trail (Mandatory on Every Entity Table)
Every entity table MUST include these five standard columns:
```sql
id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
created_at  TIMESTAMPTZ NOT NULL DEFAULT NOW(),
updated_at  TIMESTAMPTZ NOT NULL DEFAULT NOW(),
created_by  UUID NULL REFERENCES users(id) ON DELETE RESTRICT,
deleted_at  TIMESTAMPTZ NULL -- Soft delete to preserve legal pharmacy BPOM & accounting audit trails
```

### 2.3 Relational Integrity & Deletion Policy
* **Foreign Keys:** MUST use `ON DELETE RESTRICT` (or `ON DELETE NO ACTION`). Hard cascade deletes (`ON DELETE CASCADE`) are **strictly prohibited** on historical transactions, batches, prescriptions, and financial ledgers to prevent destruction of legal pharmacy audit records.
* **Soft Deletes:** Deletion sets `deleted_at = NOW()`. Queries MUST filter `WHERE deleted_at IS NULL`.

### 2.4 Strict Data Type Conventions
* **Identifiers:** `UUID` (never auto-increment integers in distributed or multi-branch systems).
* **Currency / Financials:** `DECIMAL(12,2)` (Selling Price, COGS/HPP, Transaction Totals, Discounts). `FLOAT` or `DOUBLE PRECISION` is **strictly prohibited**.
* **Quantities & Formulations:** `DECIMAL(10,3)` (Supports extemporaneous compounding fractions, syrup milliliters, ointment grams, and fractional tablets).
* **Timestamps:** `TIMESTAMPTZ` (always timezone-aware, stored in UTC).
* **Expiry Dates:** `DATE` (for batch expiration tracking).

### 2.5 Indexing Strategy for Pharmacy Performance
* Composite index for active FEFO batch retrieval:
  ```sql
  CREATE INDEX idx_stocks_fefo ON product_stocks(branch_id, product_id, expired_date ASC) 
  WHERE deleted_at IS NULL AND quantity > 0;
  ```
* Rapid barcode scanning lookup:
  ```sql
  CREATE UNIQUE INDEX uq_products_barcode ON products(barcode) WHERE deleted_at IS NULL;
  ```

---

## 3. Standards for Unified Contract-First TRD (BE & FE Architecture)

In our integrated pharmacy system, we utilize the **Contract-First Single TRD per Feature Domain** approach (`technical/0X-trd-*.md`). The API contract acts as the *Single Source of Truth (SSOT)* binding both Backend and Frontend teams to prevent *documentation drift*.

Division of engineering labor is organized through **Domain-Specific Sub-Sections** inside the TRD and executed independently through **GitHub Issue Tickets (`[BE]`, `[FE-WEB]`, `[FE-POS]`)**.

---

### 3.1 Standard RESTful API Envelope
Every API response (both success and failure) MUST strictly follow the uniform standard envelope:

#### Success Response (HTTP 200 / 201):
```json
{
  "success": true,
  "data": { ... },
  "meta": {
    "page": 1,
    "limit": 20,
    "total_records": 150,
    "total_pages": 8
  },
  "error": null
}
```

#### Error Response (HTTP 4xx / 5xx):
```json
{
  "success": false,
  "data": null,
  "meta": null,
  "error": {
    "code": "STOCK_INSUFFICIENT",
    "message": "Stock for Paracetamol Exp 2026-10 at Melawai Branch is insufficient for this sale.",
    "details": [
      {
        "field": "quantity",
        "issue": "Requested 10, but available batch stock is only 4"
      }
    ]
  }
}
```

---

### 3.2 Backend [BE] Engineering Standards
This section must document internal server and database considerations:
1. **Multi-Branch Isolation (`X-Branch-Id`):**
   * Every transactional request must validate the `X-Branch-Id: <uuid>` header.
   * Backend middleware must verify whether the authenticated user has an active assignment in that branch (`user_branches`).
2. **Database Transaction Integrity (ACID & Row Locking):**
   * Cashier stock deductions or inventory mutations must execute inside a database transaction utilizing *pessimistic row locking* (`SELECT ... FOR UPDATE`) to eliminate race conditions and prevent negative stock during concurrent barcode scans.
3. **Credential & Session Security:**
   * Web ERP Password: Hash using **Argon2id** (or bcrypt with cost factor >= 12).
   * Cashier Fast PIN: Separate cryptographic hash for high-speed POS verification & Manager Override.
   * Authentication Token: **JWT RS256** (Short-lived Access Token: 15 minutes) + **Refresh Token Rotation (RTR)** stored in a `HttpOnly; Secure; SameSite=Strict` cookie.
4. **Background Jobs & Crons:**
   * Daily cron schedules (e.g., pharmacist SIPA license expiry checks at D-60/D-30/D-0, automated flagging of expired drug batches).

---

### 3.3 Frontend [FE-WEB & FE-POS] Engineering Standards
This section documents the Client-Side Architecture:
1. **Client State Management:**
   * Standardized store architecture (Zustand / Redux Toolkit / Pinia) for shopping carts, active branch session, and cashier shift cash float.
   * Optimistic UI updates for ultra-fast barcode scanning response (< 100ms cashier latency).
2. **POS Cashier Hardware Protocols (Front-Office):**
   * **Thermal Receipt Printer:** Raw byte stream using standard **ESC/POS** protocol (58mm or 80mm paper width), automatic paper cutter, and cash drawer kick-out pulse via RJ11 pin (`ESC p m t1 t2`).
   * **Label / Etiket Printer:** Thermal label formatting for internal medication labels (White) and external medication labels (Blue), standard size 50x30mm.
   * **Barcode Scanner:** Keyboard-wedge listener with input debounce buffer (detecting rapid stream < 50ms terminated by `Enter`).
3. **Keyboard-First Shortcuts Registry (POS):**
   * Physical keyboard mapping for high-speed cashier operation:
     * `F1`: Help / Shortcut Cheat Sheet
     * `F2`: General Walk-in Customer Mode
     * `F3`: Quick-Tag Prescription Mode
     * `F4`: Search PMR Registered Patient
     * `F8`: Convert Walk-in to Registered PMR Patient
     * `F9`: Hold Cart
     * `F10`: Recall Cart
     * `F12` / `Space`: Open Payment Dialog
     * `Esc`: Cancel / Close Modal
4. **Offline Resilience:**
   * PWA Service Worker + IndexedDB for local catalog caching and offline transaction queues during transient network interruptions.

---

### 3.4 Standard TRD Document Template

Every TRD document created in the `technical/` folder MUST strictly adhere to this standardized format:

```markdown
# Technical Requirements Document (TRD): [Module Name / Feature Cluster]

## 1. Metadata & Traceability
- **Document Code:** TRD-PHARM-XX
- **Module Name:** [Module Name]
- **Target PRD References:** [Link to PRD in features/]
- **Target Diagram References:** [Link to Visual Blueprint in technical/diagrams/]
- **Target DBML References:** [Link to technical/database/schema.dbml]
- **Target Engineers:** Backend [BE], Web Admin [FE-WEB], POS Cashier [FE-POS]

## 2. Component Architecture & Data Flow
- Concise explanation of component architecture (Service Layer, Repository Layer, Client Store).

## 3. RESTful API Contracts (The Single Source of Truth)
### 3.1 [HTTP_METHOD] /api/v1/[endpoint]
- **Description & Business Rule:** (Implements BR-PHARM-XX-YY)
- **Mandatory Headers:** Authorization: Bearer <jwt>, X-Branch-Id: <uuid>
- **Request Parameters / Body (JSON Schema):**
- **Response Success (JSON):**
- **Response Error Scenarios (HTTP Status & Error Codes):**

## 4. Backend [BE] Engineering Specifications
- SQL queries, index utilization, ACID transaction rules, and branch isolation.
- Middleware guards, server-side validation, hashing, and RBAC authorization.

## 5. Frontend Web Admin [FE-WEB] Engineering Specifications
- Web ERP routing, form validation schemas, and API integration.

## 6. Frontend POS Cashier [FE-POS] Engineering Specifications
- Keyboard-first navigation, USB barcode scanner listener, ESC/POS thermal printing, and local state management.

## 7. GitHub Issue Ticket Mapping
- Links to issue templates: [BE Task](.github/ISSUE_TEMPLATE/backend-task.md), [FE-WEB Task](.github/ISSUE_TEMPLATE/frontend-web-task.md), [FE-POS Task](.github/ISSUE_TEMPLATE/frontend-pos-task.md).
```
