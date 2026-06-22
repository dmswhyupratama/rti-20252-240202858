# WS-11: Data Validation & Integrity

> **Bab 11 — Validasi Data & Integritas**

---

## Ringkasan Materi

### Data Trust Model

```
Raw Data → Data Cleaning → Consistency Check → Validation Process → Trusted Data
```

Data mentah belum bisa dipercaya. Harus melewati pipeline validasi sebelum siap untuk analisis statistik.

### Empat Pilar Data Quality

| Pilar | Deskripsi | Contoh Pelanggaran |
|-------|----------|-------------------|
| **Accuracy** | Nilai dalam range masuk akal | Akurasi = 1.5 (di luar [0,1]) |
| **Consistency** | Format seragam di semua run | Run 1: CSV, Run 2: JSON |
| **Completeness** | Tidak ada data hilang dari plan | 97 dari 100 run tercatat |
| **Validity** | Data sesuai desain eksperimen | Parameter baseline tercampur treatment |

### Proses Validasi Progresif

1. **Format validation** — Tipe file, header, kolom
2. **Range validation** — Nilai dalam batas logis
3. **Consistency validation** — Format seragam antar-run
4. **Logic validation** — Data cocok dengan desain eksperimen

Jika gagal di langkah awal → tidak perlu lanjut.

### Anomaly Detection — 3 Jenis

| Jenis | Deskripsi | Deteksi |
|-------|----------|---------|
| **Statistical outlier** | Nilai di luar distribusi normal | IQR: < Q1-1.5×IQR atau > Q3+1.5×IQR |
| **Contextual anomaly** | Normal absolut, abnormal dalam konteks | Run 1-10: ~91%, Run 11-20: ~88% |
| **Pattern anomaly** | Pola sistematis (bukan random) | Performa menurun berurutan |

**Prinsip:** Detect → Investigate → Document → Decide — **JANGAN langsung hapus.**

### Engineering vs Research Validation

| Aspek | Engineering | Research |
|-------|-----------|---------|
| Tujuan | Data sesuai spesifikasi bisnis | Data layak untuk analisis statistik |
| Missing data | Impute / set default | Investigasi penyebab → dokumentasi |
| Outlier | Bug → fix | Mungkin temuan → investigasi |
| Dokumentasi | Minimal (log error) | Komprehensif (anomali + keputusan) |

### Jebakan Kognitif

1. "Logging otomatis ≠ data benar" → bisa ada bug di logger
2. "Outlier = hapus" → bisa jadi temuan penting
3. "Dataset kecil tidak perlu validasi" → justru lebih rentan
4. "Mean normal = data benar" → [94, 95, 93, **44**, 94] → mean 84% terlihat wajar

---

## Template A.11 — Data Validation Checklist

DATA VALIDATION CHECKLIST

Completeness:
  [x] Semua skenario tercakup (Ad-Hoc dan BVA/EP selesai)
  [x] Jumlah run sesuai rencana
  [x] Tidak ada file output hilang
  Missing: 0 dari 5 data points

Format Consistency:
  [x] Semua file format sama (Terekam dalam format CSV)
  [x] Header konsisten (`Run_ID`, `Skenario`, `Status_Bug`, `DDR_Score`)
  [x] Tipe data konsisten (numerik tetap numerik)

Range & Logic:
  [x] Nilai dalam range masuk akal
  [x] Tidak ada waktu negatif
  [x] Metrik DDR 0–100%, tidak ada nilai di luar range (misal 110%)
  Anomali ditemukan: Satu run menghasilkan skor sangat rendah akibat cache database.

Cross-Validation:
  [x] Run identik → hasil mendekati (Skor DDR stabil di kisaran 80%)
  [x] Trend konsisten dengan ekspektasi teori (BVA/EP selalu menemukan lebih banyak bug dibanding Ad-Hoc)

Keputusan:
  [x] Data siap analisis (Setelah data outlier dibersihkan/dire-run)
  [ ] Perlu cleaning
  [ ] Perlu re-run (skenario: ____)

---

## Latihan 1 — Completeness Check

Verifikasi apakah semua data yang direncanakan sudah terkumpul.

| Skenario | Run Direncanakan | Run Tercatat | Missing | Alasan |
|----------|-----------------|-------------|---------|--------|
| Baseline (Ad-Hoc Testing) | 2 | 2 | 0 | — |
| Intervensi (BVA & EP Matrix) | 3 | 2 | 1 | Lupa me-reset (TRUNCATE) database MySQL pada Run ke-3 sehingga eksekusi ditolak oleh Primary Key |

