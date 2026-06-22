# WS-14: Analysis, Interpretation & Failure Analysis

> **Bab 14 — Analisis Data, Interpretasi & Failure Analysis**

---

## Ringkasan Materi

### Data → Knowledge Model

```
Data → Analysis → Interpretation → Explanation → Knowledge
```

Tiga level yang berbeda:
- **Analysis** — "Apa yang terjadi?" (deskriptif + inferensial)
- **Interpretation** — "Apa artinya?" (konteks RQ + literatur)
- **Failure Analysis** — "Mengapa tidak berhasil?" (boundary conditions)

### Beyond p-value

**Statistical significance ≠ practical significance.** Selalu laporkan:
1. p-value (signifikansi statistik)
2. Effect size (besarnya efek)
3. Confidence interval (rentang ketidakpastian)

| Effect Size (Cohen's d) | Interpretasi |
|-------------------------|-------------|
| < 0.2 | Small |
| 0.2 – 0.8 | Medium |
| > 0.8 | Large |

### Pemilihan Uji Statistik

| Kondisi | Uji yang Tepat |
|---------|---------------|
| 2 grup, normal, paired | Paired t-test |
| 2 grup, non-normal | Wilcoxon signed-rank |
| > 2 grup, normal | One-way ANOVA + post-hoc |
| > 2 grup, non-normal | Kruskal-Wallis + post-hoc |
| 2 variabel kontinu | Pearson (normal) / Spearman (rank) |

### Failure Analysis as Contribution

Hipotesis yang ditolak adalah **temuan yang berharga**:

| Dataset | New (F1) | Baseline (F1) | p-value | Cohen's d |
|---------|---------|--------------|---------|-----------|
| DS-1 (small, clean) | 94.2±1.1 | 89.3±1.5 | <0.001 | **3.7** |
| DS-4 (medium, noisy) | 78.3±3.2 | 82.1±2.8 | 0.008 | **-1.3** |
| DS-5 (large, noisy) | 71.6±4.1 | 80.5±3.0 | <0.001 | **-2.5** |

**Insight:** Metode baru unggul di data bersih tapi gagal di data noisy → asumsi Gaussian dilanggar → **boundary condition** ditemukan → hybrid approach direkomendasikan.

**Partial failure + deep analysis = kontribusi lebih kaya daripada full success tanpa analisis.**

### Limitation Types

| Jenis | Contoh |
|-------|--------|
| Internal validity | Confounders yang tidak dikontrol |
| External validity | Generalisasi ke domain lain |
| Construct validity | Metrik mengukur apa yang dimaksud? |
| Statistical limitation | Sample size, asumsi distribusi |

### Jebakan Kognitif

1. "Signifikan statistik = penting secara praktis" → cek effect size
2. "Hipotesis tidak didukung → cari sudut baru" → p-hacking
3. "Kegagalan tidak perlu dilaporkan detail" → missed insight
4. "Limitasi cukup disebutkan, tidak perlu dianalisis" → kedalaman hilang

---

## Template A.14 — Analysis & Interpretation Report

```
ANALYSIS & INTERPRETATION

1. Statistik Deskriptif:
   | Skenario | Mean | Std | Median | Min | Max | n |
   |----------|------|-----|--------|-----|-----|---|
   |          |      |     |        |     |     |   |

2. Uji Hipotesis:
   Uji yang digunakan  : ____________________
   Justifikasi          : ____________________
   Hasil: p = ____, effect size (d/r/η²) = ____
   CI 95%               : [____, ____]

3. Keputusan:
   [ ] H₀ ditolak → H₁ diterima
   [ ] H₀ tidak ditolak

4. Interpretasi:
   Hubungan ke RQ       : ____________________
   Practical significance: ____________________
   Perbandingan literatur: ____________________

5. Limitation:
   | Jenis | Ancaman | Dampak | Mitigasi |
   |-------|---------|--------|----------|
   |       |         |        |          |

6. Failure Analysis (jika H₀ tidak ditolak):
   Penyebab potensial  : ____________________
   Boundary condition   : ____________________
   Insight              : ____________________
```

---

## Latihan 1 — Pemilihan Uji Statistik

Tentukan uji statistik yang tepat untuk eksperimen Anda.

| Pertanyaan | Jawaban |
|-----------|---------|
| Berapa grup yang dibandingkan? | *Contoh: 3 (BERT, LSTM, SVM)* |
| Apakah data berpasangan (paired)? | |
| Apakah distribusi normal? (uji normalitas) | |
| **Uji yang dipilih:** | |
| **Justifikasi:** | |

**Effect size yang akan dilaporkan:** [ ] Cohen's d / [ ] Eta-squared / [ ] Lainnya: ____

---

## Latihan 2 — Interpretasi Hasil

Gunakan data berikut (atau data riil Anda) untuk berlatih interpretasi.

**Data:**
| Model | Accuracy (mean ± std) | n |
|-------|----------------------|---|
| A | 89.2 ± 1.5 | 10 |
| B | 87.8 ± 2.1 | 10 |

p = 0.045, Cohen's d = 0.74, CI 95% = [0.03, 2.77]

| Aspek | Interpretasi |
|-------|-------------|
| Signifikansi statistik | *Contoh: p < 0.05 → signifikan pada α=0.05* |
| Effect size | *Contoh: d=0.74 → medium-to-large effect* |
| Practical significance | |
| Hubungan ke RQ | |
| Perbandingan literatur | |

---

## Latihan 3 — Failure Analysis

Latih kemampuan failure analysis: hipotesis TIDAK didukung. Apa yang bisa dipelajari?

**Skenario:** Metode baru Anda mendapat F1 = 83.2%, baseline = 84.7%. p = 0.12 (tidak signifikan).

| Pertanyaan | Jawaban |
|-----------|---------|
| Apakah ini "gagal"? | *Contoh: Bukan gagal total — hipotesis tidak terdukung adalah temuan yang valid dan bisa menjadi kontribusi.* |
| Kemungkinan penyebab? | *Contoh: Metode baru menambah kompleksitas komputasi (+40% waktu) tanpa peningkatan F1 yang cukup — overhead tidak sebanding.* |
| Boundary condition? | *Contoh: Metode ini hanya efektif ketika data ≥ 10.000 record; di dataset kecil (<1.000), baseline lebih stabil.* |
| Insight yang bisa diambil? | *Contoh: Ada trade-off ukuran data vs kompleksitas — rekomendasikan hybrid approach yang adaptif berdasarkan ukuran dataset.* |
| Apakah layak dilaporkan? Mengapa? | *Contoh: Ya — negative result + boundary condition analysis adalah kontribusi riset yang diakui komunitas (ex: ACL, SIGIR). Mencegah riset duplikasi yang berulang.* |

**Limitation terkait:**
| Jenis | Ancaman | Dampak |
|-------|---------|--------|
| *Contoh: Statistical* | *Contoh: Hanya 5 run per skenario* | *Power test rendah* |
| | | |
| | | |

---

## Refleksi

> Apakah "failure" dalam riset benar-benar gagal, atau justru kontribusi? Bagaimana failure analysis mengubah cara Anda melihat hasil negatif?

> ___________________________________________________
> ___________________________________________________
