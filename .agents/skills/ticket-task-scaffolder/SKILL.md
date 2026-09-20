---
name: ticket-task-scaffolder
description: "Framework and guidelines for scaffolding lean, high-clarity, atomic task tickets (GitHub Issues) for Backend, Frontend Web Admin, and Frontend POS engineers from PRD, DRA, and TRD documentation. Enforces task decomposition to prevent monolithic tasks."
---

# Ticket Task Scaffolder Skill (Spec-Driven / Ticket-Driven Development)

Standardized framework for creating production-ready, lean, and **atomically decomposed task tickets (GitHub Issues)** from the **Pharmacy POS & ERP System** documentation for Backend, Web Admin, and POS engineers.

---

## 1. Core Principles of Ticket-Driven Development (TDD / IDD)

1. **Single Source of Truth (SSOT):**
   * Business specifications (`PRD`), database architecture (`DBML`), and API contracts (`TRD`) are the absolute sources of truth.
   * **Lean Scoping Rule:** Strictly forbidden to copy-paste 500 lines of SQL DDL or hundreds of lines of JSON payloads into the GitHub Issue body. Task tickets contain only the task objective, an actionable checklist (*Acceptance Criteria*), and **direct clickable markdown links** to reference documents in the repository.
2. **Larangan Tiket Monolitik & Prinsip Dekomposisi Atomik (*Anti-Monolithic Rule*):**
   * **DILARANG KERAS** menggabungkan seluruh fitur modul ke dalam 1 tiket raksasa (*monolithic ticket*) yang memicu *Monster PR* (> 1.000 baris kode).
   * **Ukuran Ideal 1 Tiket:** Ditargetkan selesai dalam **1–3 hari kerja developer** (menghasilkan 1 Pull Request fokus berukuran 200–400 baris kode).
   * **Prinsip "1 Tiket = 1 PR Teruji":** Setiap tiket memiliki batas pengujian yang jelas, mempermudah *code review*, dan mencegah *merge conflict*.
3. **Official Ticket Categories:**
   * `[BE]` — **Backend & Database:** Migrasi database relasional, domain business logic, FEFO stock locking, middleware isolasi cabang, dan RESTful API endpoints.
   * `[FE-WEB]` — **Frontend Web Admin/ERP:** Backoffice management dashboard, master data tables, form pendaftaran berjenjang, dan branch switcher.
   * `[FE-POS]` — **Frontend POS Cashier & Prescriptions:** Layar kasir cepat, scanner listener, pop-up supervisor override, kalkulator racikan, dan pencetakan etiket/struk ESC/POS.
   * `[QA]` — **Quality Assurance & Testing Matrix:** Skenario uji integrasi, validasi keamanan, simulasi lockout, dan pengujian multi-cabang.
4. **Traceability:** Setiap tiket tugas wajib mencantumkan tautan ke Business Rule ID (`BR-PHARM-XX-YY`) dan endpoint TRD yang diimplementasikan.
5. **Bahasa Deliverable:** Judul tiket, ringkasan tugas, checklist, dan kriteria penerimaan WAJIB ditulis dalam **Bahasa Indonesia** demi kejelasan operasional tim lokal.
6. **Standar Penamaan Database:** Seluruh nama tabel dan kolom database relasional WAJIB berbahasa **Inggris** (`snake_case`) untuk struktur teknis & atribut umum (`created_at`, `is_active`, `quantity`, `unit_price`, dll.). Khusus istilah regulasi, perizinan, dan domain khas farmasi Indonesia (seperti `sipa_number`, `strttk_number`, `sia_number`, `is_apa`, `bpjs_card_number`, `sipnap_reported_at`, `satusehat_ihs_id`, `nik`, `tuslah_amount` / `tuslah_fee`, `embalase_fee`, dan kode enum golongan BPOM), diwajibkan mempertahankan istilah baku Indonesia (prinsip *Ubiquitous Language*).

