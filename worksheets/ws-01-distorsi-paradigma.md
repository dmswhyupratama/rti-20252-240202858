# WS-01: Distorsi & Paradigma

> **Bab 1 — Research Mindset in IT**

---

## Ringkasan Materi

### Research Trust Model

Pengetahuan ilmiah tidak muncul langsung dari kenyataan. Ia melewati **6 tahap transformasi** yang masing-masing rawan distorsi:

```
Reality → Data → Processing → Analysis → Inference → Knowledge
```

Etika mencegah distorsi yang disengaja (fabrikasi, cherry-picking). Validitas mendeteksi distorsi yang tidak disengaja (confounding variable, sampling bias).

### Tiga Jenis Validitas

| Jenis | Pertanyaan | Contoh Ancaman |
|-------|-----------|----------------|
| **Internal Validity** | Apakah hubungan kausal benar ada? | Confounding variable |
| **External Validity** | Apakah bisa digeneralisasi? | Dataset terlalu homogen |
| **Construct Validity** | Apakah mengukur hal yang benar? | Metrik tidak sesuai klaim |

### Paradigma Riset

Mata kuliah ini menggunakan pendekatan **Positivist** (fenomena TI bisa diukur objektif melalui eksperimen terkontrol) diperkuat **Design Science Research** (artefak dibuat sebagai instrumen pengujian hipotesis, bukan tujuan akhir).

### Mode Berpikir Peneliti

**Curious** (mempertanyakan fenomena) → **Critical** (mengevaluasi klaim berdasarkan bukti) → **Systematic** (merancang investigasi terstruktur dan reproducible).

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan | Membuat sistem yang bekerja | Menghasilkan pengetahuan yang valid |
| Pertanyaan khas | "Bagaimana membuatnya jalan?" | "Apakah klaim ini benar?" |
| Ukuran sukses | Sistem berfungsi, client puas | Hipotesis terjawab, temuan tervalidasi |
| Kegagalan | Harus dihindari | Harus dilaporkan (negative result = kontribusi) |

### Istilah Penting

- **Research Mindset** — Pola pikir yang menuntut bukti dan mempertanyakan asumsi
- **Research Ethics** — Prinsip perilaku: kejujuran, objektivitas, keterbukaan, akuntabilitas
- **HARKing** — Hypothesizing After Results are Known — merumuskan hipotesis setelah melihat data
- **Falsifiability** — Hipotesis harus bisa dibuktikan salah

---

## Template A.1 — Research Mindset Self-Assessment

```
Nama Peneliti    : Dimas Wahyu Pratama
Tanggal          : 19 April 2026

1. Ketika membaca klaim "nilai kecocokan 910.2431 dari 117 generasi":
   - Pertanyaan pertama saya: Apakah nilai fitness tersebut benar-benar merepresentasikan rute yang efisien di dunia nyata atau hanya angka optimal secara matematis dalam model simulasi? [cite: 10, 257]
   - Data yang dibutuhkan untuk verifikasi: Perbandingan visual rute yang dihasilkan dengan kondisi lalu lintas sebenarnya di Google Maps pada waktu yang sama.

2. Posisi paradigma:
   - Pendekatan: [ ] Positivis  [ ] Interpretivis  [x] Design Science  [ ] Mixed
   - Alasan: Penelitian ini fokus pada pengembangan sebuah artefak (sistem perangkat lunak) untuk memecahkan masalah praktis (penentuan rute wisata)[cite: 32, 254].

3. Identifikasi distorsi:
   - Asumsi tersembunyi: Diasumsikan bahwa kemacetan di Bandung bisa disederhanakan menjadi 3 bobot statis (1.0, 1.5, 2.0).
   - Sumber bias potensial: Sampling Bias (hanya menggunakan 162 lokasi dari ribuan kemungkinan titik di Bandung)[cite: 33, 86].
   - Langkah mitigasi: Mengintegrasikan data lalu lintas real-time dari API pihak ketiga daripada menggunakan bobot statis.

4. Komitmen etika:
   - Data yang tidak akan dimanipulasi: Hasil pengujian nilai kecocokan (fitness) dan jumlah generasi yang diperlukan hingga konvergen.
   - Batasan yang diakui sejak awal: Peneliti mengakui bahwa rute yang dihasilkan sistem masih belum sempurna karena masih berbentuk zig-zag.
```

---

## Latihan 1 — Identifikasi Distorsi

**Paper yang dipilih:**
> Judul: Optimalisasi Rute Obyek Wisata Di Bandung Raya Menggunakan Algoritma Genetika
> Penulis (Tahun): Nur Muhammad Hasyim, Esmeralda C. Djamal, Agus Komarudin (2017)
>  Sumber/Link DOI: https://journal.uii.ac.id/Snati/article/view/8490/7215

