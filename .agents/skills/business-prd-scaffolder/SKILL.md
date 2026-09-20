---
name: business-prd-scaffolder
description: "Framework and guidelines for scaffolding Business-Centric Product Requirements Documents (PRDs) for the Pharmacy POS & ERP System. Focuses strictly on business logic, pharmacy operations, workflows, and domain rules (FEFO, dispensing, compounding/racikan, regulatory compliance), while preventing low-level technical/engineering leakages."
---

# Business-Centric PRD Scaffolder Skill (Pharmacy POS & ERP)

Standardized framework for creating high-impact, business-centric Product Requirements Documents (PRDs) for the **Pharmacy POS & ERP System (Apotek)**.

---

## 1. Core Philosophy: "WHAT & WHY", Not "HOW"

A **Product Requirements Document (PRD)** is written for business stakeholders, pharmacy owners, Apoteker Pengelola Apotek (APA), Tenaga Teknis Kefarmasian (TTK), cashiers, product managers, and UI/UX designers.
Technical implementation details belong exclusively in technical documents (`technical/database/schema.dbml`, `technical/diagrams/`, and `technical/0X-trd-*.md`).

### 🚫 Strictly Prohibited in PRDs:
* ❌ **API Endpoints & HTTP Methods:** Forbidden to include endpoint tables (e.g., `POST /api/v1/pos/checkout`, `GET /branches`).
* ❌ **JSON Payloads & DTOs:** Forbidden to include request/response JSON payload schemas.
* ❌ **Database Tables, DDL & Foreign Keys:** Forbidden to name SQL tables (`sales`, `product_stocks`), SQL data types (`UUID`, `VARCHAR`, `DECIMAL`), primary keys, or database constraints.
* ❌ **Cryptographic / Technical Algorithms:** Forbidden to specify low-level algorithms (e.g., `Argon2id`, `bcrypt`, `JWT RS256`).
* ❌ **Code Snippets & Libraries:** Forbidden to write programming language code, ORMs, or technical libraries.

### ✅ Mandatory Focus in PRDs:
* ✅ **Background & Business Value:** Why is this feature necessary for pharmacy operational efficiency, margin protection, prevention of expired drugs, and compliance with MoH/BPOM regulations?
* ✅ **User Context & Working Environment:** Cashier at the front counter facing queue pressure, assistant pharmacist at the compounding desk calculating dosages, warehouse staff verifying PBF invoices, or pharmacist screening prescriptions.
* ✅ **User Journey & Experience:** Step-by-step narrative and visual Mermaid user journeys from the perspective of what pharmacy staff observe and execute on the screen.
* ✅ **Strict Business Rules (`BR-PHARM-XX-YY`):** Concrete operational policies (e.g., FEFO allocation, minimum reorder point thresholds, prescription verification requiring valid doctor SIP, cashier discount limits, supervisor override PIN mechanism).
* ✅ **Edge Cases & Exception Handling:** What happens when raw ingredients run out midway through compounding, a physical batch differs from a PBF invoice, or a cashier discovers a cash shortage at shift closure?
* ✅ **Strict Document Output Language:** All generated PRD documents inside `features/` MUST be written in **Bahasa Indonesia** to ensure seamless comprehension by local pharmacy practitioners, apoteker, cashiers, and compliance auditors (following Permenkes No. 73/2016 & BPOM terminology).
* ✅ **Acceptance Criteria (Gherkin):** Functional verification checklist from the user/QA perspective using Given-When-Then syntax.

---

## 2. Comparison Table: PRD (Business) vs TRD / DRA (Technical)

| Aspect | Proper Phrasing in PRD (Pure Business) | Phrasing in TRD / DRA (Technical) |
| :--- | :--- | :--- |
| **Branch Context** | "The system binds the cashier session to the Main Branch and ensures all sales deduct physical inventory from that branch." | `branch_id UUID NOT NULL REFERENCES branches(id)`, Header: `X-Branch-Id: <uuid>` |
| **Stock Deduction** | "Items dispensed at checkout automatically pick the batch with the nearest expiration date (FEFO)." | `CREATE INDEX idx_stocks_fefo ON product_stocks (branch_id, product_id, expired_date ASC)` |
| **Cashier Login** | "Cashiers can log in instantly using a 6-digit PIN or barcode card scan to eliminate queue bottlenecks." | Dual-UX endpoint `POST /api/v1/auth/login-pin`, Argon2id hashing, JWT claim `branch_id` |
| **Void Approval** | "Cancelling a scanned item requires the approval PIN of the on-duty Pharmacist or Supervisor." | Endpoint `POST /api/v1/pos/override-approval` with role validation `SUPERVISOR` or `APOTEKER` |
| **Form Input Spec** | "Pharmacist SIPA Number: Mandatory text field storing the official license number and its expiration date." | `sipa_number VARCHAR(100) NOT NULL`, `sipa_expired_date DATE NOT NULL` |

---

## 3. Standard Template Structure for Feature PRDs

Every feature PRD inside `features/` MUST strictly adhere to this structure:

```markdown
# Feature PRD: [Feature Name / Pharmacy Module]

## 1. Document Metadata
- **Document Code:** PRD-PHARM-XX
- **Module Name:** [Module Name]
- **Master PRD:** 00-MASTER-PRD.md
- **Depends On (Prerequisites):** [List of PRDs/modules that this feature depends on]
- **Consumed By (Downstream):** [List of PRDs/modules impacted by this feature]
- **Target Users:** [Cashier, Assistant Pharmacist / TTK, Pharmacist In Charge (APA), Procurement Staff, Owner]

## 2. Background & Business Problem
- Why is this feature needed?
- What real-world pharmacy problem does it solve (e.g., slow queues, batch mix-ups, expired drug financial losses)?
- Financial benefits and regulatory compliance impact.

## 3. Personas & Operational Context
- Who are the users?
- What are their operational environments? (Touchscreen cashier / USB barcode scanner, compounding lab, warehouse receiving).

## 4. Core User Journey
- Visual workflow diagram (Mermaid flowchart / user journey).
- Step-by-step narrative from the user's perspective.

## 5. Functional Requirements & Business Rules
Use standardized coding: `BR-PHARM-[MODULE_CODE]-[NUMBER]` (e.g., `BR-PHARM-AUTH-01`, `BR-PHARM-POS-03`).
- **BR-PHARM-XX-01:** [Unambiguous business rule description]
- **BR-PHARM-XX-02:** [Business calculation or validation rule]

## 6. User Interface & Information Elements
- List of information displayed to users (without referencing database types).
- Form inputs, interactive elements, and available actions.

## 7. Edge Cases & Exception Scenarios
- Handling abnormal conditions (out of stock during compounding, cashier input errors, transient offline state).

## 8. Business Success Metrics (KPIs)
- Direct operational impact (e.g., checkout time < 15 seconds, zero near-miss dispensing errors).

## 9. Acceptance Criteria
Format: Given [initial state], When [user action], Then [expected business outcome].
- **AC-01:** ...
- **AC-02:** ...
```
