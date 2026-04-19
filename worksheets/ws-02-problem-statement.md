# WS-02: Problem Statement

> **Bab 2 — Problem Formulation & System Context**

---

## Ringkasan Materi

### Problem Formation Model

Masalah riset melewati 5 tahap transformasi. Melompat langsung dari Reality ke Variable adalah kesalahan paling umum.

```
Reality → Observed Issue (Symptom) → Diagnosed Problem (Root Cause)
→ Researchable Problem (Scoped) → Measurable Variable (Operationalized)
```

### Topic ≠ Problem ≠ Research Problem

| Level | Contoh | Status |
|-------|--------|--------|
| **Topik** | Keamanan IoT | Terlalu luas, tidak bisa diuji |
| **Problem** | MQTT tidak terenkripsi | Spesifik tapi belum riset |
| **Research Problem** | Belum ada studi membandingkan overhead TLS 1.3 vs DTLS pada MQTT di IoT RAM < 64KB | Bisa dirancang eksperimennya |

### Symptom vs Root Cause

Apa yang diamati (gejala) ≠ mengapa terjadi (akar masalah). Gunakan **5 Whys** atau **Fishbone Diagram** untuk menggali.

Contoh: "User meninggalkan checkout" (symptom) → "Waktu loading > 8 detik karena API call sequential" (root cause).

### System Thinking

Setiap masalah riset TI harus terikat pada komponen sistem: **Input → Process → Output → Outcome → Constraints → Stakeholders**.

### Problem Quality Check

Masalah riset yang layak harus memenuhi 5 kriteria:
- **Clarity** — Satu orang membaca akan paham
- **Measurability** — Ada metrik kuantitatif
- **Relevance** — Penting untuk domain
- **Testability** — Bisa gagal (falsifiable)
- **Impact** — Ada kontribusi jika terjawab

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan | Menyelesaikan masalah (*solve*) | Memahami dan membuktikan (*understand & prove*) |
| Masalah | Bug, error, fitur belum ada | Gap dalam pengetahuan |
| Scope | Selesaikan semua yang perlu | Batasi agar bisa dibuktikan |
| Output | Working system | Evidence, paper, replicable findings |

### Istilah Penting

- **Problem Statement** — Formulasi tertulis: konteks sistem + gap + dampak + justifikasi
- **System Context** — Deskripsi lengkap: input, proses, output, outcome, constraints, stakeholders
- **Problem Drift** — Masalah "bermutasi" dari pendahuluan ke metodologi karena statement awal tidak presisi
- **Solution-First Thinking** — Memulai dari solusi tanpa masalah yang jelas — berbahaya dalam riset
- **Operational Definition** — Definisi variabel yang cukup jelas agar peneliti lain bisa mengukur hal yang sama

---

## Template A.2 — Problem Statement Builder

