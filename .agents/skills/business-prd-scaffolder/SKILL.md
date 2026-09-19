---
name: business-prd-scaffolder
description: "Framework and guidelines for scaffolding Business-Centric Product Requirements Documents (PRDs) for the Pharmacy POS & ERP System. Focuses strictly on business logic, pharmacy operations, workflows, and domain rules (FEFO, dispensing, compounding/racikan, regulatory compliance), while preventing low-level technical/engineering leakages."
---

# Business-Centric PRD Scaffolder Skill (Pharmacy POS & ERP)

Standardized framework for creating high-impact, business-centric Product Requirements Documents (PRDs) for the **Pharmacy POS & ERP System (Apotek)**.

---

## 1. Core Philosophy: "WHAT & WHY", Not "HOW"

A **Product Requirements Document (PRD)** is written for business stakeholders, pharmacy owners, Apoteker Pengelola Apotek (APA), Tenaga Teknis Kefarmasian (TTK), kasir, product managers, and UI/UX designers.
Technical implementation details belong exclusively in a **Technical Requirements Document (TRD)** or **Data Requirements Architecture (DRA)** inside `technical/`.

### 🚫 Strictly Prohibited in PRDs (Dilarang Masuk ke PRD):
* ❌ **API Endpoints & HTTP Methods:** Dilarang mencantumkan tabel endpoint (misal: `POST /api/v1/pos/checkout`, `GET /branches`).
* ❌ **JSON Payloads & DTOs:** Dilarang mencantumkan request/response JSON payload.
* ❌ **Database Tables, DDL & Foreign Keys:** Dilarang menyebutkan nama tabel SQL (`sales`, `product_stocks`), tipe data SQL (`UUID`, `VARCHAR`, `DECIMAL`), primary key, atau constraint database.
* ❌ **Cryptographic / Technical Algorithms:** Dilarang menyebut algoritma enkripsi (misal: `Argon2id`, `bcrypt`, `JWT RS256`).
* ❌ **Code Snippets & Libraries:** Dilarang menuliskan bahasa pemrograman, ORM, atau library teknis.

### ✅ Mandatory Focus in PRDs (Wajib Ada di PRD):
* ✅ **Latar Belakang & Nilai Bisnis (Business Value):** Mengapa fitur ini penting bagi efisiensi apotek, perlindungan margin kas, pencegahan obat expired, dan kepatuhan regulasi BPOM/Kemenkes?
* ✅ **Konteks & Lingkungan Kerja Pengguna (User Context):** Kasir di meja depan dengan antrean pasien, asisten apoteker di meja racik menghitung dosis, staf gudang mengecek batch faktur PBF, atau apoteker memvalidasi resep.
* ✅ **Alur Pengguna & Interaksi (User Journey & Experience):** Langkah demi langkah dari sudut pandang apa yang dilihat dan dilakukan staf apotek di antarmuka.
* ✅ **Aturan Bisnis Tegas (Business Rules - `BR-PHARM-XX-YY`):** Kebijakan operasional (misal: aturan FEFO, batas minimum reorder point, validasi obat keras harus ada nomor SIP dokter, batas diskon kasir, mekanisme PIN otorisasi supervisor).
* ✅ **Skenario Khusus / Pengecualian (Edge Cases):** Apa yang terjadi jika stok obat racikan kurang di tengah jalan, nomor batch fisik berbeda dengan faktur PBF, atau kasir mendapati uang fisik selisih saat tutup shift?
* ✅ **Kriteria Penerimaan (Acceptance Criteria):** Checklist fungsional dari kacamata pengguna/QA untuk validasi hasil bisnis.

---

## 2. Tabel Komparasi: PRD (Bisnis) vs TRD/DRA (Teknis)

