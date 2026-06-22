# WS-08: Proposal Integration (UTS)

> **Bab 8 — Proposal & Checkpoint**

---

## Ringkasan Materi

### Proposal = Satu Argumen Utuh

Proposal riset bukan kumpulan bab yang independen. Ia adalah **satu argumen** yang mengalir dari masalah ke rencana solusi. Jika satu koneksi putus, seluruh proposal kehilangan koherensi.

### Integration Map — 6 Koneksi Kritis

```
Problem (Bab 2) → Gap (Bab 3) → RQ & H (Bab 4) → Metrik (Bab 5) → Sistem (Bab 6) → Eksperimen (Bab 7)
```

| Koneksi | Pertanyaan Verifikasi |
|---------|----------------------|
| Problem → Gap | Apakah gap muncul dari analisis literatur terhadap masalah? |
| Gap → RQ | Apakah RQ langsung menjawab gap yang teridentifikasi? |
| RQ → Metrik | Apakah setiap variabel di RQ punya metrik terdefinisi? |
| Metrik → Sistem | Apakah setiap metrik bisa diukur oleh komponen sistem? |
| Sistem → Eksperimen | Apakah desain eksperimen menggunakan sistem sebagai instrumen? |

### Koherensi Vertikal + Horizontal

- **Vertikal** — Alur logis atas-ke-bawah (problem → experiment)
- **Horizontal** — Konsistensi terminologi (nama variabel di RQ = di hipotesis = di metrik = di desain)

### Jebakan Kognitif

| Jebakan | Deskripsi |
|---------|----------|
| "Selling" Introduction | Menulis promosi, bukan menyajikan data dan gap |
| Copy-paste Methodology | Menyalin deskripsi tekstbook tanpa menyesuaikan ke RQ |
| Optimistic Timeline | Meremehkan waktu implementasi; selalu tambah buffer 30-50% |
| No Possibility of Failure | Mengimplikasikan hasil pasti sukses — proposal jujur mengakui H₀ mungkin tidak ditolak |

### Struktur Proposal

1. **Pendahuluan** — Latar belakang + problem statement (Bab 1-2)
2. **Tinjauan Pustaka** — Literature review + gap + baseline (Bab 3)
3. **RQ / Kontribusi / Hipotesis** — (Bab 4)
4. **Metodologi** — Metrik + sistem + desain eksperimen (Bab 5-7)
5. **Timeline & Output**

### Istilah Penting

- **Integration Map** — Diagram 6 koneksi kritis antar komponen proposal
- **Vertical Coherence** — Alur logis atas-ke-bawah
- **Horizontal Coherence** — Konsistensi terminologi di semua bagian
- **Checkpoint** — Titik self-assessment sebelum transisi dari desain ke eksekusi

---

## Template A.8 — Integration Checklist

PROPOSAL INTEGRATION CHECKLIST

Koneksi Vertikal (Flow Atas-Bawah):
  [x] Problem → Gap: masalah terdokumentasi di literatur
  [x] Gap → RQ: pertanyaan menjawab gap spesifik
  [x] RQ → Hypothesis: hipotesis memprediksi jawaban
  [x] Hypothesis → Metric: metrik mengukur variabel dalam hipotesis
  [x] Metric → System: komponen sistem menghasilkan/mengukur metrik
  [x] System → Experiment: desain eksperimen menggunakan sistem

Koneksi Horizontal (Konsistensi):
  [x] Istilah sama di semua bagian
  [x] Variabel di RQ = variabel di hipotesis = metrik di desain
  [x] Scope tidak berubah dari masalah ke eksperimen

Rubrik Self-Assessment:
| Kriteria | 1 (Lemah) | 2 (Cukup) | 3 (Baik) | Skor |
|----------|-----------|-----------|----------|------|
| Koherensi |          |           |     ✓    |  3   |
| Specificity |        |           |     ✓    |  3   |
| Feasibility |        |           |     ✓    |  3   |
| Rigor     |          |           |     ✓    |  3   |

---

## Latihan 1 — Kompilasi Proposal Mini

Kumpulkan hasil dari WS-02 sampai WS-07 menjadi satu ringkasan proposal.

| Komponen | Sumber | Isi (1-2 kalimat) |
|----------|--------|-------------------|
| Problem Statement | WS-02 | Tingginya tempo kerja memicu *human-error* (salah ketik) oleh petugas gudang, yang berakibat rusaknya algoritma FEFO dan laporan penyusutan karena lemahnya validasi pada lapisan *Controller*. |
| Gap | WS-03 | Studi pengujian *Black-Box* (BVA/EP) terdahulu hanya berfokus mendemonstrasikan pengujian form antarmuka sederhana (*View*), belum mengevaluasi ketahanan fungsional lapisan logika bisnis (*Controller*) MVC yang mengelola logistik. |
| RQ | WS-04 | Seberapa besar peningkatan nilai *Defect Detection Rate* (DDR) dari pengujian *Boundary Value Analysis* (BVA) dan *Equivalence Partitioning* (EP) dibandingkan pengujian kasual (*ad-hoc*) pada *Controller* Sistem Manajemen Gudang Hasil Tani? |
| Hipotesis | WS-04 | H₁: Penerapan BVA dan EP akan menghasilkan persentase nilai DDR yang secara signifikan lebih tinggi dibandingkan pengujian kasual karena terstruktur menyasar titik buta matematis *Controller*. |
| Variabel & Metrik | WS-05 | IV = Pendekatan Uji (*Ad-hoc* vs BVA/EP); DV = Efektivitas Uji; Metrik = *Defect Detection Rate* (Rasio jumlah bug valid / total kasus uji, bersatuan %). |
| Sistem | WS-06 | Purwarupa aplikasi web Sistem Manajemen Gudang Hasil Tani (PHP Native MVC), difokuskan murni pada *script Controller* modul *Inbound* dan *Outbound* (FEFO). |
| Desain Eksperimen | WS-07 | Studi komparatif terkontrol: *Baseline* (Uji *Ad-hoc*) dijalankan lalu diukur, dilanjutkan *reset database* (TRUNCATE), kemudian intervensi (Uji Matriks BVA/EP) dijalankan lalu diukur dan dibandingkan. |

