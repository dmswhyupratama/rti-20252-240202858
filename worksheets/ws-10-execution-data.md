# WS-10: Experiment Execution & Data Collection

> **Bab 10 — Eksekusi Eksperimen & Pengumpulan Data**

---

## Ringkasan Materi

### Experiment Execution Pipeline

```
Design → Execution Plan → Controlled Execution → Data Collection → Data Logging → Dataset for Analysis
```

### Multiple Run = Non-Negotiable

Single run **tidak pernah cukup** untuk klaim ilmiah. Minimum 5-10 run per skenario dengan seed berbeda. Multiple run menghasilkan:
- Mean, std, confidence interval
- Distribusi hasil → uji statistik
- Variabilitas → error bar di grafik

### Execution Plan

Setiap eksperimen harus memiliki plan sebelum eksekusi:
- Daftar skenario
- Jumlah run per skenario
- Random seed per run (pre-determined!)
- Urutan eksekusi (randomisasi/counterbalancing)
- Pre-execution checklist

### Data Logging Komprehensif

Setiap run menghasilkan log terstruktur:
1. **Identitas** — Run ID, timestamp, skenario
2. **Konfigurasi** — Semua parameter, seed, code version
3. **Hasil** — Semua metrik, output detail
4. **Metadata** — Waktu eksekusi, resource usage, warning/error

Format: CSV/JSON/database — **bukan stdout yang di-copy-paste**.

### Engineering vs Research Execution

| Aspek | Engineering | Research |
|-------|-----------|---------|
| Run | Sekali (deploy) | Multiple (min 5-10, seed berbeda) |
| Logging | Error log, access log | Semua parameter, metrik, metadata |
| Anomali | Bug → fix → redeploy | Investigasi → dokumentasi → analisis |
| Urutan | Tidak penting | Bisa bias — perlu randomisasi |

### Anomali = Dokumentasi, Bukan Hapus

Run gagal/anomali tidak boleh dihapus tanpa dokumentasi. Bisa jadi:
- **Bug** → fix & re-run (dokumentasikan!)
- **Batas kemampuan metode** → DNF = temuan
- **Data yang bias** jika hanya simpan run "berhasil"

### Jebakan Kognitif

1. "Satu angka cukup" → tanpa distribusi, tidak bisa diuji
2. "Seed tidak penting" → bahkan algoritma deterministik bisa dipengaruhi library stokastik
3. "Run gagal langsung hapus" → kehilangan temuan potensial
4. "Semua run harus hari ini" → thermal throttling, fatigue

---

## Template A.10 — Execution Plan & Data Log
EXECUTION PLAN

| Run # | Skenario | Seed (Grup Data) | Parameter | Status | Waktu | Output File |
|-------|----------|------|-----------|--------|-------|-------------|
| 1     | Ad-Hoc (Baseline) | Batch-A | Truncate DB, Incognito | Planned | 30 Min | log_run1.csv |
| 2     | Ad-Hoc (Baseline) | Batch-B | Truncate DB, Incognito | Planned | 30 Min | log_run2.csv |
| 3     | BVA & EP (Intervensi) | Batch-A | Truncate DB, Incognito | Planned | 30 Min | log_run3.csv |
| 4     | BVA & EP (Intervensi) | Batch-B | Truncate DB, Incognito | Planned | 30 Min | log_run4.csv |
| 5     | BVA & EP (Intervensi) | Batch-C | Truncate DB, Incognito | Planned | 30 Min | log_run5.csv |

Jumlah runs per skenario : 2 (Ad-Hoc) dan 3 (BVA/EP)
Total runs               : 5

DATA LOG (per run):
  Run ID    : RUN-BVA-003
  Timestamp : 2026-06-25T14:30:00
  Skenario  : BVA & EP (Eksekusi TC-04 Bypass UI Input 501 Kg)
  Input     : Manipulasi DOM HTML, Input Qty = 501
  Output    : Sukses Insert ke DB. Status: BUG VALID.
  Anomali   : Tidak ada crash UI, tetapi algoritma FEFO gagal beroperasi setelahnya.
  Catatan   : Temuan celah fatal di Controller modul Outbound.
  
---
## Latihan 1 — Execution Plan

Susun execution plan untuk eksperimen Anda. Tentukan skenario, jumlah run, dan seed sebelum eksekusi.