| Tahap | Apa yang Dilakukan | Potensi Distorsi |
|-------|-------------------|-----------------|
| Reality → Data | Mengambil 500 titik persimpangan dari Google Maps API dan menentukan bobot kemacetan statis (1.0 - 2.0). | **Simplification Bias**: Kondisi nyata lalu lintas Bandung sangat dinamis; penyederhanaan menjadi 3 bobot kaku menghilangkan variabel penting seperti cuaca atau penutupan jalan sementara. |
| Data → Processing | Inisialisasi 8 kromosom dengan panjang 30 gen yang mewakili titik-titik persimpangan. | **Representation Error**: Panjang kromosom yang statis (30 gen) mungkin tidak cukup untuk mewakili rute yang sangat kompleks atau terlalu panjang untuk rute yang dekat. |
| Processing → Analysis | Menghitung fitness menggunakan fungsi kecocokan dan melakukan seleksi Rank Based Fitness. | **Algorithmic Bias**: Penggunaan Rank Based Fitness mungkin menyebabkan hilangnya variasi genetik terlalu cepat jika tidak diseimbangkan dengan laju mutasi yang tepat. |
| Analysis → Inference | Membandingkan 5 rute terbaik dari 25 kali pengujian total. | **Cherry-picking**: Peneliti cenderung menonjolkan nilai kecocokan tertinggi (910.2431) di kesimpulan, padahal banyak pengujian lain yang hasilnya jauh di bawah itu. |
| Inference → Knowledge | Menyimpulkan sistem dapat memberikan rekomendasi rute terpendek dan tercepat. | **Overgeneralization**: Klaim "rute tercepat" sulit divalidasi karena model hanya menggunakan bobot estimasi, bukan durasi perjalanan nyata yang divalidasi di lapangan. |

**Distorsi paling besar di tahap:** **Analysis → Inference**

**Dua distorsi spesifik yang teridentifikasi:**
1. **Reporting Bias**: Peneliti baru mengungkapkan di bagian akhir kesimpulan bahwa rute yang dihasilkan "masih dalam keadaan zig-zag", padahal di abstrak diklaim sebagai sistem optimasi yang berhasil.
2. **External Validity Threat**: Penggunaan bobot kemacetan 1.0, 1.5, dan 2.0 merupakan distorsi data yang sangat besar karena tidak mencerminkan fluktuasi waktu tempuh yang sebenarnya di area Bandung Raya.
---

## Latihan 2 — Analisis Kasus Etika

Skenario: Seorang peneliti menemukan bahwa jika 3 data point outlier dihapus, hasil eksperimennya menjadi signifikan. Dengan outlier, hasilnya tidak signifikan.

| Perspektif | Analisis |
|------------|---------|
| Kejujuran ilmiah | Dalam konteks paper ini, peneliti menunjukkan kejujuran dengan tetap melaporkan bahwa rute yang dihasilkan masih zig-zag. Menghapus outlier tanpa alasan metodologis adalah pelanggaran kejujuran. |
| Transparansi | Peneliti harus menjelaskan kriteria *outlier* tersebut. Jika dihapus, harus dilaporkan alasan teknisnya (misalnya karena gangguan koneksi API saat pengambilan data). |
| Peer review | Laporan yang jujur tentang kegagalan atau rute zig-zag membantu penelaah memberikan masukan yang tepat untuk perbaikan algoritma ke depannya. |

**Keputusan akhir dan justifikasi:**
> Peneliti harus melaporkan kedua hasil tersebut (dengan dan tanpa outlier). Justifikasinya adalah prinsip faksifiabilitas; kegagalan atau ketidakkonsistenan data adalah bagian dari pengetahuan ilmiah yang valid untuk dilaporkan (negative results are still contributions).
---

## Latihan 3 — Posisi Paradigma

**Topik riset:** Optimalisasi Rute Wisata Bandung dengan Algoritma Genetika

| Kriteria | Positivis | Interpretivis | Design Science |
|----------|-----------|---------------|----------------|
| Kesesuaian dengan topik (1–5) | 3 | 1 | 5 |
| Jenis data yang dikumpulkan | [cite_start]Data kuantitatif seperti nilai *fitness*, jumlah generasi, dan waktu proses[cite: 212]. | Persepsi atau kepuasan wisatawan terhadap rute yang disarankan. | [cite_start]Kinerja artefak (sistem) dalam memproses dan menghasilkan rute[cite: 32]. |
| Limitasi paradigma | Hanya fokus pada angka hasil pengujian tanpa mempertimbangkan kenyamanan nyata pengguna. | Sangat subjektif, bergantung pada perasaan orang, dan sulit diukur secara teknis IT. | Terlalu fokus pada fungsionalitas sistem (apakah sistem jalan atau tidak), seringkali mengabaikan teori dasar yang lebih mendalam. |

**Paradigma yang dipilih:** **Design Science**
[cite_start]**Alasan:** Riset ini berfokus pada penciptaan sebuah artefak berupa sistem perangkat lunak untuk memberikan solusi praktis atas masalah pencarian rute wisata di Bandung Raya[cite: 32, 254].

---

## Refleksi

> Sebelum membaca materi ini, apakah pernah mempertanyakan klaim "95% akurat"? Setelah memahami rantai distorsi, pertanyaan apa yang sekarang akan diajukan saat membaca paper?

**Jawaban:**
> Sebelumnya, saya cenderung menerima hasil akhir di abstrak begitu saja. Namun setelah mempelajari rantai distorsi, saya akan lebih kritis mempertanyakan tahap **Reality → Data**. [cite_start]Saya akan mempertanyakan bagaimana peneliti menyederhanakan kondisi dunia nyata (seperti kemacetan Bandung yang dinamis) menjadi variabel angka statis, dan apakah penyederhanaan tersebut justru menghilangkan esensi dari masalah yang ingin dipecahkan[cite: 38, 124].

---

**Referensi:**
1. Nur Muhammad Hasyim, Esmeralda C. Djamal, Agus Komarudin. (2017). "Optimalisasi Rute Obyek Wisata Di Bandung Raya Menggunakan Algoritma Genetika". [cite_start]Seminar Nasional Aplikasi Teknologi Informasi (SNATi) 2017. [cite: 1, 20]
