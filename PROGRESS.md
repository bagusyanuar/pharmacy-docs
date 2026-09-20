# Status Progres Pengerjaan Dokumentasi & Arsitektur
# Pharmacy POS & ERP System (Apotek)

Dokumen ini adalah rekam jejak resmi status pengerjaan spesifikasi teknis, arsitektur data, dan progres implementasi sistem apotek terpadu.

---

## 📌 Ringkasan Status Proyek

* **Tanggal Pembaruan Terakhir:** 2026-09-20
* **Milestone Aktif Saat Ini:** **Fase 1 (Master Data, Multi-Branch Architecture & Skema Database DRA) — SEDANG BERJALAN**
* **Milestone Berikutnya:** **Fase 2 (Front-Office POS, Kasir Cepat & Resep Racikan)**
* **Integritas Knowledge Graph (Graphify):** Tersinkronisasi

---

## ✅ Deliverables yang Sudah Selesai (Completed)

### 1. Tata Kelola & Aturan Arsitektur (`.agents/` & `.github/`)
- [x] [`.agents/rules/persona-tika.md`](./.agents/rules/persona-tika.md) — Persona Tika (Lead System Analyst & Technical PM Apotek).
- [x] [`.agents/rules/graphify.md`](./.agents/rules/graphify.md) — Aturan hemat & efisien pembaruan graf (hanya saat checkpoint/save-progress).
- [x] [`.agents/workflows/save-progress.md`](./.agents/workflows/save-progress.md) — Workflow checkpoint otomatis akhir sesi (`/save-progress`).
- [x] [`.agents/workflows/graphify.md`](./.agents/workflows/graphify.md) — Workflow navigasi & sinkronisasi graf pengetahuan (`/graphify`).
- [x] [`.agents/workflows/publish-issue.md`](./.agents/workflows/publish-issue.md) — Workflow penerbitan tiket tugas GitHub Issue via `gh` CLI (`/publish-issue`).
- [x] [`.agents/skills/business-prd-scaffolder/SKILL.md`](./.agents/skills/business-prd-scaffolder/SKILL.md) — Framework PRD murni bisnis apotek ("WHAT & WHY", no engineering leaks, mandatory `Depends On` / `Consumed By`).
- [x] [`.agents/skills/trd-dra-scaffolder/SKILL.md`](./.agents/skills/trd-dra-scaffolder/SKILL.md) — Framework DRA PostgreSQL 15+, universal audit 5 kolom, invarian `branch_id`, presisi `DECIMAL`, dan envelope REST API.
- [x] [`.agents/skills/change-impact-synchronizer/SKILL.md`](./.agents/skills/change-impact-synchronizer/SKILL.md) — SOP 4-langkah Zero Documentation Drift saat terjadi revisi modul farmasi.
- [x] [`.agents/skills/issue-task-scaffolder/SKILL.md`](./.agents/skills/issue-task-scaffolder/SKILL.md) — Framework perakitan tiket tugas GitHub Issue untuk tim `[BE]`, `[FE-WEB]`, dan `[FE-POS]`.
- [x] [`.github/ISSUE_TEMPLATE/backend-task.md`](./.github/ISSUE_TEMPLATE/backend-task.md) — Template GitHub Issue untuk tugas Backend.
- [x] [`.github/ISSUE_TEMPLATE/frontend-web-task.md`](./.github/ISSUE_TEMPLATE/frontend-web-task.md) — Template GitHub Issue untuk Web Admin/ERP (React/Next.js/Vue Desktop).
- [x] [`.github/ISSUE_TEMPLATE/frontend-pos-task.md`](./.github/ISSUE_TEMPLATE/frontend-pos-task.md) — Template GitHub Issue untuk Kasir POS & Resep (Keyboard-first, barcode scan, thermal printer).

### 2. Dokumen Induk & Fondasi Identitas
- [x] [`README.md`](./README.md) — Hub dokumentasi, navigasi modul, RBAC matriks, panduan Graphify, dan status pengerjaan.
- [x] [`00-MASTER-PRD.md`](./00-MASTER-PRD.md) — Master PRD sistem (5 pilar, arsitektur modul, NFR, roadmap).
- [x] [`FEATURE-CHECKLIST.md`](./FEATURE-CHECKLIST.md) — Master Feature & Implementation Checklist lintas platform (Docs, BE, Web Admin, POS Kasir).
- [x] [`technical/00-architecture-and-multibranch-guidelines.md`](./technical/00-architecture-and-multibranch-guidelines.md) — Fondasi arsitektur multi-cabang ready, FEFO indexing, audit trail universal, dan panduan migrasi.

