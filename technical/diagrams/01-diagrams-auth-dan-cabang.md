# Blueprint Visual: Diagram Usecase, Alur Proses & Scoped ERD
# Modul: Autentikasi Dual-UX, Hak Akses (RBAC) & Sesi Cabang

---

## 1. Metadata Dokumen

| Properti | Keterangan |
| :--- | :--- |
| **Kode Dokumen** | **DIAG-PHARM-AUTH01** |
| **Nama Modul** | Diagram Alur Proses, Usecase, Sequence, dan Scoped ERD untuk Autentikasi & User |
| **Dokumen Bisnis Acuan** | • [`features/01-prd-auth-user.md`](../../features/01-prd-auth-user.md)<br>• [`features/master-data/01-prd-cabang-dan-gudang.md`](../../features/master-data/01-prd-cabang-dan-gudang.md) |
| **Skema Database Acuan** | [`technical/database/schema.dbml`](../database/schema.dbml) (Grup `Auth_And_Staff` & `Core_MultiBranch`) |
| **Audiens Target** | UI/UX Designer, Frontend POS & Web Engineers, Backend Engineers, QA Engineers, System Analyst |

---

## 2. Diagram Usecase (Aktor vs Fungsionalitas Sistem)

Diagram ini memetakan batas kewenangan 6 persona staf apotek terhadap fungsi-fungsi autentikasi, legalitas, dan sesi cabang:

```mermaid
flowchart LR
    subgraph ACTORS [Aktor Staf Apotek]
        Kasir[Kasir Front-Office]
        TTK[Asisten Apoteker TTK]
        APA[Apoteker Pengelola APA]
        Gudang[Staf Gudang / Logistik]
        Owner[Owner / Keuangan]
        SuperAdmin[Super Admin IT]
    end

    subgraph SYSTEM [Sistem Autentikasi dan Sesi Cabang]
        UC_FastLogin([UC-01: Fast PIN / Barcode Login POS])
        UC_WebLogin([UC-02: Email dan Password Login ERP])
        UC_BranchSelect([UC-03: Memilih Cabang Aktif])
        UC_SupervisorOverride([UC-04: Persetujuan Supervisor Override])
        UC_ManageLicense([UC-05: Kelola Izin SIPA / STRTTK])
        UC_ManageStaff([UC-06: Kelola Staf dan Penugasan Cabang])
        UC_AuditAuth([UC-07: Audit Log dan Aktivitas Override])
    end

    Kasir --> UC_FastLogin
    Kasir -.->|Memicu saat void atau diskon| UC_SupervisorOverride

    TTK --> UC_FastLogin
    TTK --> UC_WebLogin
    TTK --> UC_BranchSelect

    APA --> UC_FastLogin
    APA --> UC_WebLogin
    APA --> UC_BranchSelect
    APA --> UC_SupervisorOverride
    APA --> UC_ManageLicense

    Gudang --> UC_WebLogin
    Gudang --> UC_BranchSelect

    Owner --> UC_WebLogin
    Owner --> UC_BranchSelect
    Owner --> UC_SupervisorOverride
    Owner --> UC_AuditAuth

    SuperAdmin --> UC_WebLogin
    SuperAdmin --> UC_ManageStaff
    SuperAdmin --> UC_AuditAuth

    UC_FastLogin -.->|include| UC_BranchSelect
    UC_WebLogin -.->|include| UC_BranchSelect
```

---

## 3. Diagram Alur Proses Sistem (Detailed Flowcharts)

### 3.1 Flowchart 1: Alur Login Dual-UX (Terminal POS Kasir vs Web Admin ERP)

Menerjemahkan aturan login cepat di meja kasir (*Zero Lag*) vs login aman dengan verifikasi ketat di back-office ERP:

