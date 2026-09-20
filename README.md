# Pharmacy POS & ERP System (Dokumentasi & Spesifikasi Arsitektur)

Sistem Informasi & Manajemen Apotek Terpadu: **Point of Sale (POS) Kasir Meja Depan**, **Peracikan Resep Dokter**, **Pergudangan Berbasis Batch & FEFO**, **Pengadaan Rantai Pasok PBF**, **HPP & Finansial**, serta **Kepatuhan Regulasi Kemenkes/BPOM (SIPNAP & SatuSehat)**.

Dirancang dengan arsitektur **Multi-Branch Ready from Day 1** untuk mendukung pertumbuhan apotek dari cabang tunggal (*single-outlet*) hingga jaringan apotek multi-cabang tanpa risiko perombakan ulang kode.

---

## 🏛️ Arsitektur 5 Pilar Sistem

```
                     ┌─────────────────────────────────────────────────────────┐
                     │            DASHBOARD & EXECUTIVE ANALYTICS              │
                     │  (Omzet, Margin HPP, Dead Stock, ROP, Audit Log Cabang) │
                     └────────────────────────────┬────────────────────────────┘
                                                  │
         ┌───────────────────────┬────────────────┴────────────────┬───────────────────────┐
         ▼                       ▼                                 ▼                       ▼
┌──────────────────┐   ┌──────────────────┐             ┌──────────────────┐   ┌──────────────────┐
│  PILAR 1: MASTER │   │   PILAR 2: POS   │             │ PILAR 3: WMS &   │   │  PILAR 4: SUPPLY │
│  DATA & CABANG   │   │  KASIR & RESEP   │             │ INVENTORY FEFO   │   │  CHAIN & PBF     │
├──────────────────┤   ├──────────────────┤             ├──────────────────┤   ├──────────────────┤
│• Multi-Branch    │   │• Penjualan OTC   │ ◄──Mutasi── │• Batch & ED FEFO │ ◄─│• Defekta ROP     │
│• Master Produk   │   │• Skrining Resep  │    Stok     │• Multi-Satuan    │ PO│• SP Resmi APA    │
│  (Paten/Generik) │   │• Modul Racikan   │             │• Kartu Stok BPOM │   │• Faktur Masuk    │
│• Multi-Satuan    │   │• Cetak Etiket    │             │• Stock Opname    │   │• Retur ED ke PBF │
│• Master PBF & SIP│   │• Shift & Laci Kas│             │• Mutasi Cabang   │   │• Hutang & TOP    │
└──────────────────┘   └──────────────────┘             └──────────────────┘   └──────────────────┘
                                                                  │
                                                                  ▼
                                                ┌──────────────────────────────────┐
                                                │ PILAR 5: FINANSIAL & KEPATUHAN   │
                                                ├──────────────────────────────────┤
                                                │• HPP Moving Average & Laba Rugi  │
                                                │• Laporan SIPNAP (Kemenkes/BPOM)  │
                                                │• Kesiapan Integrasi SatuSehat    │
                                                └──────────────────────────────────┘
```

---

## 👥 Matriks Peran Pengguna (Role-Based Access Control / RBAC)

| Peran (Role) | Lingkup Cabang | Deskripsi & Wewenang Utama | Platform Utama |
| :--- | :--- | :--- | :--- |
| **Kasir** | Terkunci ke 1 Cabang aktif | Transaksi penjualan OTC, scan barcode, kalkulator kembalian, pembukaan & penutupan shift kasir. | `[FE-POS]` Kasir Desktop |
| **Tenaga Teknis Kefarmasian (TTK)** | Terkunci ke 1 Cabang aktif | Menyiapkan resep, peracikan obat (puyer/kapsul/salep), perhitungan Dosis Maksimum (DM), cetak etiket thermal. | `[FE-POS]` / `[FE-WEB]` |
| **Apoteker Pengelola Apotek (APA)** | 1 Cabang atau Multi-Cabang | Skrining legalitas resep, validasi Surat Pesanan (SP) dengan SIPA resmi, verifikasi pelaporan SIPNAP BPOM. | `[FE-WEB]` Web Admin |
| **Staf Gudang & Pengadaan** | Sesuai penugasan cabang/pusat | Pembuatan PO PBF, penerimaan fisik faktur & nomor batch, stock opname dinamis, transfer stok antar cabang. | `[FE-WEB]` Web Admin |
| **Finance & Akuntansi** | Seluruh cabang / per cabang | Rekonsiliasi kas shift, pelunasan faktur hutang PBF (*Term of Payment*), pengawasan HPP Moving Average, Laba Rugi. | `[FE-WEB]` Web Admin |
| **Owner / Super Admin** | Global (Semua Cabang) | Pengaturan cabang, master obat global, pricing, pemantauan omzet konsolidasi seluruh cabang, manajemen hak akses. | `[FE-WEB]` Web Admin |

---

## 📂 Struktur Repositori & Peta Navigasi Dokumen

