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

Mata kuliah ini menggunakan pendekatan **Positivist** (fenomena TI bisa diukur objektif melalui eksperimen terkontrol) diperkuat **Design Science Research** (DSR). Penting untuk membedakan keduanya:

| Paradigma | Cara Kerja | Contoh di TI |
|-----------|-----------|---------------|
| **Positivis** | Uji hipotesis dengan eksperimen terkontrol | Apakah CNN lebih akurat dari RF pada dataset X? |
| **Design Science Research** | Bangun artefak (sistem/model/framework) untuk menguji proposisi | Dapatkah arsitektur hybrid CNN+LSTM membuktikan peningkatan recall ≥5%? |
| **Interpretivis** | Pahami makna melalui konteks & kualitatif | Bagaimana peneliti manafsirkan anomali data sensor IoT? |

Dalam DSR, artefak **bukan tujuan akhir** — ia adalah instrumen untuk menghasilkan pengetahuan. Pertanyaan riset tetap harus difalsifikasi.

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
Tanggal          : 4 Mei 2026

1. Ketika membaca klaim "Aplikasi 100% valid dan bebas dari error fungsional":
   - Pertanyaan pertama saya: Apakah pengujian tersebut mencakup skenario batas ekstrim (edge cases), ataukah hanya menguji alur normal (happy path) saja?
   - Data yang dibutuhkan untuk verifikasi: Dokumen Test Case Matrix yang secara eksplisit mencantumkan nilai input uji di luar kebiasaan (misal: angka negatif, karakter non-numerik, atau batas maksimal integer).

2. Posisi paradigma:
   - Pendekatan: [x] Positivis  [ ] Interpretivis  [ ] Design Science  [ ] Mixed
   - Alasan: Penelitian ini fokus pada evaluasi objektif dan pengukuran empiris terhadap tingkat deteksi cacat (Defect Detection Rate) suatu perangkat lunak menggunakan teknik eksperimen terkontrol.

3. Identifikasi distorsi:
   - Asumsi tersembunyi: Diasumsikan bahwa pengguna akan selalu menginput data stok sesuai dengan format yang diminta oleh sistem.
   - Sumber bias potensial: Sampling Bias (hanya menguji sebagian kecil form dasar seperti login, dan mengabaikan form krusial seperti transaksi inbound/outbound).
   - Langkah mitigasi: Menerapkan metode Boundary Value Analysis (BVA) dan Equivalence Partitioning (EP) untuk memastikan semua kelompok data (valid dan invalid) terwakili dalam sampel pengujian.

4. Komitmen etika:
   - Data yang tidak akan dimanipulasi: Status kelulusan (Pass/Fail) dari setiap skenario pengujian. Jika sistem mengalami crash, mutlak dicatat sebagai 'Fail' (bug).
   - Batasan yang diakui sejak awal: Peneliti mengakui bahwa pengujian Black-Box ini hanya menguji fungsionalitas dari sisi antarmuka dan validasi Controller, tanpa melakukan code-review (White-Box) pada source code internal.
