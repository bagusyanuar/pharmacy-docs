# Master Feature & Implementation Checklist
# Pharmacy POS & ERP System (Apotek)

Dokumen ini adalah matriks pelacak implementasi fitur menyeluruh lintas fase dan platform: **Dokumentasi (PRD/DRA/TRD)**, **Backend API & Database (BE)**, **Frontend Web Admin/ERP (FE-WEB)**, dan **Frontend POS Kasir & Resep (FE-POS)**.

---

## 📊 Matriks Status Implementasi Global

| Kategori | Total Fitur | Docs Selesai | BE Selesai | FE-WEB Selesai | FE-POS Selesai | Status Fase |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Fase 1: Master Data & Cabang** | 6 | 0/6 | 0/6 | 0/6 | 0/6 | 🟡 Dalam Desain |
| **Fase 2: POS Kasir & Resep** | 6 | 0/6 | 0/6 | - | 0/6 | ⚪ Antrean |
| **Fase 3: Inventory & FEFO** | 5 | 0/5 | 0/5 | 0/5 | - | ⚪ Antrean |
| **Fase 4: Pengadaan PBF** | 5 | 0/5 | 0/5 | 0/5 | - | ⚪ Antrean |
| **Fase 5: Finansial & SIPNAP** | 4 | 0/4 | 0/4 | 0/4 | - | ⚪ Antrean |
| **TOTAL** | **26** | **0/26** | **0/26** | **0/26** | **0/26** | **0% Selesai** |

---

## 🏷️ Rincian Fitur per Fase

### Fase 1: Fondasi Master Data, Multi-Branch & Autentikasi
Fokus: Membangun fondasi arsitektur multi-cabang, katalog produk terstandarisasi, dan hierarki peran farmasi.

| Kode | Nama Fitur Bisnis | Docs PRD | Docs DRA/TRD | Backend [BE] | Web Admin [FE-WEB] |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **F1-01** | **Master Cabang & Gudang:** Struktur cabang tunggal/jaringan, alamat, izin apotek (SIA), APA penanggung jawab. | [ ] | [ ] | [ ] | [ ] |
| **F1-02** | **Role, User & Sesi Cabang:** RBAC 6 persona, hak akses, otorisasi cabang aktif, audit trail. | [ ] | [ ] | [ ] | [ ] |
| **F1-03** | **Master Obat & Zat Aktif:** Nama obat, nama generik, pabrik farmasi, golongan (Bebas, Terbatas, Keras, Narkotika). | [ ] | [ ] | [ ] | [ ] |
| **F1-04** | **Hierarki Multi-Satuan:** Relasi satuan bertingkat (Box $\rightarrow$ Strip $\rightarrow$ Tablet) & harga eceran proporsional. | [ ] | [ ] | [ ] | [ ] |
| **F1-05** | **Master Supplier (PBF):** Daftar distributor farmasi, kontak, izin PBF, default Term of Payment (TOP). | [ ] | [ ] | [ ] | [ ] |
| **F1-06** | **Master Dokter & Faskes Perujuk:** Data dokter penulis resep, nomor SIP, dan klinik/RS asal. | [ ] | [ ] | [ ] | [ ] |

---

### Fase 2: Front-Office POS, Pelayanan Resep & Peracikan
Fokus: Kecepatan pelayanan kasir depan, kalkulator racikan otomatis, cetak etiket, dan akurasi shift kasir.

| Kode | Nama Fitur Bisnis | Docs PRD | Docs TRD | Backend [BE] | POS Kasir [FE-POS] |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **F2-01** | **Kasir Cepat OTC (Obat Bebas):** Navigasi keyboard, barcode scan, keranjang belanja, hold/recall cart. | [ ] | [ ] | [ ] | [ ] |
| **F2-02** | **Pelayanan Resep Dokter:** Skrining resep, input dokter (No SIP), data pasien (alergi, BB, umur), riwayat resep. | [ ] | [ ] | [ ] | [ ] |
| **F2-03** | **Kalkulator Racikan Otomatis:** Perhitungan formula puyer/kapsul/salep, Dosis Maksimum (DM), biaya Tuslah & Embalase. | [ ] | [ ] | [ ] | [ ] |
| **F2-04** | **Pencetakan Thermal Etiket & Struk:** Cetak etiket putih (obat dalam), etiket biru (obat luar), dan struk kasir ESC/POS. | [ ] | [ ] | [ ] | [ ] |
| **F2-05** | **Multi-Metode Pembayaran:** Tunai dengan kalkulator kembalian, QRIS statis/dinamis, kartu debit/EDC, piutang instansi. | [ ] | [ ] | [ ] | [ ] |
| **F2-06** | **Manajemen Shift & Laci Kasir:** Buka kasir modal awal, penutupan shift, hitung uang fisik, laporan selisih kas. | [ ] | [ ] | [ ] | [ ] |

