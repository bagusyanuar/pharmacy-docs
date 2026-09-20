# Graph Report - pharmacy-docs  (2026-09-20)

## Corpus Check
- 21 files · ~26,465 words
- Verdict: corpus is large enough that graph structure adds value.
- Unclassified: 1 file(s) not represented in the graph (top: (none) 1)

## Summary
- 256 nodes · 268 edges · 21 communities
- Extraction: 100% EXTRACTED · 0% INFERRED · 0% AMBIGUOUS
- Token cost: 0 input · 0 output

## Graph Freshness
- Built from commit: `c929da0e`
- Run `git rev-parse HEAD` and compare to check if the graph is stale.
- Run `graphify update .` after code changes (no API cost).

## Community Hubs (Navigation)
- Feature PRD: Autentikasi, Akun Pengguna, Legalitas Profesi Farmasi & Hak Akses (RBAC)
- Feature PRD: Master Cabang, Izin Operasional Apotek (SIA) & Struktur Gudang
- Pharmacy POS & ERP System
- Pharmacy POS & ERP System (Dokumentasi & Spesifikasi Arsitektur)
- 2. Invariants & Standards for DRA (Database Architecture & ERD)
- PROGRESS.md
- 3. Template Body Tiket Standar
- 🏷️ Rincian Fitur per Fase
- Langkah-Langkah Eksekusi Otomatis
- Pharmacy POS & ERP System (Apotek)
- 2. Standard 4-Step Cascade Update Workflow
- Langkah-Langkah Eksekusi Otomatis
- Pharmacy Management System: POS & ERP Apotek
- Business-Centric PRD Scaffolder Skill (Pharmacy POS & ERP)
- Feature PRD: Master Obat, Penggolongan Regulasi Farmasi & Multi-Satuan Bertingkat
- backend-task.md
- frontend-pos-task.md
- Persona: Tika — Lead System Analyst & Technical Project Manager
- frontend-web-task.md
- Feature PRD: Master Distributor (PBF), Dokter Perujuk & Rekam Pengobatan Pasien (PMR)
- 9. Kriteria Penerimaan (Acceptance Criteria)

## God Nodes (most connected - your core abstractions)
1. `Feature PRD: Autentikasi, Akun Pengguna, Legalitas Profesi Farmasi & Hak Akses (RBAC)` - 10 edges
2. `Feature PRD: Master Cabang, Izin Operasional Apotek (SIA) & Struktur Gudang` - 10 edges
3. `Feature PRD: Master Obat, Penggolongan Regulasi Farmasi & Multi-Satuan Bertingkat` - 10 edges
4. `Feature PRD: Master Distributor (PBF), Dokter Perujuk & Rekam Pengobatan Pasien (PMR)` - 10 edges
5. `Pharmacy POS & ERP System` - 8 edges
6. `Langkah-Langkah Eksekusi Otomatis` - 7 edges
7. `Pharmacy POS & ERP System (Dokumentasi & Spesifikasi Arsitektur)` - 7 edges
8. `5. Kebutuhan Fungsional & Aturan Bisnis (Business Rules)` - 7 edges
9. `5. Kebutuhan Fungsional & Aturan Bisnis (Business Rules)` - 7 edges
10. `9. Kriteria Penerimaan (Acceptance Criteria)` - 7 edges

## Surprising Connections (you probably didn't know these)
- None detected - all connections are within the same source files.

## Communities (21 total, 0 thin omitted)

### Community 0 - "Feature PRD: Autentikasi, Akun Pengguna, Legalitas Profesi Farmasi & Hak Akses (RBAC)"
Cohesion: 0.08
Nodes (26): 1. Metadata Dokumen, 2. Latar Belakang & Masalah Bisnis, 3. Persona & Konteks Penggunaan, 4.1 Alur 1: Login Cepat Kasir di Meja Depan (POS Fast PIN), 4.2 Alur 2: Otorisasi Supervisor / Apoteker (*Manager Override*) di Meja Kasir, 4.3 Alur 3: Manajemen Profil Legalitas Profesi & Pemantauan Masa Berlaku SIPA, 4. Alur Kerja Utama (Core User Journey), 5.1 Dual-UX Autentikasi & Keamanan Sesi (+18 more)

### Community 1 - "Feature PRD: Master Cabang, Izin Operasional Apotek (SIA) & Struktur Gudang"
Cohesion: 0.08
Nodes (26): 1. Metadata Dokumen, 2. Latar Belakang & Masalah Bisnis, 3. Persona & Konteks Penggunaan, 4.1 Alur 1: Pembukaan Cabang Baru & Penetapan Legalitas SIA, 4.2 Alur 2: Penugasan Staf & Aktivasi Komputer Kasir Cabang, 4. Alur Kerja Utama (Core User Journey), 5.1 Struktur & Tipe Cabang, 5.2 Legalitas Perizinan Apotek (SIA) (+18 more)

