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
