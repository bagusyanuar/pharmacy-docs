# Graph Report - pharmacy-docs  (2026-09-19)

## Corpus Check
- Corpus is ~10,033 words - fits in a single context window. You may not need a graph.

## Summary
- 147 nodes · 146 edges · 14 communities
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

## God Nodes (most connected - your core abstractions)
1. `Langkah-Langkah Eksekusi Otomatis` - 7 edges
2. `Pharmacy Management System: POS & ERP Apotek` - 7 edges
3. `Pharmacy POS & ERP System (Dokumentasi & Spesifikasi Arsitektur)` - 7 edges
4. `Pharmacy POS & ERP System` - 7 edges
5. `2. Invariants & Standards for DRA (Database Architecture & ERD)` - 6 edges
6. `Langkah-Langkah Eksekusi Otomatis` - 6 edges
7. `4. Arsitektur 5 Pilar Modul Utama` - 6 edges
8. `🏷️ Rincian Fitur per Fase` - 6 edges
9. `Pharmacy POS & ERP System (Apotek)` - 6 edges
10. `2. Standard 4-Step Cascade Update Workflow` - 5 edges

## Surprising Connections (you probably didn't know these)
- None detected - all connections are within the same source files.

## Communities (14 total, 0 thin omitted)

### Community 0 - "Governance Rules & Persona Tika"
Cohesion: 0.12
Nodes (12): graphify, 1. Identitas & Profil Utama, 2. Penguasaan Domain & Pengetahuan Sistem (Core Competencies), 3. Tanggung Jawab Harian Tika dalam Tim, Persona: Tika — Lead System Analyst & Technical Project Manager, Workflow: graphify, 🛡️ Aturan Bisnis yang Wajib Dipenuhi, 📚 Dokumen Acuan (SSOT) (+4 more)

### Community 1 - "Multi-Branch Architecture & DB Invariants"
Cohesion: 0.12
Nodes (16): 1.1 Masalah Klasik Migrasi Apotek, 1.2 Strategi Solusi: Multi-Branch Ready Sejak Hari Pertama (Day 1), 1. Filosofi: "Single-Branch in Mind, Multi-Branch in Design", 2. Pemisahan Data: "Global Master" vs "Branch-Specific Data", 3.1 Universal Audit Trail (5 Kolom Standar), 3.2 Kolom Cabang Wajib (`branch_id`), 3.3 Relational Integrity & Zero Hard-Delete, 3.4 Tipe Data Presisi Anti-Floating Point (+8 more)

### Community 2 - "Master PRD Pharmacy POS & ERP"
Cohesion: 0.12
Nodes (15): 1. Metadata Dokumen, 2.1 Konteks Masalah Operasional Apotek, 2.2 Visi & Solusi Produk, 2. Latar Belakang & Problem Statement, 3. Matriks Peran Pengguna (Role-Based Access Control / RBAC), 4. Arsitektur 5 Pilar Modul Utama, 5. Non-Functional Requirements (NFR), 6. Roadmap & Milestone Implementasi (+7 more)

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
Nodes (9): Fase 1: Fondasi Master Data, Multi-Branch & Autentikasi, Fase 2: Front-Office POS, Pelayanan Resep & Peracikan, Fase 3: Pergudangan, Batch FEFO & Kartu Stok BPOM, Fase 4: Procurement & Rantai Pasok PBF, Fase 5: Finansial, SIPNAP & Integrasi SatuSehat, Master Feature & Implementation Checklist, 📊 Matriks Status Implementasi Global, Pharmacy POS & ERP System (Apotek) (+1 more)

### Community 7 - "Save Progress Workflow"
Cohesion: 0.22
Nodes (8): 1. Periksa Status Berkas & Perubahan Sesi Ini, 2. Perbarui Dokumen Pelacak (`PROGRESS.md`), 3. Sinkronkan Hub Navigasi (`README.md`), 4. Sinkronisasikan Knowledge Graph (Graphify), 5. Buat Titik Simpan Git (Commit), 6. Berikan Laporan Penutup ke Pengguna, Langkah-Langkah Eksekusi Otomatis, Workflow: /save-progress