---

## Latihan 2 — Integration Checklist

Verifikasi 6 koneksi kritis. Isi dengan merujuk tabel di Latihan 1.

| Koneksi | Status | Bukti |
|---------|--------|-------|
| Problem → Gap | ✅ | Masalah ada di lapisan logika (*Controller*), dan gap membuktikan literatur selama ini hanya fokus menguji tampilan luar (*View*). |
| Gap → RQ | ✅ | RQ secara langsung menanyakan evaluasi efektivitas BVA/EP spesifik pada lapisan *Controller* MVC. |
| RQ → Hypothesis | ✅ | H₁ memprediksi adanya peningkatan metrik (DDR) pada kelompok intervensi seperti yang ditanyakan oleh RQ. |
| Hypothesis → Metric | ✅ | Klaim hipotesis tentang "penemuan celah yang lebih tinggi" diukur persis dengan rumus matematis persentase *Defect Detection Rate* (DDR). |
| Metric → System | ✅ | Sistem PHP Native merespons manipulasi input dengan memunculkan *error* atau membiarkan data lewat, yang langsung dihitung status Lulus/Gagal-nya untuk rumus DDR. |
| System → Experiment | ✅ | Eksperimen dilakukan dengan menjadikan sistem MVC sebagai instrumen laboratorium (di-reset dan dibekukan kodenya antar sesi pengujian). |

**Koneksi mana yang paling lemah?** Metric → System
**Bagaimana cara memperkuatnya?**
> Karena *bug* pada PHP Native terkadang hanya berupa layar putih kosong (blank screen) tanpa pesan, pengukuran metrik akan diperkuat dengan menghidupkan konfigurasi `error_reporting(E_ALL)` pada *server* lokal untuk memastikan setiap celah logika tercatat dengan jelas dan valid sebagai bagian perhitungan DDR.

**Konsistensi horizontal — apakah istilah dan scope konsisten?** [x] Ya / [ ] Tidak
> Jika tidak, di bagian mana terjadi inkonsistensi? (Konsisten, sudah menggunakan nama Sistem Manajemen Gudang Hasil Tani dan fokus pada *Controller* MVC di seluruh bagian).

---

## Latihan 3 — Rubrik Self-Assessment

Evaluasi proposal mini menggunakan rubrik.

| Kriteria | Skor (1-3) | Justifikasi |
|----------|-----------|-------------|
| Koherensi | 3 | Alur logis mengalir lurus dari bahaya *human-error* di gudang, turun ke lemahnya *Controller*, dan diukur efektivitas intervensinya menggunakan DDR. Tidak ada lompatan topik. |
| Specificity | 3 | Variabel dan instrumen sangat spesifik: IV menggunakan Matriks Kasus Uji batas, DV memakai rasio pasti (DDR), dan CV membekukan *database* dan kode. |
| Feasibility | 3 | Eksperimen sangat mungkin dilakukan dengan cepat dan murah karena menggunakan sistem *local/prototype* (PHP Native) dan data simulasi uji (*dummy data*). |
| Rigor | 3 | Bias eksperimen dijaga ketat: *database* di-reset (*truncate*) antar sesi dan urutan pengujian (*ad-hoc* selalu pertama) dikunci agar penguji tidak terpengaruh efek belajar (*learning effect*). |

**Skor total:** 12 / 12

**Apakah proposal siap untuk fase eksekusi?** [x] Ya / [ ] Belum
> Jika belum, apa yang perlu diperbaiki? Proposal Word dan Matriks Excel sudah selaras dan siap diajukan untuk UTS.

---

## Refleksi

> Dari seluruh proses WS-01 sampai WS-08, bagian mana yang paling mudah dan paling sulit? Mengapa? Apa yang akan dilakukan berbeda jika mengulang dari awal?

**Bagian termudah:** Mengidentifikasi arsitektur sistem dan menyiapkan skenario basis data (WS-06). Karena sudah terbiasa dengan struktur MVC, PHP Native, dan manajemen operasional tabel MySQL, membayangkan *setup environment* untuk *testing* adalah hal yang sangat natural.
**Bagian tersulit:** Menjaga rantai logika operasional (WS-05) agar tidak terjadi lompatan logika antara *Problem*, *Gap*, dan *Objective*. Memilih kalimat yang membedakan evaluasi UI dengan evaluasi logika bisnis *Controller* membutuhkan kejelian tingkat tinggi agar tidak terjebak menjadi pengujian *Black-box* biasa.
**Yang akan dilakukan berbeda:**
> Jika mengulang dari awal, proses penulisan tidak akan dimulai langsung dari pembuatan paragraf narasi di *Microsoft Word*. Semua akan dimulai dengan mengisi secara berurutan dan disiplin matriks di file Excel (*Framework Reviewer-Guided*) untuk mengunci logika variabelnya terlebih dahulu, barulah hasil justifikasinya dirakit menjadi kalimat proposal final.