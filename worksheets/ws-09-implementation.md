# WS-09: Implementation & Environment

> **Bab 9 — Implementasi Riset & Kontrol Lingkungan**

---

## Ringkasan Materi

### Implementasi Riset ≠ Coding Biasa

Tujuan implementasi riset bukan membuat software yang berfungsi, melainkan membangun **instrumen pengukuran yang konsisten**. Setiap modul harus di-mapping ke variabel (dari Bab 6), parameter harus config-driven, dan logging aktif dari hari pertama.

### Reproducible Implementation Model

```
Design → Implementation → Environment Setup → Execution Consistency → Reproducibility → Trustworthy Result
```

Setiap transisi memiliki syarat:
- Design → Implementation: kode sesuai mapping variabel-ke-komponen
- Implementation → Environment: versi, dependency, seed, path, OS eksplisit
- Environment → Consistency: seed terkunci, urutan deterministik
- Consistency → Reproducibility: dokumentasi lengkap
- Reproducibility → Trust: siapa pun ikuti dokumentasi → hasil sama/serupa

### Repeatability vs Reproducibility

| Level | Peneliti | Environment | Hasil |
|-------|---------|-------------|-------|
| **Repeatability** | Sama | Sama | Sama persis |
| **Reproducibility** | Berbeda | Berbeda (ikuti docs) | Sama/serupa |

Capai **repeatability** dulu, baru **reproducibility**.

### Engineering vs Research Perspective

| Aspek | Engineering | Research |
|-------|-----------|---------|
| Tujuan | Sistem berfungsi untuk user | Instrumen pengukuran konsisten |
| Dependency | Update ke terbaru | Lock di versi spesifik |
| Testing | Unit, integration, E2E | Repeatability test (run ulang → sama?) |
| Dokumentasi | User guide, API docs | Environment spec, execution steps, expected output |
| Config | Default masuk akal | Setiap parameter eksplisit & adjustable |

### Jebakan Kognitif

1. Menunda environment setup → bug sulit dilacak
2. Tidak pakai version control → hasil tidak bisa direkonstruksi
3. Menolak Docker/container → "di laptop saya bisa" saat review
4. 3× hasil sama ≠ repeatable (bisa cache/state tersimpan)

### Istilah Penting

- **Environment Specification** — Deskripsi lengkap: hardware, OS, runtime, library + versi, config, seed
- **Dependency** — Komponen eksternal yang harus di-lock versinya
- **Config-driven** — Parameter dieksternalisasi ke file konfigurasi, bukan hardcode

---

## Template A.9 — Dokumentasi Setup Eksperimen

EXPERIMENT SETUP DOCUMENTATION

Hardware:
  CPU     : Intel Core Ultra 5 125H
  RAM     : 16 GB
  GPU     : iGPU Intel ARC Graphics
  Storage : 512 GB SSD

Software:
  OS        : Windows 11 64-bit
  Runtime   : XAMPP Server v3.3.0 (Apache & MySQL)
  Framework : PHP Native MVC Architecture

Dependencies:
| Library | Version | Sumber | Hash/Checksum |
|---------|---------|--------|---------------|
| PHP Engine | 8.1.x | Bawaan XAMPP | - |
| MySQL Server | 10.4.x | Bawaan XAMPP | - |
| Web Browser | Google Chrome v120+ | Official Installer | - |

Konfigurasi:
  Config file     : `app/config/database.php`
  Random seed     : Tidak menggunakan RNG. Input berdasarkan Dokumen Matriks Skenario Uji.
  Hyperparameters : `error_reporting(E_ALL)` diaktifkan.

Reproducibility Check:
  [x] Dependency terdokumentasi (requirements.txt / lock file) -> *Tergantikan oleh spek XAMPP*
  [x] Seed ditetapkan di semua level (Python, NumPy, framework) -> *Input matriks dikunci*
  [x] Config di version control
  [x] README instruksi reproduksi lengkap

---

## Latihan 1 — Environment Specification

Dokumentasikan environment untuk eksperimen Anda (Sistem Manajemen Gudang Hasil Tani).