---

## 2. Aturan Dekomposisi Tugas (Decomposition & Sizing Invariants)

Setiap modul besar pada TRD **wajib dipecah** menjadi sub-task atomik berdasarkan peran:

### A. Panduan Pemecahan Sektor Backend `[BE]`
Maksimal **2–4 endpoint** per tiket BE. Pisahkan dengan pembagian klaster berikut:
1. **Pondasi Database & Migrasi:** Skema tabel relasional, foreign keys (`ON DELETE RESTRICT`), audit trail 5 kolom, unique index, dan data seeding awal.
2. **Alur Core / Transport API:** Kelompok endpoint utama (contoh: Login Web, Fast PIN POS, Refresh Token).
3. **Middleware & Sesi:** Interceptor multi-cabang `X-Branch-Id`, screen lock/unlock, rate limiting.
4. **Aksi Transaksional & Concurrency:** Endpoint berisiko tinggi / atomik (contoh: Supervisor Override + log audit, pessimistic locking pemotongan stok FEFO).
5. **Background Scheduled Workers / Crons:** Job audit berkala (contoh: cron cek masa berlaku SIPA harian, auto-flagging obat expired).

### B. Panduan Pemecahan Sektor Frontend POS Kasir `[FE-POS]`
1. **Layar Kunci & Input Cepat:** Komponen layar kunci kasir, listener keyboard numpad fisik (0-9, Enter), dan form PIN.
2. **Integrasi Hardware & Device Stream:** Penyangga stream scanner barcode USB (< 50ms interval) dan auto-lock 3 menit.
3. **Modal & Interceptor Transaksional:** Pop-up supervisor override, dialog pembayaran kasir, modal konfirmasi void.
4. **State Keranjang & Transaksi:** In-memory cart store, buffer hold/recall cart, optimistic UI update.

### C. Panduan Pemecahan Sektor Frontend Web Admin `[FE-WEB]`
1. **Autentikasi & Navigasi Global:** Form login email/password, RBAC route guards, dan komponen global Topbar Branch Switcher.
2. **Tabel Data & Manajemen:** Komponen data table, filter multi-cabang, pencarian, dan pagination server-side.
3. **Formulir Input Berjenjang:** Modal form pendaftaran multi-tahap (Data HR $\rightarrow$ Legalitas Profesi $\rightarrow$ Kredensial IAM).

---

## 3. Format Penomoran & Judul Tiket (Standard Naming)

### Format Judul:
```
[<ROLE>] <Nama Modul> (Part <N>/<Total>): <Spesifik Aksi / Cakupan Deliverable>
```

*Contoh Nyata (Modul Auth & Multi-Branch):*
* `[BE] Auth & Sesi (Part 1/5): Migrasi Skema Database Relasional, Indeks & Seeding`
* `[BE] Auth & Sesi (Part 2/5): Endpoint Web Login, Fast PIN POS & Refresh Token Rotation`
* `[BE] Auth & Sesi (Part 3/5): Middleware Isolasi X-Branch-Id & Lock/Unlock Layar POS`
* `[BE] Auth & Sesi (Part 4/5): Supervisor Override Transaksional & Audit Logging`
* `[BE] Auth & Sesi (Part 5/5): CRUD Manajemen Staf & Scheduled Worker Kedaluwarsa SIPA`
* `[FE-POS] Kasir Auth (Part 1/3): Layar Kunci (Lock Screen), Numpad Listener & Fast PIN Login`
* `[FE-POS] Kasir Auth (Part 2/3): Penyangga Barcode Scanner ID Card & Auto-Lock Inactivity 3 Menit`
* `[FE-POS] Kasir Auth (Part 3/3): Pop-Up Dialog Supervisor Override & Action Interceptor`
* `[FE-WEB] Admin Auth (Part 1/2): Halaman Login ERP, RBAC Route Guards & Global Branch Switcher`
* `[FE-WEB] Admin Auth (Part 2/2): Manajemen Pengguna & Form Pendaftaran Staf Berjenjang 3 Tahap`