### 3. Modul Spesifikasi Kebutuhan Bisnis (PRD)
- [x] [`features/01-prd-auth-user.md`](./features/01-prd-auth-user.md) — Dual-UX Login (Fast PIN POS & Email/Password ERP), profil staf & legalitas profesi (SIPA Apoteker & STRTTK), matriks hak akses 6 persona (RBAC), pop-up otorisasi supervisor (*Manager Override* untuk void/diskon), dan isolasi sesi cabang.
- [x] [`features/master-data/01-prd-cabang-dan-gudang.md`](./features/master-data/01-prd-cabang-dan-gudang.md) — Master Cabang, klasifikasi tipe cabang (Apotek Ritel/Klinik/Gudang Pusat), izin resmi SIA, prinsip 1 Apotek 1 APA, penugasan staf kasir, zona simpan baku & penomoran rak fisik.
- [x] [`features/master-data/02-prd-obat-dan-satuan-bertingkat.md`](./features/master-data/02-prd-obat-dan-satuan-bertingkat.md) — Master Obat & Zat Aktif, Penggolongan Regulasi Farmasi (Bebas/Terbatas/Keras/OWA/Psiko/Narko), Hierarki Multi-Satuan Bertingkat (Base Unit, Sub, Outer), Multi-Barcode per kemasan, Seamless Auto-Breakdown di POS dengan pencatatan ganda kartu stok (`AUTO_BREAKDOWN` & `DISPENSE_SALE`), Wizard Konversi Satuan Terkecil, Valuasi HPP Bersih (termasuk bonus barang PBF & diskon bertingkat), dan Dual-View Display stok ramah manusia.
- [x] [`features/master-data/03-prd-pbf-supplier-dan-dokter.md`](./features/master-data/03-prd-pbf-supplier-dan-dokter.md) — Master Pedagang Besar Farmasi (PBF), izin operasional PBF, sertifikasi CDOB (Cold Chain/Psikotropika), rekening bank terverifikasi, syarat pembayaran (TOP) & batas plafon kredit, direktori dokter perujuk (validasi masa aktif SIP), serta Patient Medication Record (PMR) dengan pendekatan fleksibel 4-tingkat di meja kasir (Walk-in umum anonim < 15 detik, Quick-Tag resep sekali jalan, registrasi regulasi SIPNAP, dan Full PMR dengan alert otomatis alergi zat aktif).

---

## 🎯 Antrean Pengerjaan Berikutnya (Next Action Items: Fase 1)

Berikutnya kita akan menyusun **Spesifikasi Arsitektur Teknis (DRA & TRD)** untuk mengunci seluruh fondasi Master Data & Autentikasi:

1. ⏳ **`technical/01-dra-database-erd-master-auth.md`**
   * *Cakupan Teknis:* Skema DDL PostgreSQL 15+ (tabel cabang, user, profil SIPA, obat, multi-satuan, PBF, dokter, pasien/PMR), Foreign Key `ON DELETE RESTRICT`, invarian `branch_id`, indeks performa FEFO, dan diagram Mermaid ERD komprehensif.
2. ⏳ **`technical/02-trd-auth-session-api.md`**
   * *Cakupan Teknis:* Kontrak REST API Autentikasi Dual-UX (PIN & Email), JWT RS256, Refresh Token Rotation, dan validasi header `X-Branch-Id`.
3. ⏳ **`technical/03-trd-master-data-api.md`**
   * *Cakupan Teknis:* Kontrak RESTful API CRUD untuk seluruh modul Master Data dengan envelope standar `{ success, data, meta, error }`.

---

## 🔑 Aturan Penting yang Wajib Dipertahankan (Invariants)

1. **Format PRD Bisnis Murni:** Tidak boleh ada JSON schema, kode hash enkripsi, query database, atau tabel endpoint API di dalam folder `features/`. PRD hanya memuat alur kerja pengguna, aturan bisnis (`BR-PHARM-*`), dan kriteria penerimaan.
2. **Metadata Ketergantungan Wajib:** Setiap dokumen PRD & TRD harus memiliki baris metadata `Depends On (Prasyarat)` dan `Consumed By (Dampak)`.
3. **Standar Multi-Branch Ready:** Semua tabel data transaksi, stok fisik, dan mutasi WAJIB menyertakan kolom `branch_id UUID NOT NULL REFERENCES branches(id)`.
4. **Standar DRA Database:** PostgreSQL 15+, UUID v4, 5 kolom audit universal (`id`, `created_at`, `updated_at`, `created_by`, `deleted_at`), `ON DELETE RESTRICT`, `DECIMAL(12,2)` untuk finansial dan `DECIMAL(10,3)` untuk dosis racikan.
5. **Scoping Pemisahan:** DRA diglobalkan per milestone, TRD di-split modular per klaster domain API.
6. **Lean Scoping GitHub Issues:** Tiket issue murni memuat checklist tugas dan tautan acuan dokumen SSOT, DILARANG menduplikasi DDL SQL atau payload JSON di body issue.

---

## 💡 Cara Memulai Kembali Sesi (Resume Prompt)

Saat Anda membuka sesi berikutnya, cukup ketik pesan singkat berikut:

> *"Halo Tika, tolong baca `PROGRESS.md` dan kita lanjutkan pengerjaan ke Fase 1: `technical/01-dra-database-erd-master-auth.md`."*
