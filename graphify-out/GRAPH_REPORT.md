# Graph Report - pharmacy-docs  (2026-09-19)

## Corpus Check
- Corpus is ~14,333 words - fits in a single context window. You may not need a graph.

## Summary
- 168 nodes · 168 edges · 17 communities
- Extraction: 100% EXTRACTED · 0% INFERRED · 0% AMBIGUOUS
- Token cost: 0 input · 0 output

## Community Hubs (Navigation)
- Governance Rules & Persona Tika
- Multi-Branch Architecture & DB Invariants
- Master PRD Pharmacy POS & ERP
- Master Hub & System Architecture
- Technical Architecture & DRA Standards
- Issue-Driven Development Scaffolder
- Feature Checklist & Implementation Matrix
- Save Progress Workflow
- Change Impact & Traceability
- Publish Issue Workflow
- Project Progress & Milestone Tracker
- Business PRD Scaffolder
- Frontend POS Task Template
- Frontend Web Task Template
- Auth & Professional License PRD
- Community 15
- Community 16

## God Nodes (most connected - your core abstractions)
1. `Feature PRD: Autentikasi, Akun Pengguna, Legalitas Profesi Farmasi & Hak Akses (RBAC)` - 10 edges
2. `Langkah-Langkah Eksekusi Otomatis` - 7 edges
3. `Pharmacy POS & ERP System (Dokumentasi & Spesifikasi Arsitektur)` - 7 edges
4. `Pharmacy POS & ERP System` - 7 edges
5. `2. Invariants & Standards for DRA (Database Architecture & ERD)` - 6 edges
6. `Langkah-Langkah Eksekusi Otomatis` - 6 edges
7. `🏷️ Rincian Fitur per Fase` - 6 edges
8. `Pharmacy POS & ERP System (Apotek)` - 6 edges
9. `9. Kriteria Penerimaan (Acceptance Criteria)` - 6 edges
10. `2. Standard 4-Step Cascade Update Workflow` - 5 edges

## Surprising Connections (you probably didn't know these)
- None detected - all connections are within the same source files.

## Communities (17 total, 0 thin omitted)

### Community 0 - "Governance Rules & Persona Tika"
Cohesion: 0.11
Nodes (19): 1. Metadata Dokumen, 2. Latar Belakang & Masalah Bisnis, 3. Persona & Konteks Penggunaan, 4.1 Alur 1: Login Cepat Kasir di Meja Depan (POS Fast PIN), 4.2 Alur 2: Otorisasi Supervisor / Apoteker (*Manager Override*) di Meja Kasir, 4.3 Alur 3: Manajemen Profil Legalitas Profesi & Pemantauan Masa Berlaku SIPA, 4. Alur Kerja Utama (Core User Journey), 5.1 Dual-UX Autentikasi & Keamanan Sesi (+11 more)

### Community 1 - "Multi-Branch Architecture & DB Invariants"
Cohesion: 0.12
Nodes (16): 1.1 Masalah Klasik Migrasi Apotek, 1.2 Strategi Solusi: Multi-Branch Ready Sejak Hari Pertama (Day 1), 1. Filosofi: "Single-Branch in Mind, Multi-Branch in Design", 2. Pemisahan Data: "Global Master" vs "Branch-Specific Data", 3.1 Universal Audit Trail (5 Kolom Standar), 3.2 Kolom Cabang Wajib (`branch_id`), 3.3 Relational Integrity & Zero Hard-Delete, 3.4 Tipe Data Presisi Anti-Floating Point (+8 more)

### Community 2 - "Master PRD Pharmacy POS & ERP"
Cohesion: 0.14
Nodes (9): Master Product Requirements Document (PRD), graphify, Workflow: graphify, 🛡️ Aturan Bisnis yang Wajib Dipenuhi, 📚 Dokumen Acuan (SSOT), 🎯 Lingkup Pengerjaan (Checklist), ⚡ Non-Functional Requirements (NFR), 📌 Ringkasan Tugas (+1 more)

### Community 3 - "Master Hub & System Architecture"
Cohesion: 0.14
Nodes (13): 1. Membuka Graf Pengetahuan Interaktif, 2. Menanyakan Relasi Arsitektur via AI, 3. Memperbarui Graf Pengetahuan, 🏛️ Arsitektur 5 Pilar Sistem, Cara Menerbitkan Tiket via AI:, 👥 Matriks Peran Pengguna (Role-Based Access Control / RBAC), Menutup Tiket Otomatis via Pull Request (Cross-Repo Auto-Close):, 🧠 Panduan Navigasi Knowledge Graph (Graphify) (+5 more)