```mermaid
flowchart TD
    Start([Pengguna Mengakses Sistem]) --> DetectClient{Deteksi Perangkat / Antarmuka?}
    
    %% Cabang 1: Terminal POS Kasir
    DetectClient -->|Layar Kasir POS / Tablet| POS_Mode["Mode Layar Sentuh / Kasir Cepat"]
    POS_Mode --> ChooseInput{Metode Input Staf?}
    ChooseInput -->|Scan Kartu Barcode| ReadBarcode["Barcode Scanner Membaca ID Card"]
    ChooseInput -->|Numpad Layar / Keyboard| InputPIN["Staf Mengetik 4-6 Digit PIN Cepat"]
    ReadBarcode --> VerifyPOSAuth["Backend Mencocokkan Hash Barcode / PIN"]
    InputPIN --> VerifyPOSAuth
    
    VerifyPOSAuth --> CheckPOSValid{Kredensial Valid & User Aktif?}
    CheckPOSValid -->|Tidak| ShowPOSErr["Tampilkan Error: 'PIN / Barcode Salah'"]
    ShowPOSErr --> POS_Mode
    
    CheckPOSValid -->|Ya| CheckPOSBranch{"Apakah Staf Ditugaskan di Cabang Ini?"}
    CheckPOSBranch -->|Tidak Ditugaskan| RejectBranch["Tolak Login: 'Anda tidak memiliki penugasan aktif di cabang ini'"]
    RejectBranch --> POS_Mode
    CheckPOSBranch -->|Ditugaskan| IssuePOSToken["Terbitkan Token Sesi Kasir Terikat X-Branch-Id"]
    IssuePOSToken --> OpenDrawerCheck{Apakah Kasir Membuka Shift Baru?}
    OpenDrawerCheck -->|Ya| InputFloat["Input Modal Awal Kasir (Cash Float)"]
    OpenDrawerCheck -->|Tidak| GoToPOSCart["Buka Layar Transaksi Penjualan"]
    InputFloat --> GoToPOSCart
    GoToPOSCart --> EndPOS([Kasir Siap Melayani Transaksi])

    %% Cabang 2: Web Admin ERP Backoffice
    DetectClient -->|Peramban Web Browser / ERP| Web_Mode["Halaman Login Web ERP"]
    Web_Mode --> InputCreds["Input Alamat Email & Kata Sandi"]
    InputCreds --> VerifyWebAuth["Backend Memvalidasi Email & Hash Argon2id"]
    VerifyWebAuth --> CheckWebValid{Email & Password Cocok?}
    CheckWebValid -->|Tidak| WebError["Tampilkan Error: 'Kredensial Tidak Valid'"]
    WebError --> Web_Mode
    
    CheckWebValid -->|Ya| FetchBranches["Ambil Daftar Cabang yang Berhak Diakses Staf"]
    FetchBranches --> BranchCountCheck{Jumlah Cabang Terdaftar?}
    BranchCountCheck -->|1 Cabang Saja| AutoSelectBranch["Otomatis Pilih Cabang Tunggal"]
    BranchCountCheck -->|Multi-Cabang (Lebih dari 1 Cabang)| ModalSelectBranch["Munculkan Dialog Pemilihan Cabang Kerja"]
    ModalSelectBranch --> UserSelectBranch["Pengguna Memilih Cabang yang Akan Dioperasikan"]
    UserSelectBranch --> IssueWebToken["Terbitkan JWT RS256 + Refresh Token"]
    AutoSelectBranch --> IssueWebToken
    IssueWebToken --> LoadDashboard["Buka Dashboard ERP Sesuai Hak Akses RBAC"]
    LoadDashboard --> EndWeb([Staf Siap Bekerja di Web ERP])
```

---

### 3.2 Flowchart 2: Alur Persetujuan Supervisor (*Manager Override*) di Meja Kasir

Mekanisme otorisasi instan saat kasir melakukan tindakan sensitif (membatalkan/void item yang sudah di-scan, memberikan diskon di luar wewenang kasir, atau retur obat):

```mermaid
flowchart TD
    TriggerAction([Kasir Menekan Tombol Void Item / Diskon Khusus]) --> CheckLimit{Apakah Tindakan Memerlukan Wewenang Supervisor?}
    
    CheckLimit -->|Tidak| ExecuteAction["Sistem Langsung Mengeksekusi Perubahan"]
    ExecuteAction --> ResumeSale([Lanjutkan Penjualan])
    
    CheckLimit -->|Ya, Butuh Otorisasi| LockCart["Kunci Layar Kasir Sementara & Tampilkan Modal Pop-up Override"]
    LockCart --> SupervisorInput["Supervisor / Apoteker Mengetik PIN Otorisasi Khusus"]
    SupervisorInput --> VerifySupervisor["Sistem Memvalidasi PIN Terhadap Akun Ber-role Supervisor/Apoteker"]
    
    VerifySupervisor --> IsSupervisorValid{PIN Valid & Memiliki Hak can_supervisor_override?}
    IsSupervisorValid -->|Tidak Valid / Bukan Supervisor| ShowOverrideFail["Tampilkan Peringatan: 'PIN Otorisasi Ditolak / Tidak Berwenang'"]
    ShowOverrideFail --> RetryCountCheck{Percobaan Gagal 3 Kali atau Lebih?}
    RetryCountCheck -->|Ya| CancelOverrideAction["Batalkan Tindakan Sensitif & Kirim Notifikasi Peringatan ke Owner"]
    CancelOverrideAction --> UnlockCartFail["Buka Kunci Layar Kasir (Item Tetap Ada)"]
    UnlockCartFail --> ResumeSale
    RetryCountCheck -->|Tidak| SupervisorInput
    
    IsSupervisorValid -->|Valid| LogAudit["Catat Jejak Audit Universal:<br/>• ID Kasir yang meminta<br/>• ID Supervisor yang mengizinkan<br/>• Waktu & Alasan Tindakan"]
    LogAudit --> ApplyChange["Terapkan Void Item / Terapkan Diskon Khusus"]
    ApplyChange --> UnlockCartSuccess["Buka Kunci Layar Kasir dengan Status Disetujui"]
    UnlockCartSuccess --> ResumeSale
```