| Komponen | Spesifikasi |
|----------|------------|
| CPU | Intel Core Ultra 5 125H |
| RAM | 16 GB |
| GPU | iGPU Intel ARC Graphics |
| OS | Windows 11 64-bit |
| Runtime | XAMPP (Apache HTTP Server & MySQL) |
| Framework | PHP Native MVC |
| Random Seed | N/A (Menggunakan input manual statis dari TC-04) |

**Dependencies (minimal 5):**

| Library | Version | Alasan Dibutuhkan |
|---------|---------|-------------------|
| PHP Engine | 8.1.x | Memproses logika Controller dan algoritma FEFO |
| MySQL/MariaDB | 10.4.x | Menyimpan data tabel stok dan transaksi SO |
| Apache | 2.4.x | Web server lokal untuk menjalankan aplikasi |
| Google Chrome | 120+ | Eksekusi UI dan memanipulasi DOM HTML (Inspect Element) |
| phpMyAdmin | 5.2.x | Manajemen database (Truncate/Reset tabel antar sesi uji) |

---

## Latihan 2 — Repeatability Test Plan

Rancang tes repeatability sederhana: jalankan kode yang sama 3× di environment yang sama.
*(Skenario: Eksekusi TC-04 BVA Batas Maksimal - Bypass UI 501 Kg)*

| Run | Seed | Metrik Utama (Respons Sistem) | Hasil Sama? |
|-----|------|-------------|-------------|
| 1 | *Input: 501* | *Bug Valid: Sistem berhasil save 501 Kg ke database* | — |
| 2 | *Input: 501* | *Bug Valid: Sistem berhasil save 501 Kg ke database* | [x] Ya / [ ] Tidak |
| 3 | *Input: 501* | *Bug Valid: Sistem berhasil save 501 Kg ke database* | [x] Ya / [ ] Tidak |

**Jika hasil berbeda, kemungkinan penyebab:**
> Sesi pengujian tidak di-reset. Jika *Run* 2 gagal (sistem menolak input) padahal *Run* 1 berhasil, itu kemungkinan karena tabel *database* transaksi belum di-TRUNCATE, sehingga sistem memblokir karena deteksi ID Pesanan ganda (*Duplicate Entry*), bukan karena validasi logika kuantitas stok.

**Checklist kontrol yang sudah diterapkan:**
- [x] Random seed di-set di semua level *(Input dikunci wajib 501)*
- [x] Tidak ada background process yang mengganggu
- [x] Cache dibersihkan antar-run *(Wajib Truncate DB & Mode Incognito)*
- [x] Config file yang sama untuk semua run

---

## Latihan 3 — README Eksperimen

Tulis README minimum untuk eksperimen Anda (6 komponen wajib).

```text
# Judul Eksperimen: Evaluasi Defect Detection Rate pada Controller MVC WMS Hasil Tani

## 1. Environment
- OS: Windows 11 64-bit
- Runtime: XAMPP (PHP 8.1, MySQL 10.4)
- Browser: Google Chrome

## 2. Installation
- Pindahkan folder source code WMS ke `C:\xampp\htdocs\`.
- Jalankan Apache dan MySQL dari XAMPP Control Panel.
- Buat database `db_hasiltani` di phpMyAdmin, lalu import file `database_empty.sql`.

## 3. Data
Tidak menggunakan dataset otomatis. Data pengujian diinput manual berdasarkan Dokumen Skenario Uji. Skenario utama: TC-04 (Pisang Cavendish, Stok Awal 500 Kg, Input Permintaan 501 Kg).

## 4. Execution
- Buka Chrome, login sebagai Sales ke `http://localhost/wms/public`.
- Masuk ke menu Buat Pesanan Baru.
- Gunakan fitur "Inspect Element" pada browser untuk menghapus atribut `max="500"` di HTML.
- Masukkan input ekstrem (501) lalu Simpan.
- PENTING: Antar percobaan, jalankan query `TRUNCATE TABLE transaksi_outbound` untuk reset env.

## 5. Configuration
Koneksi database pada `app/config/database.php` menggunakan user: root dan password: (kosong).

## 6. Expected Output
Sistem MVC yang rentan akan meloloskan data 501 ke dalam database (menyebabkan status "Pending" pada dashboard Admin). Temuan ini dicatat sebagai Bug Valid (Fail) untuk metrik DDR.