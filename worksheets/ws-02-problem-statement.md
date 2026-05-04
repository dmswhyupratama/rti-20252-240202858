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
  Domain   : Software Quality Assurance (SQA) / Pengujian Perangkat Lunak.
  Konteks  : Pengujian fungsionalitas validasi form pada modul Inbound dan Outbound di aplikasi manajemen gudang (Agri-Pos).

System Context
  Input       : Data transaksi barang (angka stok, ID barang, harga) dengan berbagai variasi kondisi batas (valid, invalid, nilai ekstrim).
  Process     : Validasi data di level Controller (PHP MVC) untuk menolak atau menerima input sesuai dengan aturan logika bisnis.
  Output      : Pembaruan jumlah stok di database MySQL dan respon UI berupa pesan keberhasilan atau error.
  Outcome     : Integritas data inventori terjaga sehingga laporan stok gudang selalu akurat dan terhindar dari kerugian operasional.
  Constraints : Pengujian dilakukan murni melalui interaksi antarmuka (Black-Box) secara manual, tanpa memodifikasi atau melihat source code internal.
  Stakeholders: Quality Assurance (Peneliti), Developer (Pengembang Agri-Pos), dan Admin/Staff Gudang (Pengguna Sistem).

Fenomena → Problem
  Fenomena yang diamati             : Sering terjadi selisih pencatatan stok di gudang akibat human-error saat karyawan melakukan input data secara cepat.
  Gejala (symptom) yang terukur     : Sistem terkadang menerima input yang tidak masuk akal (misal: stok menjadi minus, atau lolosnya input huruf pada field kuantitas).
  Masalah yang didiagnosis          : Lemahnya validasi di level Controller karena pengembang mayoritas hanya melakukan pengujian ad-hoc (happy path), mengabaikan skenario batas ekstrim (edge cases).
  Masalah riset (researchable)      : Apakah pengujian menggunakan metode Equivalence Partitioning (EP) dan Boundary Value Analysis (BVA) lebih efektif menemukan cacat fungsional dibanding pengujian ad-hoc pada modul Inbound/Outbound Agri-Pos?
  Variabel yang terukur             : Defect Detection Rate (DDR) / Persentase Skenario Uji (Test Case) yang berstatus 'Fail' atau menemukan bug.

Problem Quality Check
  [x] Clarity — Apakah satu orang membaca akan paham?
  [x] Measurability — Apakah ada metrik kuantitatif?
  [x] Relevance — Apakah penting untuk domain?
  [x] Testability — Apakah bisa gagal?
  [x] Impact — Apakah ada kontribusi jika terjawab?

Problem Statement (1 paragraf):
  Dalam operasional sistem manajemen gudang seperti Agri-Pos, integritas data pada modul Inbound dan Outbound sangat krusial. Namun, fenomena yang sering terjadi adalah munculnya cacat data (seperti stok bernilai negatif atau error database) akibat sistem gagal menangani kesalahan input tak terduga dari pengguna. Akar masalahnya terletak pada praktik pengujian pengembang yang seringkali hanya menguji skenario normal (happy path) secara ad-hoc, mengabaikan pengujian pada titik batas (edge cases). Oleh karena itu, penelitian ini bertujuan untuk mengevaluasi tingkat efektivitas penemuan cacat perangkat lunak (Defect Detection Rate) menggunakan metode pengujian terstruktur Boundary Value Analysis (BVA) dan Equivalence Partitioning (EP), guna memastikan sistem kebal terhadap anomali input sebelum digunakan secara masal.
