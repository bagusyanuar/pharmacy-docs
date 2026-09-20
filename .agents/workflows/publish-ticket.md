---
name: publish-ticket
description: Menerbitkan tiket tugas (Task Ticket) untuk tim Backend (BE), Frontend Web (Admin/ERP), dan Frontend POS (Kasir/Racikan) secara otomatis berdasarkan spesifikasi PRD, DRA, dan TRD dengan prinsip dekomposisi tugas atomik (anti-monolitik).
---

# Workflow: /publish-ticket

Gunakan workflow ini untuk mengonversi spesifikasi PRD, DRA, dan TRD menjadi **tiket tugas (Task Ticket) atomik** siap eksekusi bagi developer Backend atau Frontend tanpa risiko tiket raksasa (*anti-monolithic*).

---

## Langkah-Langkah Eksekusi Otomatis

### 1. Identifikasi Modul & Peran Target
Tentukan modul yang ingin diterbitkan tiketnya dan target timnya:
* **Modul:** (Contoh: `Autentikasi & Multi-Branch`, `Master Data Obat`, `POS Kasir & Resep`, `Inventory FEFO`, `Procurement SP PBF`)
* **Target Tim:**
  * `BE`: Backend (Database Relasional & REST API)
  * `FE-WEB`: Frontend Web Admin / ERP (Desktop & Backoffice)
  * `FE-POS`: Frontend POS Kasir & Resep (Keyboard-first, Fast Scan, Thermal Printer)
  * `ALL`: Terbitkan seluruh klaster peran secara paralel

### 2. Dekomposisi Tugas Menjadi Tiket Atomik (Anti-Monolithic Invariant)
**Wajib memecah skop modul** menjadi sub-task berukuran 1–3 hari kerja (1 Pull Request fokus):
* **Backend [BE]:**
  * `Part 1`: Migrasi Skema Database Relasional, Constraints, Index, & Seeding Awal
  * `Part 2`: Endpoint Alur Utama (Core Transport & DTOs, maks 2–4 endpoint)
  * `Part 3`: Middleware, Interceptor Sesi & Header Cabang
  * `Part 4`: Aksi Transaksional Khusus (Supervisor Override, Lock Concurrency)
  * `Part 5`: Background Scheduled Workers / Cron Audits
* **Frontend POS [FE-POS]:**
  * `Part 1`: Komponen Layar Utama & Input Keyboard/Numpad Listener
  * `Part 2`: Integrasi Hardware (Barcode Scanner Stream Debounce / Thermal Printer)
  * `Part 3`: Modal Interceptor Transaksional (Supervisor Override / Payment Dialog)
* **Frontend Web Admin [FE-WEB]:**
  * `Part 1`: Halaman Autentikasi, Route Guards & Global Branch Switcher
  * `Part 2`: Data Table Management & Form Pendaftaran Berjenjang

### 3. Kumpulkan Konteks Dokumen (SSOT)
Buka dan pelajari file spesifikasi yang relevan:
* **PRD:** Ambil aturan bisnis (`BR-PHARM-*`) dan kriteria penerimaan pengguna.
* **DBML / DRA:** Ambil nama tabel, foreign key `ON DELETE RESTRICT`, invarian `branch_id`, dan indeks performa.
* **TRD:** Ambil rincian spesifik endpoint REST API, skema JSON DTO, dan penanganan konkurensi.
* **Diagram:** Ambil tautan diagram sequence & flowchart alur kerja.

### 4. Susun Isi Tiket Sesuai Format Atomik
Gunakan template standar di `.github/ISSUE_TEMPLATE/`:
* [Backend Task Template](file:///.github/ISSUE_TEMPLATE/backend-task.md) untuk tiket `[BE]`.
* [Frontend Web Template](file:///.github/ISSUE_TEMPLATE/frontend-web-task.md) untuk tiket `[FE-WEB]`.
* [Frontend POS Template](file:///.github/ISSUE_TEMPLATE/frontend-pos-task.md) untuk tiket `[FE-POS]`.

Gunakan format judul terstruktur:
`[<ROLE>] <Nama Modul> (Part <N>/<Total>): <Spesifik Aksi / Cakupan Deliverable>`

### 5. Terbitkan Tiket via GitHub CLI (`gh`)
Jalankan perintah `gh issue create`:
```bash
# Contoh Backend Part 1
gh issue create \
  --title "[BE] Auth & Sesi (Part 1/5): Migrasi Skema Database Relasional, Indeks & Seeding" \
  --label "backend,auth,database" \
  --body "..."

# Contoh Frontend POS Part 1
gh issue create \
  --title "[FE-POS] Kasir Auth (Part 1/3): Layar Kunci (Lock Screen), Numpad Listener & Fast PIN Login" \
  --label "frontend-pos,auth" \
  --body "..."
```

### 6. Laporkan Matriks Tiket ke Pengguna
Tampilkan tabel rekapitulasi seluruh tiket yang berhasil diterbitkan beserta URL langsungnya agar developer dapat langsung menggunakannya sebagai acuan kerja saat koding.
