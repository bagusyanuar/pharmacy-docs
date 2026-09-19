# Master Feature & Implementation Checklist
# Pharmacy POS & ERP System (Apotek)

Dokumen ini adalah matriks pelacak implementasi fitur menyeluruh lintas fase dan platform: **Dokumentasi (PRD/DRA/TRD)**, **Backend API & Database (BE)**, **Frontend Web Admin/ERP (FE-WEB)**, dan **Frontend POS Kasir & Resep (FE-POS)**.

---

## 📊 Matriks Status Implementasi Global

| Kategori | Total Fitur | Docs Selesai | BE Selesai | FE-WEB Selesai | FE-POS Selesai | Status Fase |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Fase 1: Auth & Master Data** | 8 | 0/8 | 0/8 | 0/8 | 0/8 | 🟡 Dalam Desain |
| **Fase 2: POS Kasir & Resep** | 7 | 0/7 | 0/7 | - | 0/7 | ⚪ Antrean |
| **Fase 3: Inventory WMS & FEFO** | 6 | 0/6 | 0/6 | 0/6 | - | ⚪ Antrean |
| **Fase 4: Pengadaan PBF** | 6 | 0/6 | 0/6 | 0/6 | - | ⚪ Antrean |
| **Fase 5: Finansial & Regulasi** | 5 | 0/5 | 0/5 | 0/5 | - | ⚪ Antrean |
| **TOTAL** | **32** | **0/32** | **0/32** | **0/32** | **0/32** | **0% Selesai** |

---

## 🏷️ Rincian Fitur per Fase

### Fase 1: Autentikasi, Legalitas Profesi, Master Data & Multi-Branch Ready
Fokus: Akses pengguna, legalitas SIPA/STRTTK, isolasi sesi cabang, fondasi multi-cabang, dan katalog produk farmasi.

| Kode | Nama Fitur Bisnis | Docs PRD | Docs DRA/TRD | Backend [BE] | Frontend Web/POS |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **F0-01** | **Dual-UX Autentikasi & Sesi Cabang:** Fast PIN/Barcode login POS kasir & Email/Password Web ERP terikat `X-Branch-Id`. | [ ] | [ ] | [ ] | [ ] |
| **F0-02** | **Profil Karyawan & Legalitas Profesi:** Pencatatan izin SIPA Apoteker (masa berlaku) dan STRTTK/SIPTTK untuk SP & etiket. | [ ] | [ ] | [ ] | [ ] |
| **F0-03** | **Matriks Hak Akses (RBAC) & Supervisor Override:** Hak akses 6 persona dan pop-up PIN otorisasi (void, diskon khusus, approval resep). | [ ] | [ ] | [ ] | [ ] |
| **F1-01** | **Master Cabang & Gudang:** Struktur cabang tunggal/jaringan, izin apotek (SIA), APA penanggung jawab, gudang cabang vs pusat. | [ ] | [ ] | [ ] | [ ] |
| **F1-02** | **Master Obat, Zat Aktif & Golongan:** Nama obat, nama generik, pabrik, barcode EAN-13, golongan (Bebas/Terbatas/Keras/Narko). | [ ] | [ ] | [ ] | [ ] |
| **F1-03** | **Hierarki Multi-Satuan Bertingkat:** Relasi satuan (Box $\rightarrow$ Strip $\rightarrow$ Tablet) & aturan margin harga jual eceran. | [ ] | [ ] | [ ] | [ ] |
| **F1-04** | **Master Supplier (PBF):** Direktori distributor farmasi, kontak salesman, izin PBF, rekening, default Term of Payment (TOP). | [ ] | [ ] | [ ] | [ ] |
| **F1-05** | **Master Dokter & Rekam Pasien (PMR):** Dokter perujuk (No SIP) dan Patient Medication Record (alergi, riwayat medikasi). | [ ] | [ ] | [ ] | [ ] |

---

### Fase 2: Front-Office POS, Pelayanan Resep & Peracikan
Fokus: Kecepatan pelayanan kasir depan, kalkulator racikan otomatis, cetak etiket, dan akurasi shift kasir.

| Kode | Nama Fitur Bisnis | Docs PRD | Docs TRD | Backend [BE] | POS Kasir [FE-POS] |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **F2-01** | **Kasir Cepat OTC (Obat Bebas):** Navigasi keyboard, barcode scan, keranjang belanja, hold/recall cart, diskon transaksi. | [ ] | [ ] | [ ] | [ ] |
| **F2-02** | **Pelayanan Resep Dokter:** Skrining resep, input dokter (No SIP), data pasien (alergi, BB), Salinan Resep (*Copy Resep/Iter*). | [ ] | [ ] | [ ] | [ ] |
| **F2-03** | **Kalkulator Racikan Otomatis:** Formula puyer/kapsul/salep, Dosis Maksimum (DM), auto-deduct bahan baku mentah pecahan. | [ ] | [ ] | [ ] | [ ] |
| **F2-04** | **Komponen Biaya Farmasi (Tuslah & Embalase):** Jasa farmasi otomatis (Tuslah) & biaya kemasan (Embalase: wadah, kapsul, kertas). | [ ] | [ ] | [ ] | [ ] |
| **F2-05** | **Pencetakan Thermal Etiket & Struk:** Cetak etiket putih (obat dalam), etiket biru (obat luar), dan struk kasir ESC/POS. | [ ] | [ ] | [ ] | [ ] |
| **F2-06** | **Multi-Metode Pembayaran:** Tunai (kalkulator kembalian), QRIS statis/dinamis, kartu debit/EDC, piutang penjamin/asuransi. | [ ] | [ ] | [ ] | [ ] |
| **F2-07** | **Manajemen Shift & Laci Kasir:** Buka kasir modal awal, penutupan shift, hitung uang fisik, laporan selisih kas fisik. | [ ] | [ ] | [ ] | [ ] |

