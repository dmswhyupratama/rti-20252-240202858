# Riset Teknologi Informasi

> Semester Genap 2025/2026 — Teknik Informatika, Universitas Putra Bangsa

---

## Langkah Pengerjaan

### 1. Fork & Rename

1. Klik **Fork** di kanan atas halaman ini
2. Pada halaman fork, ubah **Repository name** menjadi:
   ```
   rti-20252-<NIM>
   ```
   Contoh: `rti-20252-2021001`
3. Klik **Create fork**

### 2. Clone

```bash
git clone https://github.com/<USERNAME>/rti-20252-<NIM>.git
cd rti-20252-<NIM>
```

### 3. Kerjakan Worksheet

Buka file di folder `worksheets/` sesuai minggu yang sedang berjalan. Isi semua bagian yang diminta.

### 4. Commit & Push

```bash
git add worksheets/ws-XX-*.md
git commit -m "ws-XX selesai"
git push
```

> Commit boleh dilakukan berkali-kali. Yang dinilai adalah kondisi terakhir sebelum deadline.

---

## Jadwal & Worksheet

### Bagian I — Fondasi Berpikir Ilmiah

| Week | Topik | Worksheet | Deadline |
|------|-------|-----------|----------|
| 1 | Distorsi & Paradigma Riset | [`ws-01`](worksheets/ws-01-distorsi-paradigma.md) | Week 3 |
| 2 | Problem Statement | [`ws-02`](worksheets/ws-02-problem-statement.md) | Week 4 |
| 3 | Literature Mapping & Gap | [`ws-03`](worksheets/ws-03-literature-gap.md) | Week 5 |
| 4 | Research Question & Hypothesis | [`ws-04`](worksheets/ws-04-rq-hypothesis.md) | Week 6 |

### Bagian II — Merancang Riset

| Week | Topik | Worksheet | Deadline |
|------|-------|-----------|----------|
| 5 | Variabel & Metrik | [`ws-05`](worksheets/ws-05-variabel-metrik.md) | Week 7 |
| 6 | System-Experiment Mapping | [`ws-06`](worksheets/ws-06-system-experiment.md) | Week 8 |
| 7 | Experimental Design & Validity | [`ws-07`](worksheets/ws-07-experiment-design.md) | Week 9 |

### UTS — Week 8

| Week | Topik | Worksheet | Deadline |
|------|-------|-----------|----------|
| 8 | Proposal Integration | [`ws-08`](worksheets/ws-08-proposal-integration.md) | Week 10 |

### Bagian III — Eksekusi dan Analisis

| Week | Topik | Worksheet | Deadline |
|------|-------|-----------|----------|
| 9 | Implementation & Reproducibility | [`ws-09`](worksheets/ws-09-implementation.md) | Week 11 |
| 10 | Execution & Data Collection | [`ws-10`](worksheets/ws-10-execution-data.md) | Week 12 |
| 11 | Data Validation | [`ws-11`](worksheets/ws-11-data-validation.md) | Week 13 |
| 12 | Result Presentation | [`ws-12`](worksheets/ws-12-result-presentation.md) | Week 14 |

### Bagian IV — Analisis dan Komunikasi

| Week | Topik | Worksheet | Deadline |
|------|-------|-----------|----------|
| 13 | Preprocessing | [`ws-13`](worksheets/ws-13-preprocessing.md) | Week 15 |
| 14 | Analysis & Interpretation | [`ws-14`](worksheets/ws-14-analysis-interpretation.md) | Week 16 |
| 15 | Scientific Writing | [`ws-15`](worksheets/ws-15-scientific-writing.md) | Week 16 |

### UAS — Week 16

| Week | Topik | Worksheet | Deadline |
|------|-------|-----------|----------|
| 16 | Presentation & Defense | [`ws-16`](worksheets/ws-16-presentation-defense.md) | Week 16 |

---

## Ketentuan

- Setiap worksheet memiliki deadline **2 minggu** setelah materi diberikan
- Kerjakan di branch `main`
- Nama repo harus mengikuti format `rti-20252-<NIM>`
- Worksheet yang tidak di-push sebelum deadline dianggap belum dikumpulkan

---

## Struktur Repo

```
rti-20252-<NIM>/
├── README.md
├── LICENSE
└── worksheets/
    ├── ws-01-distorsi-paradigma.md
    ├── ws-02-problem-statement.md
    ├── ...
    └── ws-16-presentation-defense.md
```