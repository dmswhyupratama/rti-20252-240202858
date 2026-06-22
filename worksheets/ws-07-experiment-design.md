# WS-07: Experimental Design & Validity

> **Bab 7 — Experimental Design & Validity**

---

## Ringkasan Materi

### Correlation ≠ Causality

Kausalitas membutuhkan 3 syarat:
1. **Covariance** — X dan Y bergerak bersama
2. **Temporal precedence** — X berubah sebelum Y
3. **Elimination of alternatives** — Tidak ada faktor lain yang menjelaskan Y

Controlled experiment adalah satu-satunya metode yang bisa membuktikan kausalitas.

### Empat Jenis Validitas

| Jenis | Pertanyaan | Ancaman Umum |
|-------|-----------|-------------|
| **Internal** | Apakah hubungan IV→DV nyata? | Confounding variable, selection bias |
| **External** | Apakah bisa digeneralisasi? | Dataset terlalu spesifik |
| **Construct** | Apakah mengukur konsep yang benar? | Metrik tidak sesuai |
| **Conclusion** | Apakah kesimpulan statistik valid? | Sample size kecil, uji salah |

Internal dan external validity sering berkonflik: semakin terkontrol (internal kuat) → semakin artificial (external lemah).

### Tiga Tipe Eksperimen dalam Riset TI

| Tipe | Deskripsi | Kapan Digunakan |
|------|----------|----------------|
| **Comparison Study** | Metode A vs B pada kondisi identik | Membandingkan pendekatan berbeda |
| **Ablation Study** | Full system → lepas komponen satu per satu | Mengukur kontribusi tiap komponen |
| **Parameter Study** | Variasikan satu parameter, amati dampak | Uji sensitifitas/robustness |

### Fairness dalam Perbandingan

Perbandingan yang adil = **kondisi identik** untuk semua metode: dataset sama, preprocessing sama, tuning effort sebanding, environment sama, metrik sama.

Contoh tidak adil: Transformer (30 fitur tambahan + Bayesian optimization) vs RF (default params) → hasilnya misleading.

### Threats to Validity = Diidentifikasi Sebelum Eksperimen

Ancaman validitas harus diidentifikasi **sebelum** eksperimen dan mitigasinya dirancang sebagai bagian dari desain — bukan ditulis sebagai boilerplate setelah selesai.

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan testing | Memastikan sistem memenuhi requirement | Membuktikan hubungan kausal antar variabel |
| Baseline | Versi sebelumnya (last release) | Metode tervalidasi dari literatur |
| Kegagalan | Bug → fix → release | H₀ tidak ditolak → tetap kontribusi ilmiah |
| Sukses | 100% test pass | Evidence valid — mendukung atau menolak hipotesis |

### Istilah Penting

- **Causality** — Hubungan sebab-akibat (covariance + temporal + elimination)
- **Controlled Experiment** — Ubah satu variabel, kontrol sisanya, amati efek
- **Fairness** — Semua metode diuji pada kondisi yang benar-benar identik
- **Threats to Validity** — Faktor yang bisa melemahkan kesimpulan jika tidak dimitigasi
- **Conclusion Validity** — Validitas statistik: power, sample size, uji yang tepat

---

## Template A.7 — Desain Eksperimen Lengkap

EXPERIMENT DESIGN

Research Question : Seberapa besar peningkatan nilai Defect Detection Rate (DDR) dari pengujian Boundary Value Analysis (BVA) dan Equivalence Partitioning (EP) dibandingkan dengan pengujian kasual (ad-hoc) pada Controller MVC Sistem Manajemen Gudang Hasil Tani?
Hypothesis        : Metode kasus batas (BVA dan EP) akan menghasilkan persentase nilai DDR yang secara signifikan lebih tinggi dibandingkan pengujian kasual karena menyasar titik buta matematis validasi pada lapisan Controller.
Tipe Eksperimen   : [x] Comparison  [ ] Ablation  [ ] Parameter

Kondisi Eksperimen:
| Kondisi | Deskripsi | IV Value | CV Settings |
|---------|-----------|----------|-------------|
| Control | Pengujian kasual oleh pengembang tanpa dokumen panduan matriks. | Ad-Hoc Testing | Sistem Hasil Tani, Code Freeze, Reset DB (TRUNCATE) |
| Treatment | Pengujian terstruktur menggunakan dokumen matriks nilai batas. | Matriks BVA & EP | Sistem Hasil Tani, Code Freeze, Reset DB (TRUNCATE) |

Fairness Checklist:
  [x] Dataset identik untuk semua kondisi
  [x] Preprocessing setara
  [x] Tuning effort setara
  [x] Environment identik
  [x] Metrik evaluasi sama

Threat Analysis:
| Threat Type | Ancaman Spesifik | Mitigasi |
|-------------|-----------------|----------|
| Internal    | Carry-over effect (penguji ad-hoc belajar dari pengujian BVA/EP jika urutannya terbalik). | Skenario pengujian dikunci: Ad-Hoc wajib dilakukan lebih dulu sebelum dokumen matriks BVA/EP diekspos ke penguji. |
| External    | Hasil pengujian hanya berlaku untuk modul FEFO dan Inbound aplikasi Hasil Tani. | Mendokumentasikan kompleksitas algoritma bisnis agar dapat direplikasi untuk sistem pergudangan lain. |
| Construct   | Menghitung bug visual/UI (seperti tombol salah warna) sebagai keberhasilan pengujian fungsional. | Menetapkan batasan tegas: bug hanya dihitung valid jika lolos mengubah basis data secara keliru atau membuat Controller *crash*. |
| Conclusion  | Jumlah skenario uji (N) terlalu sedikit sehingga perbedaan persentase DDR tidak bermakna. | Merancang variasi data dummy yang cukup luas dan representatif berdasarkan pedoman BVA dan EP baku. |

