---
name: trd-dra-scaffolder
description: "Framework and architectural standards for scaffolding Technical Requirements Documents (TRD) and Data Requirements Architecture (DRA) specifications for Pharmacy POS & ERP. Enforces PostgreSQL best practices, strict multi-branch isolation (branch_id), universal audit trails, precise decimal types, FEFO index strategies, RESTful API envelope standardization, and bidirectional traceability to PRD business rules."
---

# TRD & DRA Scaffolder Skill (Pharmacy POS & ERP)

Standardized framework for creating production-ready, enterprise-grade **Technical Requirements Documents (TRD)** and **Data Requirements Architecture (DRA)** for the **Pharmacy POS & ERP System (Apotek)**.

---

## 1. Core Principles of Technical Documentation

Technical specifications translate business requirements from the PRD into concrete, unambiguous engineering blueprints for Backend Engineers, DBAs, Frontend POS Engineers, and DevOps.

* **DRA (Data Requirements Architecture):** Focuses on the *Logical & Physical Data Model* (ERD, DDL PostgreSQL, constraints, indexing, and data retention).
  * **Scoping Rule:** **DIGLOBALKAN PER MILESTONE BESAR.** Database relasional memerlukan integritas Foreign Key dan satu diagram ERD utuh agar tidak tercerai-berai.
* **TRD (Technical Requirements Document):** Focuses on the *Software Architecture & API Contracts* (RESTful endpoints, DTO payloads, security mechanisms, caching, and hardware integration).
  * **Scoping Rule:** **DI-SPLIT PER KLASTER / DOMAIN API.** Setiap domain (Auth & Cabang, Master Data Obat, POS & Resep, WMS & FEFO, Procurement PBF, Keuangan) memiliki dokumen TRD tersendiri agar payload JSON terdokumentasi teratur.
* **Bidirectional Traceability:** Every database column, constraint, and API endpoint MUST explicitly trace back to a business rule in the corresponding PRD using its ID (e.g., `-- Implements BR-PHARM-WMS-02`).

---

## 2. Invariants & Standards for DRA (Database Architecture & ERD)

All DRA documents MUST comply with the following PostgreSQL standards:

### 2.1 Multi-Branch Ready from Day 1 (Invarian Mutlak)
Semua tabel stok fisik, mutasi gudang, transaksi kasir, sesi shift, dan pengadaan WAJIB menyertakan:
```sql
branch_id UUID NOT NULL REFERENCES branches(id) ON DELETE RESTRICT
```
*Tabel master data global (seperti `products`, `generic_names`, `manufacturers`, `pbf_suppliers`) tidak menggunakan `branch_id`, namun seluruh data turunan operasional WAJIB terikat ke cabang.*

### 2.2 Universal Audit Trail (Wajib di Setiap Tabel)
Every entity table MUST include these five standard columns:
```sql
id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
created_at  TIMESTAMPTZ NOT NULL DEFAULT NOW(),
updated_at  TIMESTAMPTZ NOT NULL DEFAULT NOW(),
created_by  UUID NULL REFERENCES users(id) ON DELETE RESTRICT,
deleted_at  TIMESTAMPTZ NULL -- Soft delete untuk menjaga rekam jejak audit BPOM & akuntansi
```

### 2.3 Relational Integrity & Deletion Policy
* **Foreign Keys:** MUST use `ON DELETE RESTRICT` (or `ON DELETE NO ACTION`). Hard cascade deletes (`ON DELETE CASCADE`) are **strictly prohibited** on historical transactions, batches, prescriptions, and financial ledgers to prevent destruction of legal pharmacy audit trails.
* **Soft Deletes:** Deletion sets `deleted_at = NOW()`. Queries MUST filter `WHERE deleted_at IS NULL`.

### 2.4 Strict Data Type Conventions
* **Identifiers:** `UUID` (never auto-increment integer in distributed or multi-branch systems).
* **Currency / Financials:** `DECIMAL(12,2)` (Harga Jual, HPP, Total Transaksi, Diskon). `FLOAT` or `DOUBLE PRECISION` is **strictly prohibited**.
* **Quantities & Formulations:** `DECIMAL(10,3)` (Mendukung pembagian racikan puyer, takaran sirup/ml, salep/gram, dan pecahan tablet).
* **Timestamps:** `TIMESTAMPTZ` (always timezone-aware, stored in UTC).
* **Expiry Dates:** `DATE` (untuk tanggal kadaluarsa batch obat).

### 2.5 Indexing Strategy for Pharmacy Performance
* Composite index untuk pencarian batch FEFO aktif:
  ```sql
  CREATE INDEX idx_stocks_fefo ON product_stocks(branch_id, product_id, expired_date ASC) 
  WHERE deleted_at IS NULL AND quantity > 0;
  ```
* Pencarian cepat barcode kasir:
  ```sql
  CREATE UNIQUE INDEX uq_products_barcode ON products(barcode) WHERE deleted_at IS NULL;
  ```

---

## 3. Standards for TRD (API Contracts & Architecture)

### 3.1 Standard RESTful API Envelope
Every API response must follow this standard envelope:
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
Untuk respon error:
```json
{
  "success": false,
  "data": null,
  "meta": null,
  "error": {
    "code": "STOCK_INSUFFICIENT",
    "message": "Stok batch obat Paracetamol Exp 2026-10 tidak mencukupi untuk transaksi ini.",
    "details": []
  }
}
```

### 3.2 Context & Multi-Branch Security
* Setiap request API yang membutuhkan otorisasi cabang harus menyertakan header atau JWT claim: `X-Branch-Id: <uuid>`.
* Backend wajib memvalidasi apakah user memiliki hak akses terhadap cabang tersebut sebelum mengeksekusi transaksi.

### 3.3 Hardware Integration Standards (POS & Etiket)
* **Thermal Receipt Printer:** Dukungan protokol ESC/POS (lebar kertas 58mm atau 80mm).
* **Etiket Printer:** Templating etiket obat dalam (Putih) dan obat luar (Biru) ukuran 50x30mm atau 60x40mm.
* **Barcode Scanner:** Kompatibel dengan input keyboard wedge / USB HID tanpa lag.