---

### Fase 3: Pergudangan, Batch FEFO & Kartu Stok BPOM
Fokus: Mencegah kerugian obat kadaluarsa, transparansi mutasi stok, dan kepatuhan audit BPOM.

| Kode | Nama Fitur Bisnis | Docs PRD | Docs DRA/TRD | Backend [BE] | Web Admin [FE-WEB] |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **F3-01** | **Alokasi FEFO Otomatis:** Pengeluaran stok otomatis mendahulukan batch dengan tanggal expired terdekat. | [ ] | [ ] | [ ] | [ ] |
| **F3-02** | **Early Warning Expiry Date (ED):** Dashboard alert obat ED 6 bulan, 3 bulan, dan 1 bulan sebelum jatuh tempo. | [ ] | [ ] | [ ] | [ ] |
| **F3-03** | **Kartu Stok Digital Terverifikasi:** Jejak mutasi per batch, masuk/keluar, saldo sisa, dokumen rujukan standar BPOM. | [ ] | [ ] | [ ] | [ ] |
| **F3-04** | **Stock Opname (SO) Dinamis:** Opname parsial per rak tanpa tutup apotek, input fisik, verifikasi selisih, jurnal penyesuaian. | [ ] | [ ] | [ ] | [ ] |
| **F3-05** | **Transfer Stok Antar Cabang:** Permintaan transfer (Request), pengiriman (In-Transit), dan penerimaan fisik (Receipt). | [ ] | [ ] | [ ] | [ ] |

---

### Fase 4: Procurement & Rantai Pasok PBF
Fokus: Pengadaan obat tepat waktu, kepatuhan format Surat Pesanan resmi, dan mitigasi kebocoran harga beli.

| Kode | Nama Fitur Bisnis | Docs PRD | Docs DRA/TRD | Backend [BE] | Web Admin [FE-WEB] |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **F4-01** | **Buku Defekta Otomatis (ROP):** Rekomendasi belanja saat stok $\le$ Reorder Point berdasarkan buffer stock. | [ ] | [ ] | [ ] | [ ] |
| **F4-02** | **Generator Surat Pesanan (SP) Resmi:** Format SP Reguler, OOT, Prekursor, dan Psikotropika/Narkotika standar BPOM/APA. | [ ] | [ ] | [ ] | [ ] |
| **F4-03** | **Penerimaan Barang & Faktur PBF:** Pencocokan PO vs Faktur fisik, verifikasi nomor batch fisik & ED, diskon bertingkat. | [ ] | [ ] | [ ] | [ ] |
| **F4-04** | **HPP Moving Average Dinamis:** Pembaruan otomatis HPP setiap penerimaan faktur baru agar laba kotor selalu akurat. | [ ] | [ ] | [ ] | [ ] |
| **F4-05** | **Retur Pembelian (ED / Rusak):** Nota retur ke PBF untuk obat menjelang expired sesuai syarat retur distributor. | [ ] | [ ] | [ ] | [ ] |

---

### Fase 5: Finansial, SIPNAP & Integrasi SatuSehat
Fokus: Pengawasan likuiditas hutang dagang, laporan laba rugi riil, dan integrasi Kemenkes.

| Kode | Nama Fitur Bisnis | Docs PRD | Docs TRD | Backend [BE] | Web Admin [FE-WEB] |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **F5-01** | **Jatuh Tempo Hutang PBF (TOP):** Pengawasan faktur jatuh tempo (7, 14, 30 hari), status lunas/sebagian, pembayaran kas/bank. | [ ] | [ ] | [ ] | [ ] |
| **F5-02** | **Laporan Laba Rugi & Margin:** Laba kotor/bersih per cabang, margin per kategori obat, analisis dead stock & slow-moving. | [ ] | [ ] | [ ] | [ ] |
| **F5-03** | **Pelaporan SIPNAP Kemenkes:** Ekspor data mutasi Narkotika dan Psikotropika sesuai format resmi BPOM/Kemenkes. | [ ] | [ ] | [ ] | [ ] |
| **F5-04** | **Integrasi SatuSehat Kemenkes:** Format data pertukaran resep dan dispensing obat berbasis standar FHIR. | [ ] | [ ] | [ ] | [ ] |