```text
PROBLEM STATEMENT BUILDER

Domain & Konteks
  Domain   : Optimasi Rute (Sistem Pencarian Cerdas)
  Konteks  : Penentuan rute perjalanan obyek wisata di wilayah Bandung Raya pada waktu libur (weekend).

System Context
  Input       : Koordinat latitude/longitude dari 162 obyek wisata dan 500 titik persimpangan, serta bobot kemacetan statis (Normal=1.0, Padat=1.5, Macet=2.0).
  Process     : Eksekusi Algoritma Genetika (Pembangkitan populasi 8 kromosom, Seleksi Rank-Based, Persilangan Permutasi, Mutasi Inversi, Evaluasi Fitness).
  Output      : 5 rekomendasi rute obyek wisata terdekat dan tercepat.
  Outcome     : Wisatawan dapat menghemat waktu dan bahan bakar saat berwisata di Bandung Raya.
  Constraints : Sistem rentan terjebak pada optimum lokal sehingga rute yang dihasilkan terkadang masih dalam keadaan zig-zag. Algoritma dihentikan maksimal pada 1000 generasi atau jika konvergen selama 20 generasi.
  Stakeholders: Wisatawan (pengguna sistem) dan Pengembang Perangkat Lunak.

Fenomena → Problem
  Fenomena yang diamati             : Pariwisata Bandung tumbuh cepat dengan sangat banyak pilihan lokasi (162 titik).
  Gejala (symptom) yang terukur     : Wisatawan kebingungan dan membuang banyak waktu di jalan karena banyaknya kombinasi persimpangan (500 titik) saat hari libur.
  Masalah yang didiagnosis          : Belum adanya sistem terkomputerisasi yang mampu membandingkan dan mengevaluasi ribuan kombinasi rute wisata secara simultan untuk mencari jarak terpendek.
  Masalah riset (researchable)      : Bagaimana kinerja Algoritma Genetika dalam memecahkan masalah optimasi rute wisata (Travel Salesperson Problem modifikasi) berdasarkan parameter jarak, waktu, dan indeks kemacetan?
  Variabel yang terukur             : Jarak rute (km), waktu tempuh (menit), dan Nilai Kecocokan (Fitness).

Problem Quality Check
  [x] Clarity — Apakah satu orang membaca akan paham?
  [x] Measurability — Apakah ada metrik kuantitatif?
  [x] Relevance — Apakah penting untuk domain?
  [x] Testability — Apakah bisa gagal?
  [x] Impact — Apakah ada kontribusi jika terjawab?

Problem Statement (1 paragraf):
  Wisatawan di wilayah Bandung Raya sering menghadapi kesulitan dalam menentukan rute perjalanan yang efisien, terutama pada akhir pekan, mengingat terdapat 162 obyek wisata dan 500 titik persimpangan jalan yang harus dipertimbangkan. Kesulitan dalam mengevaluasi ribuan kombinasi persimpangan secara manual ini menyebabkan pemborosan waktu tempuh. Oleh karena itu, penelitian ini bertujuan untuk mengevaluasi kinerja Algoritma Genetika dalam merancang sistem rekomendasi yang mampu mencari rute terpendek dan tercepat berdasarkan variabel jarak tempuh, estimasi waktu, dan indeks kemacetan jalan, serta menganalisis kendala algoritma saat menghasilkan rute yang masih berbentuk zig-zag.
```

---

## Latihan 1 — Dari Topik ke Masalah Riset

**Topik awal:** Optimasi Pencarian Rute Obyek Wisata di Bandung Raya

| Tahap | Hasil |
|-------|-------|
| Reality | Pariwisata di Bandung sangat ramai saat *weekend* dengan banyak obyek wisata yang letaknya tersebar. |
| Observed Issue (Symptom) | Wisatawan sering mengambil rute yang tidak efisien dan terjebak macet karena kebingungan memilih dari ratusan persimpangan jalan. |
| Diagnosed Problem (Root Cause) | Mencari rute optimal secara manual dari 500 titik persimpangan adalah masalah kombinatorial yang sangat kompleks dan tidak bisa diselesaikan dengan tebakan biasa. |
| Researchable Problem | Sejauh mana tingkat keberhasilan Algoritma Genetika dalam menemukan solusi rute dengan nilai *fitness* tertinggi dari populasi rute yang dibangkitkan? |
| Measurable Variable | Nilai *Fitness* (diperoleh dari perhitungan fungsi matematika jarak, waktu, dan bobot kemacetan), jumlah generasi konvergen, dan durasi proses komputasi (menit). |

**Apakah terjebak solution-first thinking?** [ ] Ya / [x] Tidak
> Jika ya, kembali ke tahap mana? -
---

## Latihan 2 — System Context Decomposition

Gambarkan konteks sistem dari masalah riset di Latihan 1.

