---
name: issue-task-scaffolder
description: "Framework and guidelines for scaffolding lean, high-clarity GitHub Issues for Backend, Frontend Web Admin, and Frontend POS engineers from PRD, DRA, and TRD documentation."
---

# Issue Task Scaffolder Skill (Spec-Driven / Issue-Driven Development)

Standardized framework for creating production-ready, lean **GitHub Issues** from the **Pharmacy POS & ERP System** documentation for Backend, Web Admin, and POS engineers.

---

## 1. Core Principles of Issue-Driven Development (IDD)

1. **Single Source of Truth (SSOT):**
   * Spesifikasi bisnis (`PRD`), arsitektur database (`DRA`), dan kontrak API (`TRD`) adalah sumber kebenaran mutlak.
   * **Lean Scoping Rule:** Dilarang meng-copy-paste 500 baris DDL SQL atau ratusan baris payload JSON ke dalam body GitHub Issue. Tiket issue hanya berisi tujuan tugas, checklist pengerjaan (*Acceptance Criteria*), dan **tautan Markdown langsung (*clickable link*)** ke dokumen rujukan di repositori.
2. **Kategori Tiket Resmi:**
   * `[BE]` — **Backend & Database:** Migrasi PostgreSQL, domain logic, FEFO stock locking, REST API endpoints.
   * `[FE-WEB]` — **Frontend Web Admin/ERP:** Dashboard manajemen, master data, gudang pusat, pengadaan PBF, laporan keuangan.
   * `[FE-POS]` — **Frontend POS Kasir & Resep:** Kasir cepat keyboard-first, scan barcode, kalkulator racikan, etiket & receipt printer thermal.
3. **Traceability:** Setiap issue harus mencantumkan referensi aturan bisnis (`BR-PHARM-XX-YY`) yang diimplementasikan.

---

## 2. Format Penamaan & Labeling Standar

### Format Judul Tiket:
```
[<ROLE>] <Nama Modul>: <Aksi Spesifik / Scope Tugas>
```
*Contoh:*
* `[BE] Master Data Obat: Migrasi Schema & REST API CRUD Multi-Satuan`
* `[FE-POS] Kasir OTC: Keyboard-First Checkout, Scan Barcode & Thermal Print`
* `[FE-WEB] Pengadaan PBF: Antarmuka Defekta Digital & Generator Surat Pesanan (SP)`

### Standar Labeling:
* **Role:** `backend`, `frontend-web`, `frontend-pos`
* **Domain:** `master-data`, `pos`, `inventory`, `procurement`, `finance`, `compliance`
* **Tipe:** `feature`, `enhancement`, `bug`

---

## 3. Template Body Tiket Standar

### A. Format Backend (`[BE]`)
```markdown
## 📌 Ringkasan Tugas
Implementasi skema database PostgreSQL dan REST API untuk [Nama Modul].

## 📚 Dokumen Acuan (SSOT)
- **Business PRD:** [Nama Modul PRD](URL_KE_PRD)
- **Database Architecture (DRA):** [DRA Spesifikasi](URL_KE_DRA#anchor)
- **API Contracts (TRD):** [TRD Spesifikasi](URL_KE_TRD#anchor)

## 🎯 Lingkup Pengerjaan (Scope & Checklist)
- [ ] Buat file migration tabel PostgreSQL sesuai DRA (pastikan kolom universal audit & `branch_id`).
- [ ] Implementasi Service & Repository dengan penanganan transaksi atomic (`DB Transaction`).
- [ ] Implementasi Controller & Validasi DTO sesuai envelope standar TRD.
- [ ] Unit test & API integration test (Coverage > 80%).

## 🛡️ Aturan Bisnis yang Wajib Dipenuhi
- [ ] `BR-PHARM-XX-01`: ...
- [ ] `BR-PHARM-XX-02`: ...
```

### B. Format Frontend POS Kasir (`[FE-POS]`)
```markdown
## 📌 Ringkasan Tugas
Bangun antarmuka kasir cepat untuk [Nama Modul] dengan navigasi keyboard-first dan integrasi thermal printer.

## 📚 Dokumen Acuan (SSOT)
- **Business PRD:** [Nama Modul PRD](URL_KE_PRD)
- **API Contracts (TRD):** [TRD Spesifikasi](URL_KE_TRD#anchor)

## 🎯 Lingkup Pengerjaan (Scope & Checklist)
- [ ] Komponen antarmuka pencarian cepat (barcode & nama paten/generik).
- [ ] Shortcut keyboard (F1 - F12, Enter, Escape).
- [ ] Integrasi cetak struk kasir & cetak etiket thermal (ESC/POS).
- [ ] Penanganan state keranjang belanja (hold/recall cart).
```

### C. Format Frontend Web Admin / ERP (`[FE-WEB]`)
```markdown
## 📌 Ringkasan Tugas
Bangun antarmuka web backoffice untuk pengelolaan [Nama Modul].

## 📚 Dokumen Acuan (SSOT)
- **Business PRD:** [Nama Modul PRD](URL_KE_PRD)
- **API Contracts (TRD):** [TRD Spesifikasi](URL_KE_TRD#anchor)

## 🎯 Lingkup Pengerjaan (Scope & Checklist)
- [ ] Antarmuka tabel data (sorting, pagination, filtering per cabang).
- [ ] Form input & validasi client-side.
- [ ] Export laporan (PDF & Excel).
```
