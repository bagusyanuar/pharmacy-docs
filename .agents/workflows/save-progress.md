---
name: save-progress
description: Checkpoint dan simpan progres pengerjaan dokumentasi Pharmacy POS & ERP, perbarui PROGRESS.md, sinkronkan Knowledge Graph (Graphify), dan siapkan git commit.
---

# Workflow: /save-progress

Gunakan workflow ini setiap kali pengguna ingin mengakhiri sesi kerja atau membuat titik simpan (*checkpoint*) proyek.

---

## Langkah-Langkah Eksekusi Otomatis

### 1. Periksa Status Berkas & Perubahan Sesi Ini
Jalankan pemeriksaan berkas yang baru dibuat atau diubah:
```bash
git status -s
```

### 2. Perbarui Dokumen Pelacak (`PROGRESS.md`)
* Periksa dokumen apa saja yang telah berhasil diselesaikan dalam sesi ini.
* Tandai checklist `[x]` pada dokumen terkait di [PROGRESS.md](file:///Users/dystopia/projects/pharmacy/pharmacy-docs/PROGRESS.md).
* Perbarui bagian **Antrean Pengerjaan Berikutnya** jika ada urutan atau kisi-kisi baru.
* Perbarui cap waktu **Tanggal Pembaruan Terakhir**.

### 3. Sinkronkan Hub Navigasi (`README.md`)
* Pastikan pohon struktur berkas dan tautan dokumen di [README.md](file:///Users/dystopia/projects/pharmacy/pharmacy-docs/README.md) sudah mencerminkan semua berkas baru di `features/` dan `technical/`.

### 4. Sinkronisasikan Knowledge Graph (Graphify)
Eksekusi pembaruan graf pengetahuan agar semua berkas, entitas, dan relasi baru terindeks:
```bash
graphify update .
```

### 5. Buat Titik Simpan Git (Commit)
Siapkan commit git dengan pesan deskriptif:
```bash
git add .
git commit -m "checkpoint: update progress tracker, technical specs, and knowledge graph"
```

### 6. Berikan Laporan Penutup ke Pengguna
Tampilkan ringkasan singkat:
* Jumlah dokumen yang tersimpan.
* Status Knowledge Graph (Nodes, Edges, Communities).
* *Prompt* satu kalimat yang bisa digunakan pengguna untuk melanjutkan pengerjaan di sesi berikutnya:
  > *"Halo bro, tolong baca `PROGRESS.md` dan kita lanjutkan ke [Nama Modul Berikutnya]."*