Statistical Plan:
  Uji statistik   : Two-Proportion Z-Test
  Justifikasi      : Cocok untuk membandingkan dua rasio/persentase proporsi (DDR kelompok intervensi vs DDR kelompok kontrol) yang saling independen.
  Alpha            : 0.05
  Effect size min  : 15% peningkatan nilai DDR.

---

## Latihan 1 — Desain Eksperimen

Susun desain eksperimen berdasarkan RQ, variabel, dan sistem dari WS-04 sampai WS-06.

**RQ:** Seberapa besar peningkatan nilai Defect Detection Rate (DDR) dari pengujian Boundary Value Analysis (BVA) dan Equivalence Partitioning (EP) dibandingkan dengan pengujian kasual (ad-hoc) pada Controller MVC Sistem Manajemen Gudang Hasil Tani?
**Tipe eksperimen:** [x] Comparison / [ ] Ablation / [ ] Parameter

| Kondisi | Deskripsi | IV Value | CV Settings |
|---------|-----------|----------|-------------|
| Control | Pengujian fungsional *baseline* secara kasual oleh pengembang. | Eksekusi Ad-Hoc | Modul Transaksi Inbound/Outbound Hasil Tani, Reset DB per siklus. |
| Treatment | Pengujian fungsional dengan dokumen panduan matriks terstruktur. | Eksekusi Matriks BVA/EP | Modul Transaksi Inbound/Outbound Hasil Tani, Reset DB per siklus. |

---

## Latihan 2 — Fairness Checklist

Evaluasi apakah desain eksperimen di Latihan 1 sudah fair.

| Kriteria | Status | Detail |
|----------|--------|--------|
| Dataset identik | ✅ | Data dummy input (berat barang, nama komoditas, jumlah pengeluaran) diambil dari rentang domain yang sama. |
| Preprocessing setara | ✅ | Sistem di-reset (TRUNCATE database MySQL) ke kondisi kosong/awal setiap sebelum sesi pengujian dimulai. |
| Tuning effort setara | ✅ | Batas waktu sesi pengujian (misal 2 jam per sesi) disetarakan untuk metode Ad-Hoc dan BVA/EP. |
| Environment identik | ✅ | Diuji pada server lokal (XAMPP), versi PHP Native yang sama, dan peramban web yang persis sama. |
| Metrik evaluasi sama | ✅ | Sama-sama menggunakan perhitungan persentase metrik DDR. |

**Ada yang tidak fair?** [ ] Ya / [x] Tidak
> Jika ya, bagaimana cara memperbaikinya? Desain sudah dipastikan 100% adil (fair) karena variabel kontrol (CV) mengunci kondisi lingkungan dan waktu uji dengan sangat ketat.

---

## Latihan 3 — Threat Analysis

Identifikasi ancaman validitas untuk desain eksperimen ini.

| Threat Type | Ancaman Spesifik | Mitigasi |
|-------------|-----------------|----------|
| Internal | *Testing bias* atau *Learning effect* jika dilakukan oleh satu tester yang sama bolak-balik. | Kunci urutan eksekusi: Baseline (Ad-hoc) harus dijalankan sebelum desain matriks uji (BVA/EP) dibuat/diserahkan ke tester. |
| External | Batasan arsitektur (hanya teruji pada PHP Native murni). | Definisikan dengan jelas di batasan masalah bahwa intervensi difokuskan pada lapisan logika Controller berbasis prosedur *native*. |
| Construct | Kesalahan definisi "cacat fungsional" saat menghitung skor. | Menggunakan parameter error logger PHP (`E_ALL`) sebagai penentu objektif suatu *crash* pada *backend*. |
| Conclusion | Pembulatan nilai metrik DDR yang manipulatif. | Melaporkan skor metrik secara telanjang beserta nilai mentah/rasio aslinya (jumlah error/total kasus). |

**Ancaman mana yang paling sulit dimitigasi?** Internal Validity (*Testing bias* / Bias Penguji).
**Mengapa?**
> Karena dalam skenario pengujian fungsional, seorang penguji memiliki ingatan tentang aplikasi. Jika orang yang menguji *Ad-Hoc* adalah orang yang juga menulis matriks BVA/EP, secara tidak sadar dia sudah mengetahui letak kelemahan sistem dan akan mengincar titik tersebut di sesi *Ad-Hoc*, sehingga menaikkan skor *baseline* secara tidak natural dan merusak keadilan eksperimen.

---

## Refleksi

> Sebuah paper melaporkan "metode kami mengalahkan semua baseline." Apa 3 pertanyaan pertama yang harus diajukan untuk mengevaluasi klaim ini?

**Jawaban:**
1. Apakah *baseline* yang digunakan benar-benar merepresentasikan metode standar industri yang dioptimalkan, atau sengaja disetel lemah (*strawman baseline*) agar metode baru terlihat bagus?
2. Apakah perbandingan dilakukan di dalam *environment*, pengaturan parameter awal, *tuning effort*, dan *dataset* yang benar-benar 100% identik (*fairness*)?
3. Metrik apa yang digunakan sebagai tolak ukur "mengalahkan", dan apakah metrik tersebut secara objektif mengukur tujuan utama (validitas konstruk), atau hanya hasil *cherry-picking* dari peneliti?