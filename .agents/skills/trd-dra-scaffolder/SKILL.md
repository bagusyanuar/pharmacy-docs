---
name: trd-dra-scaffolder
description: "Framework and architectural standards for scaffolding Tech-Agnostic Technical Requirements Documents (TRD) and Data Requirements Architecture (DRA) specifications for Pharmacy POS & ERP. Enforces platform-independent engineering blueprints, ANSI SQL / PostgreSQL relational modeling, strict multi-branch isolation (branch_id), universal audit trails, precise decimal types, FEFO index strategies, standardized RESTful API contracts, and bidirectional traceability to PRD business rules."
---

# TRD & DRA Scaffolder Skill (Pharmacy POS & ERP)

Standardized framework for creating production-ready, enterprise-grade, **Technology-Agnostic Technical Requirements Documents (TRD)** and **Data Requirements Architecture (DRA)** for the **Pharmacy POS & ERP System (Apotek)**.

---

## 1. Core Principles of Tech-Agnostic Technical Documentation

Technical specifications translate business requirements from the PRD into concrete, unambiguous engineering blueprints for Backend Engineers, DBAs, Frontend Web Engineers, Frontend POS Engineers, and QA Testers.

### 1.1 Platform-Independent & Tech-Agnostic Principle
Dokumen TRD dirancang sebagai **aset arsitektur berumur panjang (*Durable Architectural Asset*)**. TRD **TIDAK BOLEH** terpaku pada satu bahasa pemrograman, runtime, atau framework UI tertentu (misal: NestJS vs Go, React vs Vue vs Flutter, Prisma vs GORM). Jika di masa depan tim memutuskan untuk mengonversi atau memigrasikan tech stack, **dokumen TRD harus tetap 100% valid tanpa perlu ditulis ulang**.

#### Aturan Abstraksi Arsitektur (Tech-Agnostic Mapping):
* **Fokus pada Kontrak & Perilaku (*Contracts & Behaviors*):** Tuliskan spesifikasi antarmuka (API Schemas), tipe data relasional, invarian konkurensi (*locking*), mesin status (*state machines*), protokol perangkat keras (*hardware protocols*), dan algoritma domain farmasi.
* **Gunakan Pola Arsitektur Universal, Bukan Nama Framework/Library:**

| Konsep Arsitektur | ❌ Jangan Tulis (Terlalu Spesifik Stack) | ✅ Tulis Seperti Ini (Universal / Tech-Agnostic) |
| :--- | :--- | :--- |
| **Lapisan Kontroler / API** | Express middleware, NestJS controller decorator, Gin router | **Transport / Controller Layer:** HTTP Handler yang memvalidasi header `X-Branch-Id`, mengekstrak klaim JWT, dan memetakan DTO. |
| **Lapisan Bisnis** | Service class NestJS `@Injectable()`, Laravel Service Provider | **Domain Service Layer:** Komponen logika bisnis murni yang mengelola aturan domain (FEFO, racikan) dan mengoordinasikan transaksi. |
| **Lapisan Akses Data** | Prisma Client, GORM `db.Where()`, TypeORM Entity, Eloquent | **Data Access / Repository Pattern:** Abstraksi persistensi yang menjalankan kueri relasional dengan penanganan locking dan filter cabang. |
| **State Klien (Frontend)** | Zustand store, Redux slice, Pinia store | **Client-Side Reactive State Store:** Struktur state atomik in-memory (items keranjang, kasir shift, buffer hold/recall). |
| **Integrasi Printer** | Paket npm `node-thermal-printer`, driver Windows ESC/POS | **ESC/POS Byte Protocol Specification:** Format byte command universal (kertas 58mm/80mm, pemotong otomatis, pulsa laci RJ11). |
| **Barcode Scanner** | Listener keyboard React `onKeyDown` hook | **USB HID / Keyboard-Wedge Scanner Buffer:** Listener stream karakter berkecepatan tinggi dengan buffer debounce < 50ms diakhiri `Enter`. |
| **Penyimpanan Offline** | Hanya menyebut `IndexedDB` atau `Room SQLite` | **Client-Side Offline Storage & Outbox Queue:** Mekanisme penyimpanan lokal terisolasi dengan antrean mutasi transaksi sinkronisasi berkala. |
| **Kriptografi & Auth** | Package `bcryptjs`, library `jsonwebtoken` | **Standar Algoritma:** Hash password dengan **Argon2id** (atau Bcrypt cost factor $\ge$ 12), token otentikasi **JWT RS256/Ed25519** (TTL 15 menit) + Refresh Token Rotation. |