```

## Latihan 1 — Identifikasi Distorsi

Pilih satu paper riset di bidang TI yang mengklaim "metode X meningkatkan performa." Telusuri setiap tahap Research Trust Model.

**Paper yang dipilih:**
> Judul: _______________________________________________
> Penulis (Tahun): ______________________________________

| Tahap | Apa yang Dilakukan | Potensi Distorsi |
|-------|-------------------|-----------------|
| Reality → Data | Mengamati proses pengelolaan stok saat ini dan merancang skenario pengujian fungsional. | **Simplification Bias**: Peneliti mungkin hanya berfokus pada form input sederhana, mengabaikan kompleksitas perilaku pengguna nyata yang sering melakukan kesalahan ketik atau input massal. |
| Data → Processing | Membagi data menjadi partisi valid dan invalid (EP) serta nilai batas (BVA). | **Representation Error**: Pemilihan batas nilai (boundary) yang salah atau tidak sesuai dengan tipe data database aktual (misalnya VARCHAR vs INT) dapat menghasilkan skenario yang tidak relevan. |
| Processing → Analysis | Mengeksekusi pengujian secara manual terhadap form harga dan stok. | **Confirmation Bias**: Penguji yang melakukan secara manual bisa secara tidak sadar melewati skenario yang dianggap "pasti berhasil" dan lebih fokus mencari error di area yang mereka curigai saja. |
| Analysis → Inference | Menyimpulkan bahwa sistem berfungsi dengan baik dengan beberapa anomali minor. | **Overgeneralization**: Menyimpulkan keseluruhan sistem aman padahal yang diuji mungkin hanya sebagian kecil modul utama saja. |
| Inference → Knowledge | Menyarankan perbaikan validasi input dan pengujian lanjutan. | **Reporting Bias**: Peneliti terkadang enggan menonjolkan kegagalan fatal (critical bug) agar aplikasi yang diuji tidak terlihat buruk secara publik. |

**Distorsi paling besar di tahap:** **Processing → Analysis**

**Dua distorsi spesifik yang teridentifikasi:**
1. **Confirmation Bias**: Penguji manual cenderung menguji apa yang sudah mereka ketahui, sehingga berpotensi melewatkan *blind spot* pada modul tertentu.
2. **Overgeneralization**: Klaim "aplikasi berjalan dengan baik" sering kali tidak sejalan dengan cakupan pengujian (*test coverage*) yang sebenarnya sangat terbatas.

---

## Latihan 2 — Analisis Kasus Etika

Skenario: Seorang peneliti/QA menemukan bahwa saat menginput angka minus (-50) pada form Outbound, database mengalami *fatal crash* dan merusak tabel lain. Karena takut aplikasi dinilai buruk oleh dosen/klien, ia menghapus test case tersebut dari laporannya dan hanya melaporkan error kecil seperti "salah password".

| Perspektif | Analisis |
|------------|---------|
| Kejujuran ilmiah | Ini adalah manipulasi data yang fatal (*cherry-picking*). Menghapus *test case* yang gagal secara fundamental melanggar prinsip kejujuran dalam Software Quality Assurance (SQA). |
| Transparansi | Seharusnya peneliti melaporkan *fatal crash* tersebut secara rinci, termasuk variabel input apa yang memicunya, agar pengembang (developer) bisa melakukan *patching* atau perbaikan di level Controller. |
| Peer review | Jika disembunyikan, *reviewer* atau pengguna akhir akan menemukan bug ini di tahap *production*, yang dampaknya (seperti kerugian inventori) akan jauh lebih merusak. |

**Keputusan akhir dan justifikasi:**
> Peneliti WAJIB melaporkan kegagalan sistem tersebut dalam Matriks Pengujian. Justifikasinya adalah esensi dari pengujian perangkat lunak (Software Testing) bukanlah untuk membuktikan bahwa aplikasi itu sempurna, melainkan untuk menemukan cacat (defect) sebanyak mungkin sebelum aplikasi dirilis. Negative results (sistem gagal) adalah kontribusi riset yang paling berharga.

---

## Latihan 3 — Posisi Paradigma

**Topik riset:** ________________________________________

| Kriteria | Positivis | Interpretivis | Design Science |
|----------|-----------|---------------|----------------|
| Kesesuaian dengan topik (1–5) | *Contoh: 4* | *Contoh: 2* | *Contoh: 5* |
| Jenis data yang dikumpulkan | | | |
| Limitasi paradigma | | | |

**Paradigma yang dipilih:** **Positivis**
**Alasan:** Penelitian ini mengevaluasi sebuah realitas objektif (ada atau tidaknya bug pada perangkat lunak) melalui serangkaian eksperimen terkontrol (matriks test case), di mana hasilnya berupa metrik kuantitatif (Defect Detection Rate) yang dapat direplikasi dan diverifikasi oleh pihak lain.

---

## Refleksi

> Sebelum membaca materi ini, apakah pernah mempertanyakan klaim "95% akurat"? Setelah memahami rantai distorsi, pertanyaan apa yang sekarang akan diajukan saat membaca paper?

**Jawaban:**
> Sebelumnya, saya cenderung menerima hasil akhir di abstrak begitu saja, seperti klaim "aplikasi 100% bebas bug". Namun setelah memahami rantai distorsi, saya akan lebih kritis mempertanyakan tahap **Reality → Data**. Saya akan menanyakan: Apakah skenario uji yang dirancang benar-benar merepresentasikan perilaku ekstrem pengguna di dunia nyata, ataukah peneliti melakukan *Simplification Bias* dengan hanya menguji input yang sudah pasti benar agar aplikasinya terlihat sempurna?