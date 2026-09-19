---
name: "Frontend POS Kasir (FE-POS)"
description: "Tiket implementasi Frontend POS Kasir & Resep: Keyboard-First, Scan Barcode, Racikan & Printer Thermal"
title: "[FE-POS] <Nama Modul>: <Ringkasan Tugas>"
labels: ["frontend-pos"]
assignees: []
---

## 📌 Ringkasan Tugas
<!-- Deskripsi singkat antarmuka kasir/resep yang dibangun -->

## 📚 Dokumen Acuan (SSOT)
- **Business PRD:** [Nama PRD](https://github.com/bagusyanuar/pharmacy-docs/blob/main/features/...)
- **API Contracts (TRD):** [TRD Spesifikasi](https://github.com/bagusyanuar/pharmacy-docs/blob/main/technical/...)

## 🎯 Lingkup Pengerjaan (Checklist)
- [ ] Antarmuka kasir cepat dengan fokus navigasi keyboard-first (shortcut F1-F12, Esc, Enter).
- [ ] Integrasi barcode scanner (auto focus & direct item insertion).
- [ ] Modul racikan obat: input formula, kalkulasi dosis, hitung tuslah & embalase otomatis.
- [ ] State management keranjang belanja (mendukung hold/pending cart & recall).
- [ ] Integrasi pencetakan:
  - Struk belanja kasir via thermal printer (ESC/POS 58mm/80mm).
  - Cetak etiket obat thermal (etiket putih obat dalam & etiket biru obat luar).
- [ ] Alur buka kasir & tutup shift (rekonsiliasi uang fisik laci kasir).

## 🛡️ Aturan Bisnis yang Wajib Dipenuhi
- [ ] `BR-PHARM-...`: ...
- [ ] `BR-PHARM-...`: ...

## ⚡ Non-Functional Requirements (NFR)
- Kecepatan checkout: penambahan item < 100ms.
- Tahan terhadap gangguan jaringan sementara (offline resilience saat scanning).