---

### 3.3 Flowchart 3: Siklus Pemantauan Masa Berlaku Legalitas Profesi (SIPA / STRTTK)

Memastikan apotek selalu terlindungi dari sanksi Dinkes/BPOM terkait legalitas izin praktik:

```mermaid
flowchart TD
    CheckCron([Pengecekan Harian Otomatis Masa Berlaku SIPA/STRTTK]) --> CalcDays["Hitung Selisih Hari: Tanggal Habis Izin - Hari Ini"]
    
    CalcDays --> EvaluateStatus{Kategori Sisa Hari?}
    
    EvaluateStatus -->|Lebih dari 60 Hari| StatusGreen["Status: HIJAU (Valid & Aman)"]
    StatusGreen --> EndCheck([Tidak Ada Tindakan])
    
    EvaluateStatus -->|Antara 30 sampai 60 Hari| StatusYellow["Status: KUNING (Peringatan Awal)"]
    StatusYellow --> NotifyAPA["Tampilkan Notifikasi Peringatan di Dashboard Admin:<br/>Masa Berlaku Izin SIPA Akan Berakhir dalam X Hari"]
    NotifyAPA --> EndCheck
    
    EvaluateStatus -->|Antara 1 sampai 30 Hari| StatusOrange["Status: ORANYE (Mendesak / Perpanjangan)"]
    StatusOrange --> AlertUrgent["Tampilkan Alert Oranye Banner di Seluruh Sesi Apoteker"]
    AlertUrgent --> EndCheck
    
    EvaluateStatus -->|Kedaluwarsa (0 Hari atau Kurang)| StatusRed["Status: MERAH (Kedaluwarsa / Expired)"]
    StatusRed --> LockSP["KUNCI OTOMATIS: Dilarang Menerbitkan Surat Pesanan (SP) Obat Keras/Narkotika"]
    LockSP --> RequireExtension["Wajib Unggah Nomor & Masa Berlaku SIPA Baru untuk Membuka Kunci"]
    RequireExtension --> EndCheck
```

---

## 4. Sequence Diagram (Interaksi Teknis Antar Komponen)

### 4.1 Sequence 1: Login Cepat Kasir POS (Fast PIN Authentication)

```mermaid
sequenceDiagram
    autonumber
    actor Cashier as Kasir Front-Office
    participant POS as Terminal Kasir (PWA / POS Client)
    participant Gateway as API Gateway (Reverse Proxy)
    participant AuthService as Backend Auth Service
    participant DB as Database (PostgreSQL 15+)

    Cashier->>POS: Ketik 6-Digit PIN & Tekan Enter
    POS->>POS: Ambil Nilai Header `X-Branch-Id` Lokal
    POS->>Gateway: POST /api/v1/auth/pos/login-pin (Payload: pin, branch_id)
    Gateway->>AuthService: Forward Request + Validasi Header
    AuthService->>DB: SELECT * FROM users WHERE pin_hash = crypt(pin, ...) AND is_active = true
    DB-->>AuthService: Return Data Staf (id, role, full_name)
    
    alt Kredensial Tidak Ditemukan
        AuthService-->>POS: HTTP 401 Unauthorized { success: false, error: "PIN Salah" }
        POS-->>Cashier: Animasi Numpad Merah "PIN Salah"
    else Kredensial Valid
        AuthService->>DB: SELECT * FROM user_branches WHERE user_id = :id AND branch_id = :branch_id
        DB-->>AuthService: Return Status Penugasan Cabang
        
        alt Tidak Ditugaskan di Cabang Ini
            AuthService-->>POS: HTTP 403 Forbidden { success: false, error: "Tidak berwenang di cabang ini" }
            POS-->>Cashier: Pesan Error "Penugasan Cabang Tidak Sesuai"
        else Penugasan Sah
            AuthService->>AuthService: Generate JWT Token (Claims: user_id, role, branch_id)
            AuthService->>DB: INSERT INTO auth_audit_logs (user_id, branch_id, event_type, created_at)
            AuthService-->>POS: HTTP 200 OK { access_token, user_profile, active_branch }
            POS->>POS: Simpan Token di Memory Storage (Secured)
            POS-->>Cashier: Buka Layar Kasir Penjualan Siap Pakai
        end
    end
```