### Community 2 - "Pharmacy POS & ERP System"
Cohesion: 0.11
Nodes (19): 1.1 Masalah Klasik Migrasi Apotek, 1.2 Strategi Solusi: Multi-Branch Ready Sejak Hari Pertama (Day 1), 1. Filosofi: "Single-Branch in Mind, Multi-Branch in Design", 2. Pemisahan Data: "Global Master" vs "Branch-Specific Data", 3.1 Universal Audit Trail (5 Kolom Standar), 3.2 Kolom Cabang Wajib (`branch_id`), 3.3 Relational Integrity & Zero Hard-Delete, 3.4 Tipe Data Presisi Anti-Floating Point (+11 more)

### Community 3 - "Pharmacy POS & ERP System (Dokumentasi & Spesifikasi Arsitektur)"
Cohesion: 0.14
Nodes (13): 1. Membuka Graf Pengetahuan Interaktif, 2. Menanyakan Relasi Arsitektur via AI, 3. Memperbarui Graf Pengetahuan, 🏛️ Arsitektur 5 Pilar Sistem, Cara Menerbitkan Tiket via AI:, 👥 Matriks Peran Pengguna (Role-Based Access Control / RBAC), Menutup Tiket Otomatis via Pull Request (Cross-Repo Auto-Close):, 🧠 Panduan Navigasi Knowledge Graph (Graphify) (+5 more)

### Community 4 - "2. Invariants & Standards for DRA (Database Architecture & ERD)"
Cohesion: 0.15
Nodes (12): 1. Core Principles of Technical Documentation, 2.1 Multi-Branch Ready from Day 1 (Invarian Mutlak), 2.2 Universal Audit Trail (Wajib di Setiap Tabel), 2.3 Relational Integrity & Deletion Policy, 2.4 Strict Data Type Conventions, 2.5 Indexing Strategy for Pharmacy Performance, 2. Invariants & Standards for DRA (Database Architecture & ERD), 3.1 Standard RESTful API Envelope (+4 more)

### Community 5 - "PROGRESS.md"
Cohesion: 0.29
Nodes (5): Master Product Requirements Document (PRD), graphify, Workflow: graphify, Status Progres Pengerjaan Dokumentasi & Arsitektur, Fondasi Arsitektur & Panduan Multi-Branch Ready

### Community 6 - "3. Template Body Tiket Standar"
Cohesion: 0.20
Nodes (9): 1. Core Principles of Issue-Driven Development (IDD), 2. Format Penamaan & Labeling Standar, 3. Template Body Tiket Standar, A. Format Backend (`[BE]`), B. Format Frontend POS Kasir (`[FE-POS]`), C. Format Frontend Web Admin / ERP (`[FE-WEB]`), Format Judul Tiket:, Issue Task Scaffolder Skill (Spec-Driven / Issue-Driven Development) (+1 more)

### Community 7 - "🏷️ Rincian Fitur per Fase"
Cohesion: 0.20
Nodes (9): Fase 1: Autentikasi, Legalitas Profesi, Master Data & Multi-Branch Ready, Fase 2: Front-Office POS, Pelayanan Resep & Peracikan, Fase 3: Pergudangan, Batch FEFO & Kartu Stok BPOM, Fase 4: Procurement & Rantai Pasok PBF, Fase 5: Finansial, SIPNAP & Integrasi SatuSehat, Master Feature & Implementation Checklist, 📊 Matriks Status Implementasi Global, Pharmacy POS & ERP System (Apotek) (+1 more)

### Community 8 - "Langkah-Langkah Eksekusi Otomatis"
Cohesion: 0.22
Nodes (8): 1. Periksa Status Berkas & Perubahan Sesi Ini, 2. Perbarui Dokumen Pelacak (`PROGRESS.md`), 3. Sinkronkan Hub Navigasi (`README.md`), 4. Sinkronisasikan Knowledge Graph (Graphify), 5. Buat Titik Simpan Git (Commit), 6. Berikan Laporan Penutup ke Pengguna, Langkah-Langkah Eksekusi Otomatis, Workflow: /save-progress

