# WS-04: Research Question & Hypothesis

> **Bab 4 — Research Question, Contribution & Hypothesis**

---

## Ringkasan Materi

### RQ Bukan Pertanyaan Biasa

Research Question yang baik secara implisit mengandung cetak biru eksperimen: subjek, baseline, metrik, domain, dataset.

| Kualitas | Contoh |
|----------|--------|
| **Buruk** | "Bagaimana pengaruh deep learning terhadap deteksi malware?" |
| **Baik** | "Apakah CNN menghasilkan F1-Score lebih tinggi dari RF pada CIC-MalMem-2022?" |

Perbedaan: RQ yang baik menyebutkan **metode spesifik**, **metrik terukur**, **baseline**, dan **dataset**.

### Tiga Jenis RQ

| Jenis | Pola | Kebutuhan |
|-------|------|-----------|
| **Comparison** | A vs B → mana lebih baik? | ≥ 2 metode, metrik sama |
| **Improvement** | A' vs A → modifikasi lebih baik? | Pre/post, bukti perbaikan |
| **Exploratory** | Faktor X₁...Xₙ → pengaruh terhadap Y? | Multi-variabel, korelasi/regresi |

### Contribution Statement

Tiga jenis kontribusi: **Improvement** (metode terbukti lebih baik), **Comparison** (perbandingan sistematis yang belum ada), **Novel Approach** (pendekatan baru). Kontribusi harus terhubung langsung dengan gap — kontribusi tanpa gap = klaim tanpa justifikasi.

### Hypothesis H₀ / H₁

- **H₀** (Null) = Tidak ada perbedaan signifikan — asumsi default, harus dibuktikan salah
- **H₁** (Alternative) = Ada perbedaan signifikan — diterima hanya jika H₀ ditolak
- Harus **falsifiable**, mengandung **metrik terukur**, dirumuskan **SEBELUM eksperimen**

### Rantai Operasionalisasi

```
RQ → Variable → Metric → Data → Analysis
```

Jika rantai ini tidak lengkap, RQ belum mature. Bi-directional: RQ yang tidak bisa jadi hipotesis testable harus direvisi mundur.

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan pertanyaan | Apa yang harus dibangun? | Apa yang harus dibuktikan? |
| Bentuk jawaban | Sistem yang berfungsi | Bukti empiris terukur |
| Sukses diukur oleh | User satisfaction, uptime | Signifikansi statistik, effect size |
| Jika gagal | Debug dan perbaiki | Laporkan, analisis mengapa |

### Istilah Penting

- **Research Question (RQ)** — Pertanyaan spesifik: variabel terukur + metrik + konteks
- **Contribution Statement** — Apa yang diketahui setelah riset selesai yang sebelumnya belum ada
- **H₀ / H₁** — Null vs Alternative Hypothesis
- **Falsifiability** — Kondisi hipotesis ditolak harus bisa didefinisikan sebelum eksperimen
- **Operationalization** — Proses mewujudkan konsep abstrak menjadi variabel terukur

---

## Template A.4 — RQ-Contribution-Hypothesis

```
RQ-CONTRIBUTION-HYPOTHESIS

Gap Statement  : ____________________

Research Question:
  Tipe         : [ ] Comparison  [ ] Improvement  [ ] Exploratory
  Formulasi    : ____________________
  Variabel IV  : ____________________
  Variabel DV  : ____________________
  Metrik       : ____________________
  Dataset      : ____________________
  Baseline     : ____________________

Quality Check RQ:
  [ ] Variabel spesifik
  [ ] Metrik jelas
  [ ] Baseline ada
  [ ] Konteks disebutkan
  [ ] Memerlukan eksperimen (bukan hanya survei literatur)

Contribution Statement:
  Apa yang baru diketahui : ____________________
  Jenis kontribusi        : [ ] Improvement  [ ] Comparison  [ ] Novel approach
  Gap yang diisi          : ____________________

Hypothesis Pair:
  H₀ : ____________________
  H₁ : ____________________
  Threshold              : ____________________
  Justifikasi threshold  : ____________________
```


## Latihan 1 — Dari Gap ke RQ

**Gap dari WS-03:** 
Banyak penelitian pengujian sistem gudang secara manual terbatas pada modul tertentu dan skenario standar, mengabaikan pengujian kasus batas ekstrim yang justru rawan human-error.