```
pharmacy-docs/
├── .agents/                                  # Konfigurasi AI Persona, Workflows & Skills
│   ├── rules/
│   │   ├── persona-tika.md                   # Persona Tika (Lead System Analyst & Technical PM)
│   │   └── graphify.md                       # Aturan navigasi Knowledge Graph hemat token
│   ├── workflows/
│   │   ├── save-progress.md                  # Workflow simpan progres sesi (/save-progress)
│   │   ├── graphify.md                       # Workflow perbarui graphify (/graphify)
│   │   └── publish-issue.md                  # Workflow terbitkan issue GitHub (/publish-issue)
│   └── skills/
│       ├── business-prd-scaffolder/SKILL.md  # Template penulisan PRD murni bisnis apotek
│       ├── trd-dra-scaffolder/SKILL.md       # Standard arsitektur DRA PostgreSQL & TRD API
│       ├── change-impact-synchronizer/SKILL.md # SOP 4-langkah Zero Documentation Drift
│       └── issue-task-scaffolder/SKILL.md    # Framework Lean Scoping tiket GitHub Issues
├── .github/
│   └── ISSUE_TEMPLATE/
│       ├── backend-task.md                   # Template tiket Backend [BE]
│       ├── frontend-web-task.md              # Template tiket Web Admin/ERP [FE-WEB]
│       └── frontend-pos-task.md              # Template tiket POS Kasir & Resep [FE-POS]
├── 00-MASTER-PRD.md                          # Master PRD ekosistem Pharmacy POS & ERP
├── FEATURE-CHECKLIST.md                      # Matriks pelacak status implementasi fitur
├── PROGRESS.md                               # Checkpoint resmi dan progres pengerjaan
├── README.md                                 # Hub navigasi utama (dokumen ini)
├── features/                                 # Kumpulan spesifikasi kebutuhan bisnis (PRD)
│   ├── 01-prd-auth-user.md                   # PRD Autentikasi Dual-UX, Legalitas SIPA & Hak Akses
│   ├── master-data/                          # PRD Master Cabang, Produk, Multi-Satuan, PBF
│   │   ├── 01-prd-cabang-dan-gudang.md       # PRD Master Cabang, Izin SIA & Struktur Gudang
│   │   ├── 02-prd-obat-dan-satuan-bertingkat.md # PRD Master Obat, Golongan Regulasi, Multi-Satuan & HPP
│   │   └── 03-prd-pbf-supplier-dan-dokter.md # PRD Master PBF, Dokter Perujuk & PMR Pasien
│   ├── pos-resep/                            # PRD Kasir OTC, Resep, Racikan, Shift
│   ├── inventory-wms/                        # PRD Batch FEFO, Kartu Stok BPOM, Transfer Cabang
│   ├── procurement-pbf/                      # PRD Buku Defekta, Surat Pesanan, Faktur Masuk
│   └── finance-laporan/                      # PRD HPP, Hutang PBF, Laba Rugi, SIPNAP
├── technical/                                # Kumpulan spesifikasi arsitektur teknis
│   └── 00-architecture-and-multibranch-guidelines.md # Fondasi arsitektur multi-cabang & data
└── graphify-out/                             # Knowledge Graph interaktif hasil Graphify
```

---

## 🧠 Panduan Navigasi Knowledge Graph (Graphify)

Repositori ini dilengkapi graf pengetahuan otomatis (**Graphify**) untuk memetakan keterkaitan antar dokumen, entitas database, dan aturan bisnis farmasi.

### 1. Membuka Graf Pengetahuan Interaktif
Buka berkas visual graf di browser Anda:
```bash
open graphify-out/graph.html
```

### 2. Menanyakan Relasi Arsitektur via AI
Gunakan kueri Graphify untuk menelusuri alur sistem secara cerdas:
```bash
# Tanya alur keterkaitan fitur
graphify query "Bagaimana alur dari resep racikan memotong stok batch FEFO dan mempengaruhi HPP?"

# Cari lintasan konsep
graphify path "Sales" "ProductStocks"

# Jelaskan konsep spesifik
graphify explain "MultiBranchReady"
```

### 3. Memperbarui Graf Pengetahuan
Graf diperbarui otomatis saat Anda menjalankan checkpoint `/save-progress` atau manual:
```bash
graphify update .
```

---

## 🎫 Panduan Penerbitan Tiket Tugas (Issue-Driven Development)

Repositori ini mendukung metodologi **Spec-Driven Development / Issue-Driven Development (IDD)**. Seluruh dokumen spesifikasi dapat langsung diterbitkan menjadi tiket **GitHub Issues** bagi tim developer.

### Tiga Kategori Tiket Resmi:
* `[BE]` — **Backend & Database:** Migrasi PostgreSQL, domain service, locking stok FEFO, REST API controller.
* `[FE-WEB]` — **Frontend Web Admin/ERP:** Desktop web (React/Next.js/Vue) untuk manajemen cabang, gudang pusat, pengadaan, dan keuangan.
* `[FE-POS]` — **Frontend POS Kasir & Resep:** Antarmuka kasir cepat keyboard-first, scan barcode, peracikan obat, dan printer thermal.

### Cara Menerbitkan Tiket via AI:
Gunakan perintah slash command `/publish-issue` atau minta asisten AI:
> *"Tika, tolong terbitkan tiket GitHub Issues untuk modul [Nama Modul] bagi tim BE, FE-WEB, dan FE-POS."*

### Menutup Tiket Otomatis via Pull Request (Cross-Repo Auto-Close):
Di repositori kode implementasi (misal di repo backend atau frontend), sertakan referensi tiket pada deskripsi PR:
```markdown
## Summary
Implementasi modul master obat multi-satuan dan skema database PostgreSQL.

Closes bagusyanuar/pharmacy-docs#1
```

---

## ⚡ Titik Simpan & Melanjutkan Sesi Kerja

* **Menyimpan Progres:** Ketik `/save-progress` atau perintahkan *"save progress Jon"* untuk memperbarui `PROGRESS.md`, menyinkronkan graf Graphify, dan membuat commit git.
* **Status Proyek Terkini:** Pantau daftar penyelesaian deliverable di [`PROGRESS.md`](./PROGRESS.md).
* **Melanjutkan Sesi:** Cukup ketik:
  > *"Halo Tika, tolong baca `PROGRESS.md` dan kita lanjutkan pengerjaan ke [Nama Modul]."*