---

### 4.2 Sequence 2: Dialog Otorisasi Supervisor (*Manager Override Pop-Up*)

```mermaid
sequenceDiagram
    autonumber
    actor Cashier as Kasir
    actor Supervisor as Apoteker / Supervisor
    participant POS as Layar Kasir POS
    participant AuthService as Backend Auth Service
    participant AuditDB as Database Audit Log

    Cashier->>POS: Klik Tombol "Hapus Item (Void)"
    POS->>POS: Evaluasi Rule Lokal: Item di atas batas nominal Wajib Otorisasi
    POS-->>Cashier: Munculkan Modal Pop-Up "PIN Otorisasi Supervisor Diperlukan"
    
    Supervisor->>POS: Ketik PIN Otorisasi Supervisor
    POS->>AuthService: POST /api/v1/pos/supervisor-override (Payload: supervisor_pin, action_type, branch_id)
    
    AuthService->>AuthService: Verifikasi PIN & Cek Role (can_supervisor_override = true)
    
    alt PIN Tidak Valid atau Bukan Supervisor
        AuthService-->>POS: HTTP 403 Forbidden { error: "Otorisasi Ditolak" }
        POS-->>Cashier: Tampilkan Pesan "PIN Supervisor Tidak Sah"
    else Otorisasi Disetujui
        AuthService->>AuditDB: Catat Log (action: VOID, cashier_id, supervisor_id, timestamp)
        AuthService-->>POS: HTTP 200 OK { authorized: true, supervisor_name: "Apt. Sarah" }
        POS->>POS: Eksekusi Penghapusan Item dari Keranjang
        POS-->>Cashier: Tutup Modal & Perbarui Total Belanja Kasir
    end
```

---

## 5. Diagram Status & Siklus Hidup (State Lifecycle Diagrams)

### 5.1 Siklus Status Akun Staf (`users`)

```mermaid
stateDiagram-v2
    [*] --> DRAFT : Pendaftaran Akun Staf Baru oleh HR/Admin
    DRAFT --> ACTIVE : Aktivasi Akun, Pengisian PIN & Penugasan Cabang
    
    state ACTIVE {
        [*] --> IDLE
        IDLE --> LOGGED_IN : Login Berhasil
        LOGGED_IN --> IDLE : Logout / Session Expired
    }

    ACTIVE --> SUSPENDED : Pelanggaran / Cuti Panjang / Penonaktifan Sementara
    SUSPENDED --> ACTIVE : Diaktifkan Kembali oleh Supervisor / Owner
    
    ACTIVE --> TERMINATED : Karyawan Resign / Berhenti Bekerja
    SUSPENDED --> TERMINATED : Pemutusan Hubungan Kerja Permanen
    
    TERMINATED --> [*] : Data Diarsip (Soft Delete - deleted_at)
```

---

### 5.2 Siklus Sesi Kerja Kasir di Cabang (`user_branches` & Kasir POS)

```mermaid
stateDiagram-v2
    [*] --> LOGGED_OUT : Aplikasi Terminal POS Siaga

    LOGGED_OUT --> AUTHENTICATED : PIN Valid & Cabang Terkonfirmasi
    
    AUTHENTICATED --> SHIFT_OPENED : Kasir Menginput Modal Awal (Cash Float)
    
    state SHIFT_OPENED {
        [*] --> STANDBY
        STANDBY --> SERVING_SALE : Scan Item / Input Resep
        SERVING_SALE --> PAYMENT_PROCESSING : Pilih Metode Bayar (Cash/QRIS)
        PAYMENT_PROCESSING --> STANDBY : Struk Tercetak & Laci Terbuka
        
        SERVING_SALE --> OVERRIDE_LOCKED : Trigger Void / Diskon Besar
        OVERRIDE_LOCKED --> SERVING_SALE : Supervisor Input Valid PIN
    }

    SHIFT_OPENED --> SHIFT_CLOSED : Tutup Shift, Hitung Uang Fisik Kasir & Cetak X/Z Report
    SHIFT_CLOSED --> LOGGED_OUT : Sesi Kasir Selesai
```

---

## 6. Scoped ERD: Khusus Domain Autentikasi, Staf & Cabang

