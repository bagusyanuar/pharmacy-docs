---
name: "Backend Task (BE)"
description: "Tiket implementasi Backend: Migration Database PostgreSQL & REST API Controller"
title: "[BE] <Nama Modul>: <Ringkasan Tugas>"
labels: ["backend"]
assignees: []
---

## 📌 Ringkasan Tugas
<!-- Deskripsi singkat apa yang dibangun untuk sisi Backend -->

## 📚 Dokumen Acuan (SSOT)
- **Business PRD:** [Nama PRD](https://github.com/bagusyanuar/pharmacy-docs/blob/main/features/...)
- **Database Architecture (DRA):** [DRA Spesifikasi](https://github.com/bagusyanuar/pharmacy-docs/blob/main/technical/...)
- **API Contracts (TRD):** [TRD Spesifikasi](https://github.com/bagusyanuar/pharmacy-docs/blob/main/technical/...)

## 🎯 Lingkup Pengerjaan (Checklist)
- [ ] Buat file migration tabel PostgreSQL sesuai DRA (wajib kolom universal audit & `branch_id`).
- [ ] Implementasi Service Layer dengan logic bisnis dan transaksi database aman (`DB Transaction`).
- [ ] Implementasi Controller & Validasi DTO sesuai kontrak TRD (envelope standar `{ success, data, meta, error }`).
- [ ] Implementasi mekanisme locking stok / FEFO jika berkaitan dengan mutasi barang.
- [ ] Unit test & API integration test (Coverage minimal 80%).

## 🛡️ Aturan Bisnis yang Wajib Dipenuhi
- [ ] `BR-PHARM-...`: ...
- [ ] `BR-PHARM-...`: ...

## ⚠️ Perhatian Khusus & Edge Cases
- Pastikan isolasi multi-cabang `branch_id` divalidasi dari context session/header.
- Tidak ada hard delete pada data audit/transaksi (`ON DELETE RESTRICT`).
- **Standar Penamaan Database:** Kolom & tabel database wajib berbahasa Inggris (`snake_case`) untuk atribut teknis/umum (`created_at`, `is_active`, `unit_price`, dll.). Khusus istilah regulasi & domain khas Indonesia (seperti `sipa_number`, `strttk_number`, `sia_number`, `is_apa`, `bpjs_card_number`, `sipnap_reported_at`, `satusehat_ihs_id`, `nik`, `tuslah_amount` / `tuslah_fee`, `embalase_fee`, dan enum golongan obat BPOM), pertahankan istilah baku Indonesia.
