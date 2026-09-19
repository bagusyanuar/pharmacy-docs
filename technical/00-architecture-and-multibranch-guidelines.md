# Fondasi Arsitektur & Panduan Multi-Branch Ready
# Pharmacy POS & ERP System

Dokumen ini adalah acuan arsitektur tingkat teknis yang menetapkan prinsip desain sistem, skema database, dan strategi migrasi dari **Apotek Tunggal (Single-Branch)** menuju **Jaringan Apotek (Multi-Branch)** tanpa risiko penulisan ulang kode (*zero-rewrite*).

---

## 1. Filosofi: "Single-Branch in Mind, Multi-Branch in Design"

### 1.1 Masalah Klasik Migrasi Apotek
Banyak sistem POS apotek dibangun dengan asumsi cabang tunggal:
* Tabel `products` langsung menyimpan kolom `stock` dan `price`.
* Tabel transaksi `sales` tidak mencatat lokasi fisik apotek.
* Laci kasir dan sesi shift dianggap hanya satu di dunia nyata.

Ketika apotek membuka cabang ke-2 atau gudang pusat:
* Seluruh kueri pemotongan stok rusak karena tidak tahu stok milik cabang mana.
* Transaksi penjualan cabang A bercampur dengan cabang B.
* Pengembang terpaksa merombak ratusan kueri SQL, mengubah skema tabel, dan mengambil risiko *downtime* yang sangat mahal.

### 1.2 Strategi Solusi: Multi-Branch Ready Sejak Hari Pertama (Day 1)
Pada sistem ini, arsitektur data dan API dirancang **Multi-Branch Native sejak awal**:
1. **Di Lapisan Database:** Tabel `branches` sudah dibuat sejak migrasi pertama. Setiap baris data stok fisik, mutasi gudang, transaksi kasir, dan sesi shift **wajib** memiliki *Foreign Key* `branch_id REFERENCES branches(id)`.
2. **Di Lapisan Antarmuka Pengguna (UI/UX) Tahap 1:** Sistem dapat melakukan *auto-select* ke `branch_id` cabang default (misal: "Apotek Sehat - Cabang Utama"). Pengguna kasir tidak perlu memilih cabang saat bertransaksi.
3. **Saat Cabang ke-2 Dibuka:** Cukup membuka menu switch cabang dan modul *Inter-Branch Transfer*, **tanpa perlu mengubah skema database satu pun!**

---

## 2. Pemisahan Data: "Global Master" vs "Branch-Specific Data"

Untuk menjaga efisiensi dan mencegah duplikasi data obat, sistem memisahkan domain data secara tegas:

```
┌─────────────────────────────────────────────────────────────┐
│                 GLOBAL MASTER DATA (Shared)                 │
│  • Master Produk & Barcode (`products`)                     │
│  • Nama Generik & Zat Aktif (`generic_names`)               │
│  • Golongan Obat (`drug_classifications`)                   │
│  • Pabrik / Manufaktur (`manufacturers`)                    │
│  • Master Distributor PBF (`pbf_suppliers`)                 │
│  • Satuan & Aturan Konversi (`product_units`)               │
└──────────────────────────────┬──────────────────────────────┘
                               │ Digunakan bersama oleh seluruh cabang
                               ▼
┌─────────────────────────────────────────────────────────────┐
│              BRANCH-SPECIFIC DATA (Scoped by branch_id)     │
│  • Stok Fisik & Nomor Batch (`product_stocks`)              │
│  • Transaksi Penjualan POS (`sales` & `sale_items`)         │
│  • Buku Defekta Cabang (`branch_defects`)                   │
│  • Surat Pesanan & Penerimaan Faktur (`purchase_orders`)    │
│  • Sesi Shift & Laci Uang Kasir (`cash_shifts`)             │
│  • Kartu Stok Digital Cabang (`stock_mutations`)            │
│  • Hasil Stock Opname Fisik (`stock_opnames`)               │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. Standar Invarian Database (PostgreSQL 15+)

### 3.1 Universal Audit Trail (5 Kolom Standar)
Setiap tabel entitas WAJIB memuat 5 kolom berikut:
```sql
id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
created_at  TIMESTAMPTZ NOT NULL DEFAULT NOW(),
updated_at  TIMESTAMPTZ NOT NULL DEFAULT NOW(),
created_by  UUID NULL REFERENCES users(id) ON DELETE RESTRICT,
deleted_at  TIMESTAMPTZ NULL -- Soft delete untuk audit trail
```

### 3.2 Kolom Cabang Wajib (`branch_id`)
Pada seluruh tabel yang bersifat operasional/spesifik cabang:
```sql
branch_id UUID NOT NULL REFERENCES branches(id) ON DELETE RESTRICT
```

### 3.3 Relational Integrity & Zero Hard-Delete
* Seluruh relasi *Foreign Key* wajib menggunakan `ON DELETE RESTRICT`.
* Dilarang menggunakan `ON DELETE CASCADE` pada data transaksi medis/farmasi untuk mencegah terhapusnya riwayat hukum obat keras/narkotika.

### 3.4 Tipe Data Presisi Anti-Floating Point
* **Harga & Nilai Finansial:** `DECIMAL(12,2)` (contoh: Rp 1.500.000,50).
* **Kuantitas & Formulasi Racikan:** `DECIMAL(10,3)` (contoh: 0.125 gram serbuk paracetamol, 15.500 ml sirup obat batuk).

---

## 4. Alur Manajemen Stok Berbasis Batch & FEFO

### 4.1 Skema Data Stok Fisik
Stok tidak pernah dicatat sebagai angka tunggal di tabel produk, melainkan dicatat per **nomor batch dan tanggal kadaluarsa** di cabang tertentu:

```sql
CREATE TABLE product_stocks (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    branch_id UUID NOT NULL REFERENCES branches(id) ON DELETE RESTRICT,
    product_id UUID NOT NULL REFERENCES products(id) ON DELETE RESTRICT,
    batch_number VARCHAR(100) NOT NULL,
    expired_date DATE NOT NULL,
    quantity DECIMAL(10,3) NOT NULL DEFAULT 0,
    hpp DECIMAL(12,2) NOT NULL, -- Harga Pokok Pembelian untuk batch ini
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    created_by UUID NULL REFERENCES users(id) ON DELETE RESTRICT,
    deleted_at TIMESTAMPTZ NULL,
    
    CONSTRAINT uq_branch_product_batch UNIQUE (branch_id, product_id, batch_number)
);
```

### 4.2 Kueri Indexing Kunci (High-Speed FEFO Allocation)
Kueri pencarian obat yang wajib dikeluarkan kasir selalu diurutkan dari `expired_date ASC`:
```sql
CREATE INDEX idx_product_stocks_fefo 
ON product_stocks (branch_id, product_id, expired_date ASC)
WHERE deleted_at IS NULL AND quantity > 0;
```

---

## 5. Alur Transfer Stok Antar Cabang (Inter-Branch Transfer)

Ketika jaringan apotek memiliki lebih dari 1 cabang atau gudang pusat:

```mermaid
sequenceDiagram
    autonumber
    actor CabangB as Cabang B (Pemohon)
    participant Sistem as Sistem POS & ERP
    actor CabangA as Cabang A (Pengirim)

    CabangB->>Sistem: 1. Buat Permintaan Transfer (Stock Transfer Request)
    Sistem-->>CabangA: Notifikasi permintaan stok obat
    CabangA->>Sistem: 2. Setujui & Dispatch Barang (Pilih Batch FEFO)
    Note over Sistem: Stok Cabang A berkurang.<br/>Status barang: IN-TRANSIT (Tidak ada di stok fisik A maupun B)
    Sistem-->>CabangB: Barang sedang dikirim
    CabangB->>Sistem: 3. Terima & Verifikasi Fisik Barang (Confirm Receipt)
    Note over Sistem: Stok Cabang B bertambah sesuai batch & ED yang diterima.<br/>Status transfer: COMPLETED