### Community 8 - "Change Impact & Traceability"
Cohesion: 0.25
Nodes (7): 1. The Core Philosophy: "Zero Documentation Drift", 2. Standard 4-Step Cascade Update Workflow, Change Impact Synchronizer & Traceability Skill, Langkah 1: Identifikasi & Update Dokumen Asal, Langkah 2: Lacak Dampak (Impact Traceability Analysis), Langkah 3: Eksekusi Cascade Update (Penyelarasan Hilir), Langkah 4: Simpan Progres & Sinkronkan Knowledge Graph

### Community 9 - "Publish Issue Workflow"
Cohesion: 0.25
Nodes (7): 1. Identifikasi Modul & Peran Target, 2. Kumpulkan Konteks Dokumen, 3. Susun Isi Tiket Sesuai Template, 4. Terbitkan Tiket via GitHub CLI (`gh`), 5. Laporkan Tautan Tiket ke Pengguna, Langkah-Langkah Eksekusi Otomatis, Workflow: /publish-issue

### Community 10 - "Project Progress & Milestone Tracker"
Cohesion: 0.25
Nodes (8): 1. Tata Kelola & Aturan Arsitektur (`.agents/` & `.github/`), 2. Dokumen Induk & Fondasi Identitas, 🎯 Antrean Pengerjaan Berikutnya (Next Action Items: Fase 1), 🔑 Aturan Penting yang Wajib Dipertahankan (Invariants), 💡 Cara Memulai Kembali Sesi (Resume Prompt), ✅ Deliverables yang Sudah Selesai (Completed - 100%), Pharmacy POS & ERP System (Apotek), 📌 Ringkasan Status Proyek

### Community 11 - "Business PRD Scaffolder"
Cohesion: 0.33
Nodes (5): Business-Centric PRD Scaffolder Skill (Pharmacy POS & ERP), Core Philosophy: "WHAT & WHY", Not "HOW", Mandatory Focus in PRDs:, Standard Template Structure for Feature PRDs, Strictly Prohibited in PRDs:

### Community 12 - "Frontend POS Task Template"
Cohesion: 0.33
Nodes (5): 🛡️ Aturan Bisnis yang Wajib Dipenuhi, 📚 Dokumen Acuan (SSOT), 🎯 Lingkup Pengerjaan (Checklist), ⚡ Non-Functional Requirements (NFR), 📌 Ringkasan Tugas

### Community 13 - "Frontend Web Task Template"
Cohesion: 0.40
Nodes (4): 🛡️ Aturan Bisnis yang Wajib Dipenuhi, 📚 Dokumen Acuan (SSOT), 🎯 Lingkup Pengerjaan (Checklist), 📌 Ringkasan Tugas

## Knowledge Gaps
- **101 isolated node(s):** `graphify`, `1. Identitas & Profil Utama`, `2. Penguasaan Domain & Pengetahuan Sistem (Core Competencies)`, `3. Tanggung Jawab Harian Tika dalam Tim`, `Strictly Prohibited in PRDs:` (+96 more)
  These have ≤1 connection - possible missing edges or undocumented components. (Counts symbols only; 101 node(s) total have ≤1 connection when file, concept and rationale nodes are included.)

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **What connects `graphify`, `1. Identitas & Profil Utama`, `2. Penguasaan Domain & Pengetahuan Sistem (Core Competencies)` to the rest of the system?**
  _101 weakly-connected nodes found - possible documentation gaps or missing edges._
- **Should `Governance Rules & Persona Tika` be split into smaller, more focused modules?**
  _Cohesion score 0.11764705882352941 - nodes in this community are weakly interconnected._
- **Should `Multi-Branch Architecture & DB Invariants` be split into smaller, more focused modules?**
  _Cohesion score 0.11764705882352941 - nodes in this community are weakly interconnected._
- **Should `Master PRD Pharmacy POS & ERP` be split into smaller, more focused modules?**
  _Cohesion score 0.125 - nodes in this community are weakly interconnected._
- **Should `Master Hub & System Architecture` be split into smaller, more focused modules?**
  _Cohesion score 0.14285714285714285 - nodes in this community are weakly interconnected._