**Total expected:** 5 | **Total actual:** 4 | **Missing:** 1

**Keputusan untuk data missing:**
> Data Run ke-3 dari kelompok BVA & EP dianulir (di-drop) karena terbukti dipengaruhi faktor eksternal (lupa reset DB). Solusinya adalah melakukan *re-run* (pengujian ulang) 1 kali dengan *database* yang dipastikan sudah bersih untuk melengkapi data yang *missing*.

---

## Latihan 2 — Anomaly Investigation

Periksa data Anda untuk anomali. Gunakan metode IQR atau z-score.

**Dataset sampel eksperimen WMS Hasil Tani (Berdasarkan Nilai Metrik DDR per Batch):**

| Run | Skor DDR (%) |
|-----|-------------|
| 1 | 82.0 |
| 2 | 80.0 |
| 3 | 85.0 |
| 4 | 20.0 |
| 5 | 78.0 |

**Deteksi outlier (Menggunakan IQR):**
- Data diurutkan: 20, 78, 80, 82, 85
- Q1 = 78.0 | Q3 = 82.0 | IQR = 4.0
- Batas bawah (Q1 - 1.5×IQR) = 78.0 - 6.0 = 72.0
- Batas atas (Q3 + 1.5×IQR) = 82.0 + 6.0 = 88.0
- Outlier terdeteksi: 20.0 (Karena 20.0 jauh di bawah batas bawah 72.0)

**Investigasi (untuk setiap outlier):**

| Outlier | Nilai | Kemungkinan Penyebab | Keputusan |
|---------|-------|---------------------|-----------|
| Run 4 | 20.0 | Sesi uji tercemar residu data. Tabel *database* urung di-TRUNCATE sebelum eksekusi, sehingga aplikasi memunculkan pesan "Error: ID Sudah Ada" (bukan Error karena lolos validasi stok limit). | Hapus log data ini. Eksekusi ulang (Re-run) Run 4 dengan memastikan mode *Incognito* dan *database* segar. |

---

## Latihan 3 — Validation Report

Buat laporan validasi ringkas untuk dataset eksperimen Anda.

**1. Completeness:** 100% data terkumpul (setelah dilakukan 1x *re-run* pada data yang *missing*).
**2. Format:** [x] Konsisten / [ ] Ada inkonsistensi: Semua file `.csv` memiliki kolom yang seragam.
**3. Range check (anomali):** Skor DDR dipastikan murni antara 0% hingga 100%. Tidak ada skor negatif atau lebih dari seratus. Outlier ekstrem di bawah 70% telah diselidiki dan dikoreksi.
**4. Logic check:** [x] Parameter sesuai plan / [ ] Ada ketidaksesuaian: Semua tes intervensi BVA/EP dijalankan dengan mematikan perlindungan HTML (`Inspect Element`).

**Kesimpulan:** [x] Data siap analisis / [ ] Perlu tindakan: Seluruh data saat ini sudah tervalidasi, bebas bias sisa *database*, dan secara sah dapat diajukan untuk uji hipotesis komparatif.

---

## Refleksi

> Apa perbedaan antara "data yang benar" dan "data yang dipercaya"? Mengapa proses validasi formal diperlukan meskipun data dikumpulkan secara otomatis?

> **Data yang benar (Correct Data)** adalah data yang dicatat tanpa ada kesalahan sintaks atau format (misalnya, aplikasi XAMPP sukses merekam *error log* secara otomatis tanpa kerusakan *file*). Sedangkan **Data yang dipercaya (Trusted Data)** adalah data yang sudah dipastikan bebas dari variabel perancu; artinya *error* yang tercatat murni berasal dari kelemahan logika *Controller* MVC, bukan karena koneksi *database* yang tiba-tiba putus atau penguji lupa me-reset tabel.
> 
> Proses validasi formal mutlak diperlukan karena sistem otomatis itu "buta konteks". Log PHP mungkin mencatat sebuah form ditolak dan dianggap "Aman", padahal penolakan itu terjadi karena penguji menggunakan *session login* yang kedaluwarsa, bukan karena algoritma FEFO-nya bekerja dengan baik. Validasi memastikan bahwa angka-angka yang akan dimasukkan ke rumus statistik memiliki makna operasional yang nyata.