### Community 4 - "Technical Architecture & DRA Standards"
Cohesion: 0.15
Nodes (12): 1. Core Principles of Technical Documentation, 2.1 Multi-Branch Ready from Day 1 (Invarian Mutlak), 2.2 Universal Audit Trail (Wajib di Setiap Tabel), 2.3 Relational Integrity & Deletion Policy, 2.4 Strict Data Type Conventions, 2.5 Indexing Strategy for Pharmacy Performance, 2. Invariants & Standards for DRA (Database Architecture & ERD), 3.1 Standard RESTful API Envelope (+4 more)

### Community 5 - "Issue-Driven Development Scaffolder"
Cohesion: 0.20
Nodes (9): 1. Core Principles of Issue-Driven Development (IDD), 2. Format Penamaan & Labeling Standar, 3. Template Body Tiket Standar, A. Format Backend (`[BE]`), B. Format Frontend POS Kasir (`[FE-POS]`), C. Format Frontend Web Admin / ERP (`[FE-WEB]`), Format Judul Tiket:, Issue Task Scaffolder Skill (Spec-Driven / Issue-Driven Development) (+1 more)

### Community 6 - "Feature Checklist & Implementation Matrix"
Cohesion: 0.20
Nodes (9): Fase 1: Autentikasi, Legalitas Profesi, Master Data & Multi-Branch Ready, Fase 2: Front-Office POS, Pelayanan Resep & Peracikan, Fase 3: Pergudangan, Batch FEFO & Kartu Stok BPOM, Fase 4: Procurement & Rantai Pasok PBF, Fase 5: Finansial, SIPNAP & Integrasi SatuSehat, Master Feature & Implementation Checklist, 📊 Matriks Status Implementasi Global, Pharmacy POS & ERP System (Apotek) (+1 more)

### Community 7 - "Save Progress Workflow"
Cohesion: 0.22
Nodes (8): 1. Periksa Status Berkas & Perubahan Sesi Ini, 2. Perbarui Dokumen Pelacak (`PROGRESS.md`), 3. Sinkronkan Hub Navigasi (`README.md`), 4. Sinkronisasikan Knowledge Graph (Graphify), 5. Buat Titik Simpan Git (Commit), 6. Berikan Laporan Penutup ke Pengguna, Langkah-Langkah Eksekusi Otomatis, Workflow: /save-progress

### Community 8 - "Change Impact & Traceability"
Cohesion: 0.22
Nodes (9): 1. Tata Kelola & Aturan Arsitektur (`.agents/` & `.github/`), 2. Dokumen Induk & Fondasi Identitas, 3. Modul Spesifikasi Kebutuhan Bisnis (PRD), 🎯 Antrean Pengerjaan Berikutnya (Next Action Items: Fase 1), 🔑 Aturan Penting yang Wajib Dipertahankan (Invariants), 💡 Cara Memulai Kembali Sesi (Resume Prompt), ✅ Deliverables yang Sudah Selesai (Completed - 100%), Pharmacy POS & ERP System (Apotek) (+1 more)

### Community 9 - "Publish Issue Workflow"
Cohesion: 0.25
Nodes (7): 1. The Core Philosophy: "Zero Documentation Drift", 2. Standard 4-Step Cascade Update Workflow, Change Impact Synchronizer & Traceability Skill, Langkah 1: Identifikasi & Update Dokumen Asal, Langkah 2: Lacak Dampak (Impact Traceability Analysis), Langkah 3: Eksekusi Cascade Update (Penyelarasan Hilir), Langkah 4: Simpan Progres & Sinkronkan Knowledge Graph

### Community 10 - "Project Progress & Milestone Tracker"
Cohesion: 0.25
Nodes (7): 1. Identifikasi Modul & Peran Target, 2. Kumpulkan Konteks Dokumen, 3. Susun Isi Tiket Sesuai Template, 4. Terbitkan Tiket via GitHub CLI (`gh`), 5. Laporkan Tautan Tiket ke Pengguna, Langkah-Langkah Eksekusi Otomatis, Workflow: /publish-issue

