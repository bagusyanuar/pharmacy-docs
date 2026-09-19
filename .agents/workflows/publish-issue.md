---
name: publish-issue
description: Menerbitkan tiket tugas GitHub Issue untuk tim Backend (BE), Frontend Web (Admin/ERP), dan Frontend POS (Kasir/Racikan) secara otomatis berdasarkan spesifikasi PRD, DRA, dan TRD.
---

# Workflow: /publish-issue

Gunakan workflow ini untuk mengonversi spesifikasi PRD, DRA, dan TRD menjadi tiket **GitHub Issues** siap eksekusi bagi developer Backend atau Frontend.

---

## Langkah-Langkah Eksekusi Otomatis

### 1. Identifikasi Modul & Peran Target
Tentukan modul yang ingin diterbitkan tiketnya dan target timnya:
* **Modul:** (Contoh: `Master Data Obat`, `POS Kasir & Resep`, `Inventory FEFO`, `Procurement SP PBF`, `Laporan SIPNAP`)
* **Target Tim:**
  * `BE`: Backend (Database PostgreSQL & REST API)
  * `FE-WEB`: Frontend Web Admin / ERP (React/Next.js/Vue Desktop)
  * `FE-POS`: Frontend POS Kasir & Resep (Keyboard-first, Fast Scan, Thermal Printer)
  * `ALL`: Terbitkan ketiga peran sekaligus

### 2. Kumpulkan Konteks Dokumen
Baca file spesifikasi yang relevan:
* **PRD:** Ambil aturan bisnis (`BR-PHARM-*`) dan alur pengguna.
* **DRA:** Ambil nama tabel, relasi foreign key, batasan multi-cabang `branch_id`, dan skema migrasi PostgreSQL.
* **TRD:** Ambil rincian endpoint REST API, payload request/response JSON, dan penanganan batch/FEFO.

### 3. Susun Isi Tiket Sesuai Template
Gunakan template standar di `.github/ISSUE_TEMPLATE/`:
* [Backend Task Template](file:///Users/dystopia/projects/pharmacy/pharmacy-docs/.github/ISSUE_TEMPLATE/backend-task.md) untuk tiket `[BE]`.
* [Frontend Web Template](file:///Users/dystopia/projects/pharmacy/pharmacy-docs/.github/ISSUE_TEMPLATE/frontend-web-task.md) untuk tiket `[FE-WEB]`.
* [Frontend POS Template](file:///Users/dystopia/projects/pharmacy/pharmacy-docs/.github/ISSUE_TEMPLATE/frontend-pos-task.md) untuk tiket `[FE-POS]`.

Pastikan seluruh link mengarah ke URL GitHub repositori saat ini:
`https://github.com/bagusyanuar/pharmacy-docs/blob/main/...` (atau branch aktif).

### 4. Terbitkan Tiket via GitHub CLI (`gh`)
Jalankan perintah `gh issue create`:
```bash
# Contoh Backend
gh issue create \
  --title "[BE] <Nama Modul>: <Tujuan>" \
  --label "backend,<domain>" \
  --body "..."

# Contoh Frontend POS
gh issue create \
  --title "[FE-POS] <Nama Modul>: <Tujuan>" \
  --label "frontend-pos,<domain>" \
  --body "..."
```

### 5. Laporkan Tautan Tiket ke Pengguna
Tampilkan URL tiket yang berhasil diterbitkan agar pengguna atau developer lain bisa langsung mengkliknya dan menggunakannya sebagai acuan kerja saat koding.