| Aspek | Penulisan yang Benar di PRD (Murni Bisnis) | Penulisan di TRD / DRA (Teknis) |
| :--- | :--- | :--- |
| **Identitas Cabang** | "Sistem mengikat sesi kasir ke Cabang Utama dan memastikan seluruh transaksi memotong stok fisik di cabang tersebut." | `branch_id UUID NOT NULL REFERENCES branches(id)`, Header: `X-Branch-Id: <uuid>` |
| **Pengambilan Stok** | "Obat yang keluar kasir selalu otomatis mengambil nomor batch yang tanggal kedaluwarsanya paling dekat (FEFO)." | `CREATE INDEX idx_stocks_fefo ON product_stocks (branch_id, product_id, expired_date ASC)` |
| **Login Kasir** | "Kasir dapat login instan menggunakan PIN 6 digit atau scan barcode pada kartu identitas staf untuk mempercepat antrean." | Dual-UX endpoint `POST /api/v1/auth/login-pin`, hashing Argon2id, JWT claim `branch_id` |
| **Persetujuan Void** | "Pembatalan item transaksi yang sudah ter-scan wajib memasukkan PIN otorisasi Apoteker / Supervisor yang bertugas." | Endpoint `POST /api/v1/pos/override-approval` dengan validasi role `SUPERVISOR` atau `APOTEKER` |
| **Spesifikasi Input Form** | "Nomor SIPA Apoteker: Kolom teks wajib diisi, memuat nomor izin resmi apoteker beserta tanggal masa berlaku izin." | `sipa_number VARCHAR(100) NOT NULL`, `sipa_expired_date DATE NOT NULL` |

---

## 3. Standard Template Structure for Feature PRDs

Every feature PRD inside `features/` MUST adhere strictly to this structure:

```markdown
# Feature PRD: [Nama Fitur / Modul Apotek]

## 1. Metadata Dokumen
- **Kode Dokumen:** PRD-PHARM-XX
- **Nama Modul:** [Nama Modul]
- **Dokumen Induk:** 00-MASTER-PRD.md
- **Depends On (Prasyarat):** [Daftar PRD/Modul yang menjadi dependensi modul ini]
- **Consumed By (Dampak):** [Daftar PRD/Modul yang memanfaatkan/terdampak oleh modul ini]
- **Target Pengguna:** [Kasir, Asisten Apoteker / TTK, Apoteker Pengelola Apotek (APA), Bagian Pengadaan, Owner]

## 2. Latar Belakang & Masalah Bisnis
- Mengapa fitur ini dibutuhkan?
- Masalah nyata apa yang diselesaikan di apotek (misal: antrean lambat, salah ambil obat beda batch, kerugian obat expired)?
- Manfaat bagi keuangan apotek dan kepatuhan regulasi kefarmasian.

## 3. Persona & Konteks Penggunaan
- Siapa penggunanya?
- Bagaimana situasi penggunaannya? (Meja kasir layar sentuh / keyboard barcode, meja peracikan, gudang penerimaan PBF).

## 4. Alur Kerja Utama (Core User Journey)
- Visual diagram alur proses kerja (Mermaid flowchart / user journey).
- Narasi langkah demi langkah dari awal hingga selesai dari kacamata staf apotek.

## 5. Kebutuhan Fungsional & Aturan Bisnis (Business Rules)
Gunakan kodefikasi standar: `BR-PHARM-[KODE_MODUL]-[NOMOR]` (contoh: `BR-PHARM-AUTH-01`, `BR-PHARM-POS-03`).
- **BR-PHARM-XX-01:** [Deskripsi aturan bisnis yang tegas dan tidak ambigu]
- **BR-PHARM-XX-02:** [Deskripsi aturan validasi atau penghitungan bisnis]

## 6. Elemen Antarmuka & Input Bisnis (UI & Information Elements)
- Daftar informasi yang ditampilkan ke pengguna (tanpa menyebut tipe data database).
- Data masukan (formulir) dan aksi yang dapat dipicu pengguna.

## 7. Skenario Pengecualian & Kasus Khusus (Edge Cases)
- Penanganan saat kondisi tidak normal (stok habis di tengah racik, kasir salah ketik, koneksi drop sesaat).

## 8. Metrik Keberhasilan Bisnis (KPI)
- Dampak langsung terhadap efisiensi operasional apotek (misal: waktu checkout < 30 detik, zero salah dispensing batch).

## 9. Kriteria Penerimaan (Acceptance Criteria)
Format: Given [kondisi awal], When [aksi pengguna], Then [ekspektasi hasil bisnis].
- **AC-01:** ...
- **AC-02:** ...
```