---

### Fase 3: Pergudangan, Batch FEFO & Kartu Stok BPOM
Fokus: Mencegah kerugian obat kadaluarsa, transparansi mutasi stok, dan kepatuhan audit BPOM.

| Kode | Nama Fitur Bisnis | Docs PRD | Docs DRA/TRD | Backend [BE] | Web Admin [FE-WEB] |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **F3-01** | **Alokasi FEFO Otomatis:** Pengeluaran stok otomatis mendahulukan batch dengan tanggal expired terdekat di cabang aktif. | [ ] | [ ] | [ ] | [ ] |
| **F3-02** | **Early Warning Expiry Date (ED):** Dashboard radar obat ED 6 bulan, 3 bulan, dan 1 bulan sebelum jatuh tempo. | [ ] | [ ] | [ ] | [ ] |
| **F3-03** | **Kartu Stok Digital Terverifikasi:** Jejak mutasi per batch, masuk/keluar, saldo sisa, dokumen rujukan standar BPOM. | [ ] | [ ] | [ ] | [ ] |
| **F3-04** | **Stock Opname (SO) Dinamis:** Opname parsial per rak tanpa tutup apotek, input fisik, verifikasi selisih, berita acara SO. | [ ] | [ ] | [ ] | [ ] |
| **F3-05** | **Penyesuaian Stok & Pemusnahan Obat:** Koreksi barang rusak/pecah/hilang dengan dokumen Berita Acara Pemusnahan resmi. | [ ] | [ ] | [ ] | [ ] |
| **F3-06** | **Transfer Stok Antar Cabang:** Permintaan transfer (Request), pengiriman (In-Transit), dan penerimaan fisik (Receipt). | [ ] | [ ] | [ ] | [ ] |

---

### Fase 4: Procurement & Rantai Pasok PBF
Fokus: Pengadaan obat tepat waktu, kepatuhan format Surat Pesanan resmi, dan mitigasi kebocoran harga beli.

| Kode | Nama Fitur Bisnis | Docs PRD | Docs DRA/TRD | Backend [BE] | Web Admin [FE-WEB] |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **F4-01** | **Buku Defekta Otomatis (ROP):** Rekomendasi belanja saat stok $\le$ Reorder Point berdasarkan buffer stock & lead time. | [ ] | [ ] | [ ] | [ ] |
| **F4-02** | **Generator Surat Pesanan (SP) Resmi:** Format SP Reguler, OOT, Prekursor, dan Psikotropika/Narkotika standar BPOM/APA. | [ ] | [ ] | [ ] | [ ] |
| **F4-03** | **Manajemen PO ke PBF:** Penerbitan PO digital, pelacakan status pesanan (Draft, Sent, Partially Received, Closed). | [ ] | [ ] | [ ] | [ ] |
| **F4-04** | **Penerimaan Barang & Faktur PBF:** Pencocokan 3 arah (Fisik vs PO vs Faktur PBF), input batch & ED baru, diskon bertingkat. | [ ] | [ ] | [ ] | [ ] |
| **F4-05** | **HPP Moving Average Dinamis:** Pembaruan otomatis HPP setiap penerimaan faktur baru agar laba kotor selalu riil. | [ ] | [ ] | [ ] | [ ] |
| **F4-06** | **Retur Pembelian (ED / Rusak):** Nota retur ke PBF untuk obat menjelang expired sesuai syarat distributor farmasi. | [ ] | [ ] | [ ] | [ ] |

---

### Fase 5: Finansial, SIPNAP & Integrasi SatuSehat
Fokus: Pengawasan likuiditas hutang dagang, laporan laba rugi riil, dan integrasi Kemenkes.

| Kode | Nama Fitur Bisnis | Docs PRD | Docs TRD | Backend [BE] | Web Admin [FE-WEB] |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **F5-01** | **Jatuh Tempo Hutang PBF (TOP):** Pengawasan faktur jatuh tempo (7, 14, 21, 30 hari), status lunas/sebagian, pembayaran kas/bank. | [ ] | [ ] | [ ] | [ ] |
| **F5-02** | **Laporan Laba Rugi & Margin:** Laba kotor/bersih per cabang, margin per kategori obat, konsolidasi seluruh cabang. | [ ] | [ ] | [ ] | [ ] |
| **F5-03** | **Analisis Performa Persediaan:** Analisis Pareto ABC, produk Fast-Moving, Slow-Moving, dan deteksi Dead Stock. | [ ] | [ ] | [ ] | [ ] |
| **F5-04** | **Pelaporan SIPNAP Kemenkes:** Ekspor data mutasi Narkotika dan Psikotropika sesuai format resmi BPOM/Kemenkes. | [ ] | [ ] | [ ] | [ ] |
| **F5-05** | **Integrasi SatuSehat Kemenkes:** Format data pertukaran resep dan dispensing obat berbasis standar FHIR (*MedicationDispense*). | [ ] | [ ] | [ ] | [ ] |