| Komponen | Deskripsi |
|----------|----------|
| Input | Data koordinat 162 obyek wisata, 500 titik persimpangan (dari Google Maps API), dan nilai bobot kemacetan (1.0, 1.5, 2.0). |
| Process | Proses evolusioner Algoritma Genetika: Representasi 30 gen kromosom, evaluasi kecocokan, *Rank Based Selection*, Persilangan Permutasi, dan *Inversion Mutation*. |
| Output | Perbandingan 5 rute obyek wisata terbaik berdasarkan nilai evaluasi tertinggi. |
| Outcome | Sistem perangkat lunak yang bisa dipakai wisatawan untuk mendapat panduan perjalanan yang menghemat waktu. |
| Constraints | Hasil rekomendasi belum sempurna dan rute masih bisa membentuk pola zig-zag karena batasan operator algoritma. |
| Stakeholders | Wisatawan domestik/mancanegara, masyarakat sekitar obyek wisata. |

---
**Komponen mana yang paling relevan dengan masalah riset?** **Process**. Karena di tahap proses inilah Algoritma Genetika bekerja, dan di tahap ini pula letak permasalahan utama mengapa hasil akhirnya bisa berbentuk rute zig-zag (operator persilangan atau mutasi yang kurang optimal).


## Latihan 3 — Problem Quality Check

Evaluasi problem statement yang sudah dibuat menggunakan 5 kriteria.

| Kriteria | Skor (1-5) | Justifikasi |
|----------|-----------|-------------|
| Clarity | 4 | Sangat jelas, masalah dan konteks wilayahnya tergambar dengan baik. |
| Measurability | 5 | Sangat terukur. Parameter evaluasi jelas menggunakan nilai kecocokan (*fitness*), jarak (km), dan waktu komputasi. |
| Relevance | 5 | Sangat relevan. Kemacetan dan efisiensi rute pariwisata adalah isu nyata di Bandung. |
| Testability | 5 | Jelas dapat diuji. Peneliti dapat melakukan simulasi iterasi generasi (seperti yang dilakukan 25 kali di paper). |
| Impact | 4 | Jika sistem ini berhasil mengatasi masalah rute zig-zag, dampaknya sangat besar bagi industri pariwisata lokal. |

**Skor total:** 23 / 25

**Problem statement versi final (1 paragraf):**
> Wisatawan di wilayah Bandung Raya sering menghadapi kesulitan dalam menentukan rute perjalanan yang efisien, terutama pada akhir pekan, mengingat terdapat 162 obyek wisata dan 500 titik persimpangan jalan yang harus dipertimbangkan. Kesulitan dalam mengevaluasi ribuan kombinasi persimpangan secara manual ini menyebabkan pemborosan waktu tempuh. Oleh karena itu, penelitian ini bertujuan untuk mengevaluasi kinerja Algoritma Genetika dalam merancang sistem rekomendasi yang mampu mencari rute terpendek dan tercepat berdasarkan variabel jarak tempuh, estimasi waktu, dan indeks kemacetan jalan, serta menganalisis kendala algoritma saat menghasilkan rute yang masih berbentuk zig-zag.
---

## Refleksi

> Bandingkan "masalah" yang biasa ditemui saat coding (bug, error) dengan masalah riset. Apa perbedaan fundamental dalam cara mendefinisikan dan mendekati keduanya?

**Jawaban:**
> Masalah saat coding (seperti bug atau error saat membuat program JST/Web) berorientasi pada penyelesaian (Engineering/Solve). Tujuannya adalah membuat artefak itu bekerja sesuai spesifikasi; kegagalan adalah hal yang harus diperbaiki dan dihilangkan. 
> 
> Sebaliknya, masalah riset berorientasi pada pemahaman dan pembuktian (Research/Understand & Prove). Contohnya pada kasus paper ini, ditemukannya fakta bahwa rute yang dihasilkan masih "zig-zag" bukanlah sebuah bug yang harus ditutupi, melainkan temuan riset yang valid dan membuktikan adanya gap pengetahuan terkait kemampuan mutasi/persilangan Algoritma Genetika pada kasus tersebut. Masalah coding diakhiri dengan program jalan, masalah riset diakhiri dengan kesimpulan yang terukur.