### Community 11 - "Business PRD Scaffolder"
Cohesion: 0.29
Nodes (7): 1. Metadata Dokumen, 2.1 Konteks Masalah Operasional Apotek, 2.2 Visi & Solusi Produk, 2. Latar Belakang & Problem Statement, 3. Matriks Peran Pengguna (Role-Based Access Control / RBAC), 4. Arsitektur 5 Pilar Modul Utama, Pharmacy Management System: POS & ERP Apotek

### Community 12 - "Frontend POS Task Template"
Cohesion: 0.29
Nodes (6): 1. Core Philosophy: "WHAT & WHY", Not "HOW", 2. Tabel Komparasi: PRD (Bisnis) vs TRD/DRA (Teknis), 3. Standard Template Structure for Feature PRDs, Business-Centric PRD Scaffolder Skill (Pharmacy POS & ERP), ✅ Mandatory Focus in PRDs (Wajib Ada di PRD):, 🚫 Strictly Prohibited in PRDs (Dilarang Masuk ke PRD):

### Community 13 - "Frontend Web Task Template"
Cohesion: 0.33
Nodes (6): 9. Kriteria Penerimaan (Acceptance Criteria), AC-AUTH-01: Login Cepat Kasir Meja Depan (POS Fast PIN), AC-AUTH-02: Pencegahan Pembobolan PIN (Brute-Force Lock), AC-AUTH-03: Mekanisme Supervisor Override pada Pembatalan Item, AC-AUTH-04: Blokir Otomatis Penerbitan SP saat SIPA Expired, AC-AUTH-05: Pengalihan Cabang untuk Akun Manajemen (Branch Switcher)

### Community 14 - "Auth & Professional License PRD"
Cohesion: 0.33
Nodes (5): 🛡️ Aturan Bisnis yang Wajib Dipenuhi, 📚 Dokumen Acuan (SSOT), 🎯 Lingkup Pengerjaan (Checklist), ⚠️ Perhatian Khusus & Edge Cases, 📌 Ringkasan Tugas

### Community 15 - "Community 15"
Cohesion: 0.40
Nodes (4): 1. Identitas & Profil Utama, 2. Penguasaan Domain & Pengetahuan Sistem (Core Competencies), 3. Tanggung Jawab Harian Tika dalam Tim, Persona: Tika — Lead System Analyst & Technical Project Manager

### Community 16 - "Community 16"
Cohesion: 0.40
Nodes (4): 🛡️ Aturan Bisnis yang Wajib Dipenuhi, 📚 Dokumen Acuan (SSOT), 🎯 Lingkup Pengerjaan (Checklist), 📌 Ringkasan Tugas

## Knowledge Gaps
- **117 isolated node(s):** `graphify`, `1. Identitas & Profil Utama`, `2. Penguasaan Domain & Pengetahuan Sistem (Core Competencies)`, `3. Tanggung Jawab Harian Tika dalam Tim`, `🚫 Strictly Prohibited in PRDs (Dilarang Masuk ke PRD):` (+112 more)
  These have ≤1 connection - possible missing edges or undocumented components. (Counts symbols only; 117 node(s) total have ≤1 connection when file, concept and rationale nodes are included.)

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **Why does `Feature PRD: Autentikasi, Akun Pengguna, Legalitas Profesi Farmasi & Hak Akses (RBAC)` connect `Governance Rules & Persona Tika` to `Master PRD Pharmacy POS & ERP`, `Frontend Web Task Template`?**
  _High betweenness centrality (0.265) - this node is a cross-community bridge._
- **What connects `graphify`, `1. Identitas & Profil Utama`, `2. Penguasaan Domain & Pengetahuan Sistem (Core Competencies)` to the rest of the system?**
  _117 weakly-connected nodes found - possible documentation gaps or missing edges._
- **Should `Governance Rules & Persona Tika` be split into smaller, more focused modules?**
  _Cohesion score 0.10526315789473684 - nodes in this community are weakly interconnected._
- **Should `Multi-Branch Architecture & DB Invariants` be split into smaller, more focused modules?**
  _Cohesion score 0.11764705882352941 - nodes in this community are weakly interconnected._
- **Should `Master PRD Pharmacy POS & ERP` be split into smaller, more focused modules?**
  _Cohesion score 0.14285714285714285 - nodes in this community are weakly interconnected._
- **Should `Master Hub & System Architecture` be split into smaller, more focused modules?**
  _Cohesion score 0.14285714285714285 - nodes in this community are weakly interconnected._