### Standar Label GitHub:
* **Role:** `backend`, `frontend-web`, `frontend-pos`, `qa`
* **Domain:** `auth`, `master-data`, `pos`, `inventory`, `procurement`, `finance`
* **Type:** `feature`, `enhancement`, `database`, `security`

---

## 4. Standard Task Ticket Body Templates

### A. Backend Format (`[BE]`)
```markdown
## 📌 Ringkasan Tugas
Mengimplementasikan sub-fitur [Nama Sub-Fitur] untuk modul [Nama Modul].

## 📚 Dokumen Acuan (SSOT)
- **Business PRD:** [Nama Dokumen PRD](URL_TO_PRD#anchor)
- **Blueprint Arsitektur:** [Visual Diagrams](URL_TO_DIAGRAM#anchor)
- **Spesifikasi Teknis (TRD):** [TRD Specifications](URL_TO_TRD#anchor)
- **Skema Database:** [DBML Schema](URL_TO_DBML#anchor)

## 🎯 Cakupan Tugas & Checklist Deliverable
- [ ] Implementasi skema tabel / migration file sesuai standar universal audit 5 kolom.
- [ ] Implementasi Domain Service & Repository layer dengan penanganan transaksi ACID.
- [ ] Implementasi Controller / Transport endpoint dengan validasi DTO dan envelope standar.
- [ ] Unit test & Integration test dengan target coverage > 80%.

## 🛡️ Business Rules & Invarian Teknis
- [ ] `BR-PHARM-XX-YY`: ...
- [ ] Invarian Multi-Cabang: Wajib memvalidasi header `X-Branch-Id: <uuid>`.
- [ ] Invarian Konkurensi: Menjamin integritas data tanpa race condition.
```

### B. Frontend POS Cashier Format (`[FE-POS]`)
```markdown
## 📌 Ringkasan Tugas
Membangun antarmuka kasir depan untuk [Nama Sub-Fitur] dengan navigasi keyboard-first dan integrasi hardware.

## 📚 Dokumen Acuan (SSOT)
- **Business PRD:** [Nama Dokumen PRD](URL_TO_PRD#anchor)
- **Spesifikasi Teknis (TRD):** [TRD Specifications](URL_TO_TRD#anchor)

## 🎯 Cakupan Tugas & Checklist Deliverable
- [ ] Komponen antarmuka visual (desain responsif PC Desktop kasir & Tablet sentuh 10-12 inci).
- [ ] Event listener keyboard / hardware buffer stream.
- [ ] Integrasi kontrak API TRD (penanganan status loading, sukses, dan error feedback).
- [ ] Pengujian manual alur kasir (< 100ms response feedback).

## 🛡️ Business Rules & Invarian Teknis
- [ ] `BR-PHARM-XX-YY`: ...
```

### C. Frontend Web Admin / ERP Format (`[FE-WEB]`)
```markdown
## 📌 Ringkasan Tugas
Membangun antarmuka manajemen backoffice untuk [Nama Sub-Fitur].

## 📚 Dokumen Acuan (SSOT)
- **Business PRD:** [Nama Dokumen PRD](URL_TO_PRD#anchor)
- **Spesifikasi Teknis (TRD):** [TRD Specifications](URL_TO_TRD#anchor)

## 🎯 Cakupan Tugas & Checklist Deliverable
- [ ] Navigasi halaman & proteksi hak akses berbasis RBAC.
- [ ] Komponen form input / data table dengan validasi skema.
- [ ] Integrasi kontrak API TRD dengan penyuntikan header `X-Branch-Id`.
- [ ] Pengujian skenario form validasi dan respon error API.

## 🛡️ Business Rules & Invarian Teknis
- [ ] `BR-PHARM-XX-YY`: ...
```