**RQ versi pertama (tulis bebas):**
> Apakah pengujian menggunakan metode BVA dan EP bisa menemukan lebih banyak bug di form stok barang dibanding cuma ngetes biasa?

**Evaluasi RQ:**

| Komponen | Ada? | Isi |
|----------|------|-----|
| Metode spesifik | Ya | Boundary Value Analysis (BVA) dan Equivalence Partitioning (EP) |
| Metrik terukur | Tidak | Belum ada metrik angka yang jelas (hanya kata "lebih banyak") |
| Baseline | Ya | Pengujian biasa (Ad-hoc) |
| Dataset/konteks | Ya | Form stok barang |

**Tipe RQ:** [x] Comparison / [ ] Improvement / [ ] Exploratory

**RQ versi revisi (setelah evaluasi):**
> Apakah pengujian Black Box menggunakan kombinasi metode BVA dan EP menghasilkan Defect Detection Rate yang lebih tinggi dibandingkan pengujian fungsional ad-hoc (baseline) pada modul Inbound/Outbound aplikasi web Agri-Pos?

---

## Latihan 2 — Hypothesis Pair

Rumuskan pasangan hipotesis dari RQ di Latihan 1.

| Komponen | Isi |
|----------|-----|
| H₀ | Tidak ada perbedaan Defect Detection Rate yang signifikan antara pengujian BVA & EP dengan pengujian ad-hoc pada modul Inbound/Outbound. |
| H₁ | Pengujian BVA & EP menghasilkan Defect Detection Rate yang secara signifikan lebih tinggi dibandingkan pengujian ad-hoc pada modul Inbound/Outbound. |
| Metrik | Defect Detection Rate (DDR) = (Jumlah cacat valid ditemukan / Total test case dieksekusi) x 100% |
| Threshold | Peningkatan DDR minimal 15% |
| Justifikasi threshold | Mengkompensasi waktu ekstra yang dibutuhkan untuk merancang Test Case Matrix secara formal. |

**Apakah hipotesis ini falsifiable?** [x] Ya / [ ] Tidak
> Bagaimana cara membuktikannya salah? Dengan membandingkan matriks hasil akhir. Jika persentase penemuan bug dari metode BVA & EP ternyata sama saja atau peningkatannya di bawah 15% dibandingkan metode ad-hoc, maka H₀ diterima dan H₁ ditolak.

---

## Latihan 3 — Rantai Operasionalisasi

Lengkapi rantai dari RQ hingga metode analisis.

| Tahap | Isi |
|-------|-----|
| RQ | Apakah pengujian Black Box menggunakan kombinasi metode BVA dan EP menghasilkan Defect Detection Rate yang lebih tinggi dibandingkan pengujian fungsional ad-hoc pada modul Inbound/Outbound aplikasi web Agri-Pos? |
| Variable (IV) | Pendekatan metode pengujian (BVA & EP vs Ad-hoc) |
| Variable (DV) | Tingkat deteksi cacat perangkat lunak (Defect Detection Rate) |
| Metric | Persentase status 'Fail' pada dokumen Test Case Matrix |
| Data source | Hasil rekapan log eksekusi Skenario Pengujian Fungsional secara manual pada aplikasi web |
| Analysis method | Analisis deskriptif komparatif (membandingkan persentase rasio DDR antar kedua metode) |

**Apakah rantai lengkap?** [x] Ya / [ ] Tidak
> Jika tidak, tahap mana yang perlu direvisi? (Rantai sudah lengkap dari hulu ke hilir)

---

## Refleksi

> Ambil satu judul skripsi/paper yang pernah dibaca. Coba ekstrak RQ-nya. Apakah RQ tersebut memenuhi semua komponen (metode, metrik, baseline, konteks)? Jika tidak, apa yang hilang?

**Judul:** Pengujian Black Box pada Aplikasi Sistem Informasi Akademik Menggunakan Teknik Equivalence Partitioning (Muslimin et al., 2020)
**RQ yang diekstrak:** Bagaimana hasil penerapan teknik Equivalence Partitioning dalam pengujian fungsional Sistem Informasi Akademik?
**Komponen yang hilang:** RQ tersebut tidak memiliki metrik terukur yang jelas (hanya menanyakan "bagaimana hasil"), tidak memiliki baseline pembanding (hanya menguji satu metode tanpa membandingkannya dengan metode lain), dan tidak memerlukan eksperimen (hanya berupa laporan penerapan langkah-langkah prosedural).