---

### 1.2 Structured 3-Tier Technical Blueprint

Arsitektur teknis dibagi secara modular menjadi 3 tingkat dokumentasi:

1. **Global DBML (`technical/database/schema.dbml`):**
   * **Fokus:** *Single Source of Truth (SSOT)* skema database relasional lengkap untuk DBA dan Tech Lead.
   * **Format:** File murni `.dbml` yang siap di-import/render langsung pada [dbdiagram.io](https://dbdiagram.io) atau `dbdocs.io`.
   * **Cakupan:** Bersifat global per milestone utama (memuat seluruh tabel, relasi foreign key `[delete: restrict]`, dan `TableGroups`).

2. **Visual Blueprints & Analisis Sistem (`technical/diagrams/0X-diagrams-*.md`):**
   * **Fokus:** Analisis visual interaksi sistem untuk UI/UX Designer, QA Tester, dan Developer.
   * **Format:** Dokumen Markdown dengan diagram interaktif Mermaid:
     * **Usecase Diagram:** Batas kewenangan 6 persona staf apotek terhadap modul.
     * **Detailed System Flowcharts:** Logika percabangan keputusan untuk alur kritis (login, supervisor override, alokasi FEFO).
     * **Sequence Diagrams:** Pertukaran pesan teknis antar layer (*Client $\leftrightarrow$ Gateway/Transport $\leftrightarrow$ Domain Service $\leftrightarrow$ Database*).
     * **State Lifecycle Diagrams:** Siklus status entitas (resep dokter, sesi shift kasir, faktur pembelian, surat pesanan).
     * **Scoped Sub-ERD:** Mermaid ERD yang hanya menampilkan 4–6 tabel fokus fitur tersebut (mencegah diagram spaghetti).

3. **TRD API Contracts & Architecture (`technical/0X-trd-*.md`):**
   * **Fokus:** Kontrak integrasi antar-komponen, spesifikasi API RESTful, skema DTO JSON, penanganan konkurensi, dan algoritma domain.
   * **Format:** Dokumen teknis Markdown modular per kluster domain fitur.

---

### 1.3 Mermaid Syntax & GitHub Web Rendering Rules (Zero-Error Invariant)
Agar seluruh diagram Mermaid ter-render sempurna tanpa error di GitHub Web & markdown viewers:
* **Invarian Label Panah (`|...|`):**
  * **DILARANG menggunakan kurung `(` atau `)`:** Mengakibatkan fatal lexer parse error `Expecting 'PIPE', ..., got 'PS'`. Gunakan teks bersih (contoh: `|Sudah Kedaluwarsa 0 Hari|`, `|Lebih Dari 1 Cabang|`).
  * **DILARANG menggunakan garis miring `/`:** Ganti dengan kata `atau` (contoh: `|POS atau Tablet|`).
  * **DILARANG menggunakan operator pembanding (`<`, `>`, `<=`, `>=`, `&`, `+`):** Tulis dalam kata (contoh: `|Lebih dari 60 Hari|`, `|Gagal 3 Kali atau Lebih|`).
  * **DILARANG menyisipkan tanda petik (`'` atau `"`) di dalam label `|...|`.**
* **Teks Node & Safe Quotation:**
  * SELALU bungkus teks node dengan tanda petik dua eksplisit `["..."]` jika memuat spasi, tanda baca, atau titik dua.
* **Line Breaks & HTML:**
  * Gunakan `<br/>` HANYA di dalam node berpetik dua (contoh: `Node["Baris 1<br/>Baris 2"]`). DILARANG menggunakan tag styling HTML mentah (`<b>`, `<span>`).
* **Relasi Panah Putus-Putus:**
  * Gunakan teks biasa: `-.->|include|`. DILARANG menggunakan tanda kurung ganda seperti `|"<<include>>"|`.

---

## 2. Invariants & Standards for DRA (Database Architecture & ERD)

Seluruh pemodelan data relasional mengacu pada standar ANSI SQL / relational invariant (dengan PostgreSQL 15+ sebagai dialek referensi implementasi):

### 2.1 Multi-Branch Ready Sejak Hari Pertama (Absolute Invariant)
Setiap tabel inventori fisik, mutasi kartu stok, transaksi kasir, sesi shift kasir, dan pengadaan spesifik cabang WAJIB memiliki:
```sql
branch_id UUID NOT NULL REFERENCES branches(id) ON DELETE RESTRICT
```
*Tabel master data global (contoh: `products`, `generic_names`, `manufacturers`, `pbf_suppliers`) bersifat shared tanpa `branch_id`, namun seluruh data operasional/transaksional wajib terikat pada cabang.*

### 2.2 Universal Audit Trail (5 Kolom Standar di Setiap Tabel Entitas)
```sql
id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
created_at  TIMESTAMPTZ NOT NULL DEFAULT NOW(),
updated_at  TIMESTAMPTZ NOT NULL DEFAULT NOW(),
created_by  UUID NULL REFERENCES users(id) ON DELETE RESTRICT,
deleted_at  TIMESTAMPTZ NULL -- Soft delete untuk menjaga integritas audit hukum BPOM & akuntansi
```

### 2.3 Integritas Relasional & Kebijakan Penghapusan Data
* **Foreign Keys:** WAJIB menggunakan `ON DELETE RESTRICT` (atau `ON DELETE NO ACTION`). Hard cascade delete (`ON DELETE CASCADE`) **dilarang keras** pada transaksi riwayat, batch obat, resep, dan pembukuan finansial demi kepatuhan hukum farmasi.
* **Soft Deletes:** Penghapusan data operasional hanya mengubah `deleted_at = NOW()`. Kueri aktif wajib memfilter `WHERE deleted_at IS NULL`.

### 2.4 Konvensi Tipe Data Presisi Anti-Floating Point
* **Identifier:** `UUID` (standar sistem terdistribusi multi-cabang).
* **Mata Uang & Finansial:** `DECIMAL(12,2)` (Harga Jual, HPP, Total Transaksi, Diskon). Penggunaan tipe data `FLOAT` atau `DOUBLE PRECISION` **dilarang keras**.
* **Kuantitas & Formulasi Racikan:** `DECIMAL(10,3)` (Mendukung fraksi miligram/gram serbuk racikan, mililiter sirup, dan pecahan tablet).
* **Waktu / Timestamp:** `TIMESTAMPTZ` (disimpan dalam UTC, standar ISO 8601).
* **Tanggal Kedaluwarsa:** `DATE` (untuk pelacakan batch obat).

### 2.5 Strategi Pengindeksan Berbasis Kinerja (Index Intent)
* Indeks komposit untuk alokasi cepat batch obat FEFO:
  ```sql
  CREATE INDEX idx_stocks_fefo ON product_stocks(branch_id, product_id, expired_date ASC) 
  WHERE deleted_at IS NULL AND quantity > 0;
  ```
* Indeks pencarian cepat barcode obat:
  ```sql
  CREATE UNIQUE INDEX uq_products_barcode ON products(barcode) WHERE deleted_at IS NULL;
  ```

---

## 3. Standards for Unified Contract-First TRD (Tech-Agnostic Blueprint)

Kami menerapkan pendekatan **Contract-First Single TRD per Feature Domain** (`technical/0X-trd-*.md`). Kontrak API dan spesifikasi invarian sistem menjadi *Single Source of Truth (SSOT)* yang mengikat tim Backend dan Frontend tanpa ketergantungan pada framework tertentu.

---

### 3.1 Standard RESTful API Envelope

Setiap respons API (baik sukses maupun gagal) WAJIB mengikuti format pembungkus seragam (*Standard Response Envelope*):

#### Format Respons Sukses (HTTP 200 / 201):
```json
{
  "success": true,
  "data": { },
  "meta": {
    "page": 1,
    "limit": 20,
    "total_records": 150,
    "total_pages": 8
  },
  "error": null
}
```

#### Format Respons Error (HTTP 4xx / 5xx):
```json
{
  "success": false,
  "data": null,
  "meta": null,
  "error": {
    "code": "STOCK_INSUFFICIENT",
    "message": "Stok obat Paracetamol batch EXP-2026-10 di Cabang Melawai tidak mencukupi.",
    "details": [
      {
        "field": "quantity",
        "issue": "Permintaan: 10, Stok batch tersedia: 4"
      }
    ]
  }
}
```

---

### 3.2 Spesifikasi Arsitektur Backend [BE] (Tech-Agnostic)

Dokumentasikan aturan teknis server secara netral tanpa dependensi framework:
1. **Multi-Branch Isolation Header (`X-Branch-Id`):**
   * Setiap request transaksional wajib memvalidasi header `X-Branch-Id: <uuid>`.
   * Middleware/Interceptor memvalidasi apakah identitas user yang diautentikasi memiliki penugasan aktif di cabang tersebut.
2. **Kontrol Konkurensi & Integritas Transaksi ACID:**
   * Pengurangan stok kasir atau mutasi inventori wajib dieksekusi dalam satu transaksi database dengan **Pessimistic Row Locking (`SELECT ... FOR UPDATE`)** atau **Optimistic Locking (`version` column)** untuk mencegah *negative stock* dan *race condition* saat barcode discan bersamaan.
3. **Standar Kredensial & Sesi:**
   * Password staf: Hash menggunakan algoritma **Argon2id** (atau Bcrypt cost factor $\ge$ 12).
   * Fast PIN Kasir: Hash kriptografi terpisah untuk otorisasi kilat dan supervisor override.
   * Token Otentikasi: **JWT RS256 / Ed25519** (Access Token: 15 menit) + **Refresh Token Rotation (RTR)** via cookie `HttpOnly; Secure; SameSite=Strict`.
4. **Scheduled Background Workers / Cron:**
   * Jadwal tugas berkala (pengecekan kedaluwarsa SIPA apoteker pada H-60/H-30/H-0, auto-flagging batch obat expired).

---

### 3.3 Spesifikasi Arsitektur Frontend [FE-WEB & FE-POS] (Tech-Agnostic)

Dokumentasikan arsitektur sisi klien secara netral tanpa dependensi library spesifik:
1. **Client State Store Architecture:**
   * Penyimpanan state reaktif in-memory untuk keranjang belanja kasir, sesi cabang aktif, dan saldo awal laci (*cash float*).
   * Pembaruan UI optimistik (*Optimistic UI*) untuk pemindaian barcode kasir dengan respon latensi < 100ms.
2. **Protokol Hardware Kasir (Front-Office POS):**
   * **Thermal Receipt Printer:** Pengiriman stream byte terstandar **ESC/POS** (lebar kertas 58mm atau 80mm), perintah pemotong kertas (*autocutter*), dan sinyal pulsa laci kasir melalui pin RJ11 (`ESC p m t1 t2`).
   * **Thermal Label Printer:** Format perintah cetak etiket obat putih (oral) dan biru (topikal/luar), ukuran standar 50x30mm.
   * **Barcode Scanner:** Listener Keyboard-Wedge dengan buffer debounce stream input < 50ms diakhiri karakter `Enter`.
3. **Registry Pintasan Keyboard Fisik Kasir (Keyboard-First):**
   * `F1`: Bantuan / Cheat Sheet Pintasan
   * `F2`: Mode Pasukan Umum (Walk-in Customer)
   * `F3`: Mode Resep Cepat (Prescription Mode)
   * `F4`: Cari Pasien Terdaftar (PMR)
   * `F8`: Konversi Walk-in ke Pasien PMR Baru
   * `F9`: Tahan Keranjang (Hold Cart)
   * `F10`: Buka Keranjang Tertahan (Recall Cart)
   * `F12` / `Space`: Buka Dialog Pembayaran
   * `Esc`: Batalkan / Tutup Modal
4. **Ketahanan Luring (Offline Resilience):**
   * Pola *Offline Outbox Queue* dan penyimpanan lokal klien (*Local Document/KV Store*) untuk katalog obat esensial dan antrean mutasi kasir saat koneksi internet terputus sementara.

---

### 3.4 Template Standar Dokumen TRD

Setiap dokumen TRD yang dibuat di folder `technical/0X-trd-*.md` WAJIB mematuhi format berikut:

```markdown
# Technical Requirements Document (TRD): [Nama Modul / Kluster Fitur]

## 1. Metadata & Traceability
- **Document Code:** TRD-PHARM-XX
- **Module Name:** [Nama Modul]
- **Target PRD References:** [Link ke Dokumen PRD di features/]
- **Target Diagram References:** [Link ke Dokumen Visual Blueprint di technical/diagrams/]
- **Target DBML References:** [Link ke technical/database/schema.dbml]
- **Target Roles:** Backend [BE], Web Admin [FE-WEB], POS Cashier [FE-POS]

## 2. Architectural Layers & Component Interactions
- Arsitektur berlapis: Transport Layer (HTTP/REST), Domain Service Layer, Data Persistence / Repository Layer, Client State Store.
- Interaksi antar-komponen dalam menangani request transaksi.

## 3. RESTful API Contracts (The Single Source of Truth)
### 3.1 [HTTP_METHOD] /api/v1/[endpoint]
- **Deskripsi & Business Rule:** (Mengimplementasikan BR-PHARM-XX-YY)
- **Header Wajib:** Authorization: Bearer <jwt>, X-Branch-Id: <uuid>, Idempotency-Key (jika mutasi)
- **Request Parameters / Body (JSON Schema):**
- **Response Success (JSON Envelope):**
- **Matriks Error & HTTP Status Codes:** (400, 401, 403, 404, 409, 422)

## 4. Concurrency, Transactional & Database Logic
- Kueri data relasional, pemanfaatan indeks, transaksi ACID, dan isolasi multi-cabang.
- Strategi pencegahan race condition (Pessimistic Row Locking `FOR UPDATE` / Optimistic Locking).
- Logika scheduled background worker (cron).

## 5. Frontend Web Admin [FE-WEB] Architecture
- Navigasi rute backoffice, validasi skema form, pagination & filtering multi-cabang, penanganan ekspor data (PDF/Excel).

## 6. Frontend POS Cashier [FE-POS] Architecture
- Pemetaan pintasan keyboard fisik, listener stream barcode scanner, format byte stream cetak ESC/POS, dan mekanisme offline outbox queue.

## 7. Domain Algorithms & State Lifecycles
- Pseudocode logika domain farmasi (contoh: algoritma pemilihan batch FEFO, perhitungan biaya racikan tuslah/embalase, rekonsiliasi selisih kasir).
- State Machine & transisi status entitas.

## 8. GitHub Issue Ticket Mapping
- Pemetaan tiket tugas implementasi: [BE Task](.github/ISSUE_TEMPLATE/backend-task.md), [FE-WEB Task](.github/ISSUE_TEMPLATE/frontend-web-task.md), [FE-POS Task](.github/ISSUE_TEMPLATE/frontend-pos-task.md).
```