```

*Keuntungan Alur Ini:*
* Mencegah barang "hilang" di perjalanan.
* Nilai aset persediaan tetap seimbang di neraca keuangan (*In-Transit Inventory Account*).
* Riwayat nomor batch dan tanggal expired tetap utuh dari cabang asal ke cabang tujuan.

---

## 6. Otorisasi Sesi Pengguna & Header API (`X-Branch-Id`)

1. **Token JWT:** Memuat identitas pengguna, role, dan daftar `allowed_branch_ids`.
2. **Request Header:**
   Setiap panggilan API dari kasir atau admin menyertakan header:
   ```http
   X-Branch-Id: 550e8400-e29b-41d4-a716-446655440000
   ```
3. **Middleware Backend:**
   * Memvalidasi apakah user berhak mengakses `X-Branch-Id` tersebut.
   * Menginjeksi `branch_id` tersebut ke dalam kueri database (menggunakan parameterized filter `WHERE branch_id = :branch_id`).
   * Kasir hanya dapat beroperasi pada 1 cabang tempat dia ditugaskan.
   * Owner / Super Admin memiliki izin untuk switch ke cabang manapun atau melihat laporan gabungan (*All Branches Consolidated*).

---

## 7. Arsitektur Klien: Strategi PWA-First & Jalur Migrasi Android

### 7.1 Mengapa Memilih PWA-First (Progressive Web App)?
Untuk fase awal pengembangan apotek, frontend dibangun dengan pendekatan **PWA-First**:
* **Satu Basis Kode (*Single Codebase*):** Satu aplikasi web modern dapat diakses dan di-install (*Add to Home Screen*) di seluruh form factor:
  * **Komputer Kasir PC / Laptop:** Antarmuka lebar dengan dukungan penuh shortcut keyboard (F1–F12, Enter, Esc) dan USB barcode scanner.
  * **Tablet Android / iPad (10–12 inci):** Antarmuka otomatis beradaptasi menjadi tombol-tombol sentuh ramah jari (*touch-friendly grid*), ideal untuk apotek modern dengan dudukan tablet (*stand dock*).
  * **Ponsel Pintar (*Smartphone*):** Antarmuka vertikal responsif khusus untuk Owner mengecek dashboard omzet dan staf gudang saat *Stock Opname* di lorong rak obat.
* **Pembaruan Instan Tanpa Review Play Store:** Setiap perbaikan bug atau penambahan fitur di server langsung aktif di seluruh cabang tanpa perlu mendistribusikan file APK atau menunggu antrean tinjauan Google Play Store.
* **Ketahanan Luring (*Offline-Resilience*):** Menggunakan *Service Worker* dan penyimpanan lokal browser (*IndexedDB*) untuk menyimpan katalog obat dan menampung antrean transaksi saat internet apotek terputus sementara.

### 7.2 Integrasi Perangkat Keras Kasir pada PWA
* **Printer Struk & Etiket Thermal:** Komunikasi via WebUSB, WebBluetooth, atau Network Printing (IP Printer LAN/Wi-Fi) menggunakan perintah standar ESC/POS.
* **Barcode Scanner:** Kompatibel dengan mode *Keyboard Wedge / USB HID Input* tanpa memerlukan driver khusus.
* **Laci Kasir (Cash Drawer):** Terpicu terbuka otomatis melalui sinyal pulsa kick-out printer thermal (kabel RJ11) saat transaksi berhasil dibukukan.

### 7.3 Jalur Migrasi Masa Depan Menuju Aplikasi Android Native
Jika di masa depan bisnis berkembang dan membutuhkan aplikasi native Android khusus tablet:
* **Prinsip Backend Headless (0% Perubahan Backend):** Seluruh aturan bisnis, perhitungan racikan, FEFO, dan skema database berada di Backend REST API. Frontend native Android nantinya hanya bertindak sebagai *consumer* API yang memanggil endpoint yang sama persis dengan PWA.
* **Opsi A (Jalur Kilat 1 Hari via Capacitor / TWA):** Membungkus (*wrapper*) PWA yang sudah ada ke dalam format `.apk` Android menggunakan Capacitor atau *Trusted Web Activity (TWA)* dari Google, memberikan ikon native dan akses penuh ke hardware Android tanpa perlu menulis ulang antarmuka.
* **Opsi B (Jalur Dedicated Native via Flutter / Kotlin):** Membangun UI native baru dengan SDK printer Bluetooth bawaan Android untuk perangkat POS all-in-one (seperti Sunmi/iMin), tetap mengacu pada kontrak API TRD yang ada.