### Community 9 - "Pharmacy POS & ERP System (Apotek)"
Cohesion: 0.22
Nodes (9): 1. Tata Kelola & Aturan Arsitektur (`.agents/` & `.github/`), 2. Dokumen Induk & Fondasi Identitas, 3. Modul Spesifikasi Kebutuhan Bisnis (PRD), 🎯 Antrean Pengerjaan Berikutnya (Next Action Items: Fase 1), 🔑 Aturan Penting yang Wajib Dipertahankan (Invariants), 💡 Cara Memulai Kembali Sesi (Resume Prompt), ✅ Deliverables yang Sudah Selesai (Completed), Pharmacy POS & ERP System (Apotek) (+1 more)

### Community 10 - "2. Standard 4-Step Cascade Update Workflow"
Cohesion: 0.25
Nodes (7): 1. The Core Philosophy: "Zero Documentation Drift", 2. Standard 4-Step Cascade Update Workflow, Change Impact Synchronizer & Traceability Skill, Langkah 1: Identifikasi & Update Dokumen Asal, Langkah 2: Lacak Dampak (Impact Traceability Analysis), Langkah 3: Eksekusi Cascade Update (Penyelarasan Hilir), Langkah 4: Simpan Progres & Sinkronkan Knowledge Graph

### Community 11 - "Langkah-Langkah Eksekusi Otomatis"
Cohesion: 0.25
Nodes (7): 1. Identifikasi Modul & Peran Target, 2. Kumpulkan Konteks Dokumen, 3. Susun Isi Tiket Sesuai Template, 4. Terbitkan Tiket via GitHub CLI (`gh`), 5. Laporkan Tautan Tiket ke Pengguna, Langkah-Langkah Eksekusi Otomatis, Workflow: /publish-issue

### Community 12 - "Pharmacy Management System: POS & ERP Apotek"
Cohesion: 0.29
Nodes (7): 1. Metadata Dokumen, 2.1 Konteks Masalah Operasional Apotek, 2.2 Visi & Solusi Produk, 2. Latar Belakang & Problem Statement, 3. Matriks Peran Pengguna (Role-Based Access Control / RBAC), 4. Arsitektur 5 Pilar Modul Utama, Pharmacy Management System: POS & ERP Apotek

### Community 13 - "Business-Centric PRD Scaffolder Skill (Pharmacy POS & ERP)"
Cohesion: 0.29
Nodes (6): 1. Core Philosophy: "WHAT & WHY", Not "HOW", 2. Tabel Komparasi: PRD (Bisnis) vs TRD/DRA (Teknis), 3. Standard Template Structure for Feature PRDs, Business-Centric PRD Scaffolder Skill (Pharmacy POS & ERP), ✅ Mandatory Focus in PRDs (Wajib Ada di PRD):, 🚫 Strictly Prohibited in PRDs (Dilarang Masuk ke PRD):

### Community 14 - "Feature PRD: Master Obat, Penggolongan Regulasi Farmasi & Multi-Satuan Bertingkat"
Cohesion: 0.09
Nodes (22): 1. Metadata Dokumen, 2. Latar Belakang & Masalah Bisnis, 3. Persona & Konteks Penggunaan, 4.1 Alur 1: Pendaftaran Master Obat & Definisi Hierarki Multi-Satuan, 4.2 Alur 2: Transaksi Penjualan Eceran POS & Auto-Breakdown Mutasi, 4.3 Alur 3: Prosedur Aman Penggantian Satuan Terkecil (Unit Re-basing Wizard), 4. Alur Kerja Utama (Core User Journey), 5.1 Katalog Produk & Penggolongan Regulasi Farmasi (+14 more)

### Community 15 - "backend-task.md"
Cohesion: 0.33
Nodes (5): 🛡️ Aturan Bisnis yang Wajib Dipenuhi, 📚 Dokumen Acuan (SSOT), 🎯 Lingkup Pengerjaan (Checklist), ⚠️ Perhatian Khusus & Edge Cases, 📌 Ringkasan Tugas

### Community 16 - "frontend-pos-task.md"
Cohesion: 0.33
Nodes (5): 🛡️ Aturan Bisnis yang Wajib Dipenuhi, 📚 Dokumen Acuan (SSOT), 🎯 Lingkup Pengerjaan (Checklist), ⚡ Non-Functional Requirements (NFR), 📌 Ringkasan Tugas

### Community 17 - "Persona: Tika — Lead System Analyst & Technical Project Manager"
Cohesion: 0.40
Nodes (4): 1. Identitas & Profil Utama, 2. Penguasaan Domain & Pengetahuan Sistem (Core Competencies), 3. Tanggung Jawab Harian Tika dalam Tim, Persona: Tika — Lead System Analyst & Technical Project Manager