Berikut adalah sub-diagram relasi database PostgreSQL khusus untuk domain **Auth, User, Profil Profesi, dan Cabang** (diambil secara terfokus dari [`technical/database/schema.dbml`](../database/schema.dbml)):

```mermaid
erDiagram
    BRANCHES ||--o{ BRANCH_STORAGE_RACKS : "memiliki lokasi rak simpan"
    BRANCHES ||--o{ USER_BRANCHES : "menugaskan staf"
    BRANCHES ||--o{ PHARMACIST_PROFILES : "lokasi penugasan SIPA resmi"

    ROLES ||--o{ USERS : "mengelompokkan hak akses"
    USERS ||--o{ USER_BRANCHES : "ditempatkan pada"
    USERS ||--o| PHARMACIST_PROFILES : "memiliki izin profesi"

    BRANCHES {
        uuid id PK
        varchar branch_code UK "Kode cabang (AP-MLW-01)"
        varchar branch_name "Nama apotek/gudang"
        enum branch_type "RETAIL / CLINIC / CENTRAL_WAREHOUSE"
        varchar sia_number "Nomor izin SIA"
        date sia_expired_date "Masa berlaku izin SIA"
        boolean is_active "Status operasional"
        timestamptz created_at
        timestamptz updated_at
        uuid created_by
        timestamptz deleted_at
    }

    BRANCH_STORAGE_RACKS {
        uuid id PK
        uuid branch_id FK "Invarian Cabang"
        varchar rack_code "Kode fisik rak (RAK-A-01)"
        varchar zone_area "Zona simpan obat"
        boolean is_locked "Kunci lemari psiko/narko"
    }

    ROLES {
        uuid id PK
        varchar role_code UK "SUPER_ADMIN / APOTEKER / KASIR"
        varchar role_name "Label tampilan role"
        boolean can_supervisor_override "Izin PIN override"
    }

    USERS {
        uuid id PK
        varchar employee_code UK "Nomor induk staf"
        varchar full_name "Nama staf"
        varchar email UK "Email login Web ERP"
        varchar password_hash "Hash kata sandi Web"
        varchar pin_hash "Hash PIN cepat POS 4-6 digit"
        varchar barcode_card UK "Barcode ID card staf"
        uuid role_id FK
        boolean is_active "Status aktif staf"
    }

    USER_BRANCHES {
        uuid id PK
        uuid user_id FK "Staf apotek"
        uuid branch_id FK "Cabang penugasan"
        boolean is_default "Cabang utama"
        boolean can_operate_pos "Wewenang buka kasir"
    }

    PHARMACIST_PROFILES {
        uuid id PK
        uuid user_id FK "User Apoteker / TTK"
        varchar license_type "SIPA atau STRTTK"
        varchar license_number "Nomor izin resmi"
        date license_expired_date "Masa aktif izin"
        boolean is_apa "Penanggung Jawab (1 Cabang = 1 APA)"
        uuid assigned_branch_id FK "Cabang tempat SIPA terdaftar"
    }
```

---

## 7. Rangkuman Keterkaitan Antara Diagram dengan Dokumen Spesifikasi

| Nama Diagram | Aturan Bisnis yang Direpresentasikan (PRD) | Dampak Implementasi Koding (TRD / Database) |
| :--- | :--- | :--- |
| **Usecase Diagram (Bab 2)** | `BR-PHARM-AUTH-01` s/d `BR-PHARM-AUTH-05` (Matriks 6 persona RBAC) | Menentukan rute menu Frontend Web, middleware otorisasi API, dan guards di POS. |
| **Flowchart Login Dual-UX (Bab 3.1)** | `BR-PHARM-AUTH-01` (Fast PIN vs Email Login) & `BR-PHARM-BR-03` (Isolasi Cabang) | Endpoint `POST /auth/pos/login-pin` vs `POST /auth/web/login`, validasi header `X-Branch-Id`. |
| **Flowchart Supervisor Override (Bab 3.2)**| `BR-PHARM-AUTH-03` (Otorisasi Pembatalan & Diskon) | Endpoint `POST /pos/supervisor-override` & pencatatan tabel audit khusus. |
| **SIPA License Lifecycle (Bab 3.3)** | `BR-PHARM-AUTH-02` & `BR-PHARM-BR-02` (Legalitas Profesi & 1 Apotek 1 APA) | Cron-job peringatan kedaluwarsa SIPA & penguncian modul Surat Pesanan (SP). |
| **Scoped ERD (Bab 6)** | Seluruh entitas pengguna, cabang, dan relasi peran | DDL skema database PostgreSQL pada berkas `technical/database/schema.dbml`. |