```

---

## Latihan 1 — Dari Topik ke Masalah Riset

**Topik awal:** Evaluasi Pengujian Black-Box (EP & BVA) pada Aplikasi Web Agri-Pos

| Tahap | Hasil |
|-------|-------|
| Reality | Karyawan gudang rentan melakukan *typo* atau salah *input* saat mencatat sirkulasi barang masuk/keluar di aplikasi web yang sedang sibuk. |
| Observed Issue (Symptom) | Data di *database* menjadi rusak (stok berkurang melebihi batas atau masuk karakter aneh) karena aplikasi tidak menolak *input* yang salah. |
| Diagnosed Problem (Root Cause) | Aplikasi belum diuji secara komprehensif menggunakan skenario anomali atau batas ekstrim, sehingga celah fungsional di tingkat *Controller* tidak terdeteksi oleh pengembang. |
| Researchable Problem | Seberapa besar tingkat efektivitas deteksi cacat fungsional yang bisa diidentifikasi jika menggunakan metode *Boundary Value Analysis* (BVA) dan *Equivalence Partitioning* (EP) dibandingkan pengujian biasa (*ad-hoc*)? |
| Measurable Variable | *Defect Detection Rate* (DDR), rasio persentase *Test Case* yang berstatus Gagal (menemukan bug). |

**Apakah terjebak solution-first thinking?** [ ] Ya / [x] Tidak
> Jika ya, kembali ke tahap mana? -

---

## Latihan 2 — System Context Decomposition

Gambarkan konteks sistem dari masalah riset di Latihan 1.

| Komponen | Deskripsi |
|----------|----------|
| Input | Masukan pengguna berupa nilai *integer* (jumlah barang), *string* (nama/kode barang), dan kondisi batas uji ekstrim (contoh: input 0, -1, huruf, atau angka maksimal). |
| Process | Penanganan *request* oleh logika *Controller* MVC (mengevaluasi apakah sistem memiliki blok validasi yang cukup ketat untuk memfilter anomali *input*). |
| Output | Respon antarmuka web (Pesan Error Validasi / Pesan Sukses) dan eksekusi instruksi penyimpanan ke tabel *database*. |
| Outcome | Keandalan aplikasi (*Reliability*); aplikasi menjadi aman dari malfungsi operasional akibat kelalaian *user* (integritas data stok terjaga). |
| Constraints | Pengujian terbatas pada interaksi antarmuka *front-end* secara manual, tanpa memodifikasi *source code* (White-Box) secara langsung. |
| Stakeholders | Tim *Quality Assurance* (Penguji), Pengembang Sistem (*Developer*), dan Admin Gudang. |

---
**Komponen mana yang paling relevan dengan masalah riset?** **Process**. Karena pada tahap proses di *Controller*-lah logika validasi bekerja. Kegagalan proses memvalidasi *input* ekstrim menjadi fokus utama untuk dibongkar oleh skenario pengujian BVA dan EP.

---

## Latihan 3 — Problem Quality Check

Evaluasi problem statement yang sudah dibuat menggunakan 5 kriteria.

| Kriteria | Skor (1-5) | Justifikasi |
|----------|-----------|-------------|
| Clarity | 5 | Sangat jelas. Menyebutkan objek aplikasi (Agri-Pos), area fokus (Inbound/Outbound), dan teknik spesifik (EP & BVA). |
| Measurability | 5 | Sangat terukur. Bisa dihitung dengan membagi jumlah *bug* yang ditemukan dengan total *Test Case* dieksekusi (*Defect Detection Rate*). |
| Relevance | 5 | Sangat relevan di dunia rekayasa perangkat lunak (*Software Engineering*). Mencegah *bug* integritas data adalah prioritas utama sebelum aplikasi *live*. |
| Testability | 5 | Sangat bisa diuji dengan cara menyusun matriks uji (skenario) dan mengeksekusinya langsung di *browser*. |
| Impact | 5 | Berdampak besar untuk menghasilkan *SOP Test Case* yang valid secara akademis maupun praktis untuk standar industri perangkat lunak. |

**Skor total:** 25 / 25

**Problem statement versi final (1 paragraf):**
> Dalam operasional sistem manajemen gudang seperti Agri-Pos, integritas data pada modul Inbound dan Outbound sangat krusial. Namun, fenomena yang sering terjadi adalah munculnya cacat data (seperti stok bernilai negatif atau *error database*) akibat sistem gagal menangani kesalahan input tak terduga dari pengguna. Akar masalahnya terletak pada praktik pengujian pengembang yang seringkali hanya menguji skenario normal (*happy path*) secara ad-hoc, mengabaikan pengujian pada titik batas (*edge cases*). Oleh karena itu, penelitian ini bertujuan untuk mengevaluasi tingkat efektivitas penemuan cacat perangkat lunak (*Defect Detection Rate*) menggunakan metode pengujian terstruktur *Boundary Value Analysis* (BVA) dan *Equivalence Partitioning* (EP), guna memastikan sistem kebal terhadap anomali input sebelum digunakan secara masal.

---

## Refleksi

> Bandingkan "masalah" yang biasa ditemui saat coding (bug, error) dengan masalah riset. Apa perbedaan fundamental dalam cara mendefinisikan dan mendekati keduanya?

**Jawaban:**
> Masalah *coding* (seperti *syntax error* atau *bug* fungsional saat membuat program PHP MVC) berorientasi pada penyelesaian masalah teknis (*Engineering/Solve*). Pendekatannya bersifat reaktif: begitu *bug* ditemukan, *developer* langsung melakukan *debugging* untuk memastikan program berjalan sesuai spesifikasi aslinya (solusi-orientasi). 
> 
> Sebaliknya, masalah riset (khususnya dalam *Software Quality Assurance*) berorientasi pada pemahaman dan pembuktian (*Research/Understand & Prove*). Tujuannya bukan semata-mata memperbaiki *bug* yang ada di depan mata, melainkan mengevaluasi *seberapa efektif metode* kita dalam **membongkar** atau menemukan *bug* tersebut secara terstruktur. Dalam riset pengujian perangkat lunak, sistem yang gagal merespon *input* ekstrim bukanlah sebuah aib yang harus cepat-cepat dihapus, melainkan merupakan sebuah bukti empiris (*negative result*) yang bernilai tinggi untuk memvalidasi kualitas dan ketangguhan teknik pengujian (seperti BVA/EP) yang digunakan.