### Community 18 - "frontend-web-task.md"
Cohesion: 0.40
Nodes (4): 🛡️ Aturan Bisnis yang Wajib Dipenuhi, 📚 Dokumen Acuan (SSOT), 🎯 Lingkup Pengerjaan (Checklist), 📌 Ringkasan Tugas

### Community 19 - "Feature PRD: Master Distributor (PBF), Dokter Perujuk & Rekam Pengobatan Pasien (PMR)"
Cohesion: 0.08
Nodes (25): 1. Metadata Dokumen, 2. Latar Belakang & Masalah Bisnis, 3. Persona & Konteks Penggunaan, 4.1 Alur 1: Pendaftaran Master Distributor (PBF) & Legalitas Operasional, 4.2 Alur 2: Pendaftaran Master Dokter Perujuk & Validasi Izin Praktik (SIP), 4.3 Alur 3: Pendekatan 4-Tingkat Pasien di POS (Kasir Cepat vs Resep vs PMR), 4. Alur Kerja Utama (Core User Journey), 5.1 Bagian 1: Master Pedagang Besar Farmasi (PBF / Supplier) (+17 more)

### Community 20 - "9. Kriteria Penerimaan (Acceptance Criteria)"
Cohesion: 0.29
Nodes (7): 9. Kriteria Penerimaan (Acceptance Criteria), AC-01: Pendaftaran Master Obat dengan Satuan Bertingkat & Multi-Barcode, AC-02: Seamless Auto-Breakdown di Meja Kasir POS, AC-03: Pencatatan Ganda Lalu Lintas Stok pada Pemecahan Otomatis, AC-04: Kalkulasi HPP Proporsional Termasuk Bonus Faktur PBF, AC-05: Perlindungan Mutlak terhadap Pengubahan Satuan Terkecil (Anti-Corruption Guardrail), AC-06: Manajemen Kamus Master Satuan & Validasi Anti-Free-Text

## Knowledge Gaps
- **186 isolated node(s):** `graphify`, `1. Identitas & Profil Utama`, `2. Penguasaan Domain & Pengetahuan Sistem (Core Competencies)`, `3. Tanggung Jawab Harian Tika dalam Tim`, `🚫 Strictly Prohibited in PRDs (Dilarang Masuk ke PRD):` (+181 more)
  These have ≤1 connection - possible missing edges or undocumented components. (Counts symbols only; 186 node(s) total have ≤1 connection when file, concept and rationale nodes are included.)

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **Why does `Feature PRD: Master Obat, Penggolongan Regulasi Farmasi & Multi-Satuan Bertingkat` connect `Feature PRD: Master Obat, Penggolongan Regulasi Farmasi & Multi-Satuan Bertingkat` to `9. Kriteria Penerimaan (Acceptance Criteria)`, `PROGRESS.md`?**
  _High betweenness centrality (0.206) - this node is a cross-community bridge._
- **Why does `Feature PRD: Autentikasi, Akun Pengguna, Legalitas Profesi Farmasi & Hak Akses (RBAC)` connect `Feature PRD: Autentikasi, Akun Pengguna, Legalitas Profesi Farmasi & Hak Akses (RBAC)` to `PROGRESS.md`?**
  _High betweenness centrality (0.186) - this node is a cross-community bridge._
- **Why does `Feature PRD: Master Cabang, Izin Operasional Apotek (SIA) & Struktur Gudang` connect `Feature PRD: Master Cabang, Izin Operasional Apotek (SIA) & Struktur Gudang` to `PROGRESS.md`?**
  _High betweenness centrality (0.185) - this node is a cross-community bridge._
- **What connects `graphify`, `1. Identitas & Profil Utama`, `2. Penguasaan Domain & Pengetahuan Sistem (Core Competencies)` to the rest of the system?**
  _186 weakly-connected nodes found - possible documentation gaps or missing edges._
- **Should `Feature PRD: Autentikasi, Akun Pengguna, Legalitas Profesi Farmasi & Hak Akses (RBAC)` be split into smaller, more focused modules?**
  _Cohesion score 0.07692307692307693 - nodes in this community are weakly interconnected._
- **Should `Feature PRD: Master Cabang, Izin Operasional Apotek (SIA) & Struktur Gudang` be split into smaller, more focused modules?**
  _Cohesion score 0.07692307692307693 - nodes in this community are weakly interconnected._
- **Should `Pharmacy POS & ERP System` be split into smaller, more focused modules?**
  _Cohesion score 0.10526315789473684 - nodes in this community are weakly interconnected._