| Run # | Skenario | Seed (Grup Uji) | Parameter Kunci | Status |
|-------|----------|------|----------------|--------|
| 1 | Baseline (Ad-Hoc Testing) | Modul Inbound | Cache Cleared, DB Reset | Planned |
| 2 | Baseline (Ad-Hoc Testing) | Modul Outbound | Cache Cleared, DB Reset | Planned |
| 3 | Intervensi (BVA & EP Matrix) | Modul Inbound | Cache Cleared, DB Reset | Planned |
| 4 | Intervensi (BVA & EP Matrix) | Modul Outbound | Cache Cleared, DB Reset | Planned |
| 5 | Uji Ulang (Repeatability BVA) | Modul Outbound | Validasi TC-04 Upper Boundary | Planned |

**Total skenario:** 2 (Ad-Hoc vs BVA/EP)
**Run per skenario:** 2 run dasar + 1 run validasi khusus
**Total run keseluruhan:** 5

---

## Latihan 2 — Data Log Terstruktur

Desain format data log untuk eksperimen Anda. Tentukan field apa saja yang akan dicatat.

**Identitas:**
| Field | Contoh |
|-------|--------|
| Run ID | *BVA-OUT-001* |
| Timestamp | *2026-06-25 10:15:00* |
| Target Modul | *Form Sales Order (Outbound)* |

**Konfigurasi:**
| Field | Contoh |
|-------|--------|
| Skenario Uji | *Equivalence Partitioning (EP) - Input Tipe Huruf* |
| Code Version | *WMS v1.0 (Stable)* |
| Kondisi DB | *Fresh (Truncated)* |

**Hasil:**
| Metrik | Tipe Data | Range Valid |
|--------|----------|-------------|
| Status Temuan (Bug) | *Boolean* | *True (Bug Valid) / False (Aman)* |
| Kategori Error | *String* | *Fatal Error / Logical Error / No Error* |
| Defect Detection Rate (DDR) | *Float* | *0.0 - 100.0 (%)* |

**Format output:** [x] CSV / [ ] JSON / [ ] Database / [ ] Lainnya: ____

---

## Latihan 3 — Anomaly Protocol

Rencanakan bagaimana menangani anomali. Untuk setiap jenis, tentukan langkah yang diambil.

| Jenis Anomali | Contoh | Tindakan |
|---------------|--------|----------|
| Run gagal (crash) | *Blank White Screen (BWS) pada PHP karena infinite loop FEFO* | *Buka `php_error.log` di XAMPP, catat stack trace sebagai Bug Valid, restart Apache server, lanjut ke run berikutnya.* |
| Hasil ekstrem | *Skor DDR Ad-Hoc 100% (Semua bug ketemu tanpa matriks)* | *Investigasi kemungkinan "Testing Bias" (penguji Ad-hoc sudah hafal celah sistem). Dokumentasikan bias ini di laporan akhir.* |
| Waktu eksekusi anomali | *Loading form melebihi 30 detik saat eksekusi input nilai minus (-100)* | *Catat sebagai indikasi kelemahan optimasi query Controller. Tandai run untuk diulang (re-run).* |
| Inkonsistensi dengan run lain | *Run 2 gagal insert DB, padahal Run 1 dengan input yang sama berhasil.* | *Investigasi prosedur pembersihan. Kemungkinan lupa eksekusi TRUNCATE sehingga tertolak oleh Primary Key duplicate. Hapus log Run 2, bersihkan DB, dan re-run.* |

**Prinsip:** Detect → Investigate → Document → Decide

---

## Refleksi

> Pernahkah Anda melaporkan hasil riset/tugas dari single run? Apa risikonya? Bagaimana multiple run mengubah kepercayaan terhadap hasil?

**Pengalaman sebelumnya:**
> Pada tugas-tugas mata kuliah sebelumnya, saya terbiasa hanya menguji aplikasi satu kali (*single run*). Jika skenario input berhasil diproses tanpa memunculkan *error* di layar, saya langsung men-*screenshot* layarnya dan menyimpulkannya sebagai "Sistem Berjalan Normal" untuk dilaporkan di dokumen laporan.

**Yang akan dilakukan berbeda:**
> Risikonya sangat fatal karena *single run* tidak bisa mendeteksi *flaky bug* (kutu yang kadang muncul dan kadang tidak akibat *state database* yang menumpuk). Ke depannya, dengan menerapkan *multiple run* dan me-*reset environment* (TRUNCATE tabel MySQL) pada setiap pergantian sesi, kepercayaan terhadap metrik *Defect Detection Rate* (DDR) menjadi valid dan terbukti tidak dipengaruhi oleh sisa residu dari pengujian sebelumnya.