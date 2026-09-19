---
name: business-prd-scaffolder
description: "Framework and guidelines for scaffolding Business-Centric Product Requirements Documents (PRDs) for the Pharmacy POS & ERP System. Focuses strictly on business logic, pharmacy operations, workflows, and domain rules (FEFO, dispensing, compounding/racikan, regulatory compliance), while preventing low-level technical/engineering leakages."
---

# Business-Centric PRD Scaffolder Skill (Pharmacy POS & ERP)

Standardized framework for creating high-impact, business-centric Product Requirements Documents (PRDs) for the **Pharmacy POS & ERP System (Apotek)**.

## Core Philosophy: "WHAT & WHY", Not "HOW"

A **Product Requirements Document (PRD)** is written for business stakeholders, pharmacy owners, Apoteker Pengelola Apotek (APA), Tenaga Teknis Kefarmasian (TTK), kasir, product managers, and UI/UX designers.
Technical implementation details belong in a **Technical Requirements Document (TRD)** or **Data Requirements Architecture (DRA)**.

### Strictly Prohibited in PRDs:
* ❌ Raw JSON payloads or API request/response samples.
* ❌ Low-level cryptographic algorithms or database queries (`SELECT`, `INSERT`, indexes, FK).
* ❌ Database-specific technical schemas (e.g., `UUIDv4`, `DECIMAL(12,2)`).
* ❌ API endpoint tables (e.g., `POST /api/v1/...`).
* ❌ Code snippets or programming language specific libraries.

### Mandatory Focus in PRDs:
* ✅ **Latar Belakang & Nilai Bisnis (Business Value):** Mengapa fitur ini penting bagi efisiensi apotek, pencegahan kerugian obat expired, dan kepatuhan hukum BPOM/Kemenkes?
* ✅ **Konteks & Lingkungan Kerja Pengguna (User Context):** Kasir di meja depan dengan antrean pasien, asisten apoteker di meja racik menghitung dosis, staf gudang mengecek batch faktur PBF, atau apoteker memvalidasi resep.
* ✅ **Alur Pengguna & Interaksi (User Journey & Experience):** Langkah demi langkah dari sudut pandang apa yang dilihat dan dilakukan staf apotek.
* ✅ **Aturan Bisnis Tegas (Business Rules - `BR-PHARM-XX-YY`):** Kebijakan operasional (misal: aturan FEFO, batas minimum reorder point, validasi obat keras harus ada resep, pemotongan tuslah & embalase).
* ✅ **Skenario Khusus / Pengecualian (Edge Cases):** Apa yang terjadi jika stok obat racikan kurang di tengah jalan, nomor batch fisik berbeda dengan faktur PBF, atau kasir mendapati uang fisik selisih saat tutup shift?
* ✅ **Kriteria Penerimaan (Acceptance Criteria):** Berdasarkan hasil fungsional yang dapat diuji oleh Apoteker / QA.

---

## Standard Template Structure for Feature PRDs

Every feature PRD inside `features/` MUST adhere to this structure:

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
- Narasi langkah demi langkah dari awal hingga selesai.

## 5. Kebutuhan Fungsional & Aturan Bisnis (Business Rules)
Gunakan kodefikasi standar: `BR-PHARM-[KODE_MODUL]-[NOMOR]` (contoh: `BR-PHARM-POS-01`, `BR-PHARM-WMS-03`).
- **BR-PHARM-XX-01:** [Deskripsi aturan bisnis yang tegas dan tidak ambigu]
- **BR-PHARM-XX-02:** [Deskripsi aturan validasi atau penghitungan bisnis]

## 6. Skenario Pengecualian & Kasus Khusus (Edge Cases)
- Penanganan saat stok fisik tidak sesuai sistem.
- Penanganan retur barang atau pembatalan transaksi resep yang sudah diracik.
- Penanganan listrik padam / koneksi internet drop saat kasir aktif.

## 7. Kriteria Penerimaan (Acceptance Criteria)
Format: Given [kondisi awal], When [aksi pengguna], Then [ekspektasi hasil bisnis].
- **AC-01:** ...
- **AC-02:** ...
```
