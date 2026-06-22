# WS-05: Variabel & Metrik

> **Bab 5 — Metric, Measurement & Data**

---

## Ringkasan Materi

### Measurement Alignment Model

Setiap pengukuran yang valid harus bisa ditelusuri melalui rantai ini tanpa lompatan logis:

```
Problem → Concept → Variable → Metric → Data → Result
```

### Operationalization = Keputusan Desain

Menerjemahkan konsep abstrak menjadi variabel terukur bukan proses mekanis. "Code quality" yang diukur via SonarQube code smells membawa asumsi implisit. Setiap operasionalisasi harus didokumentasikan dan dijustifikasi.

### Empat Tipe Data (NOIR)

| Tipe | Ciri | Contoh | Operasi Valid |
|------|------|--------|---------------|
| **Nominal** | Kategori, tanpa urutan | Jenis algoritma (RF, SVM, CNN) | Modus, chi-square |
| **Ordinal** | Urutan, interval tidak sama | Skala Likert (1-5) | Median, Spearman |
| **Interval** | Jarak bermakna, tanpa nol absolut | Suhu Celsius | Mean, Pearson, t-test |
| **Ratio** | Jarak bermakna + nol absolut | Waktu eksekusi (ms) | Semua operasi |

Tipe data menentukan uji statistik yang valid. Kebanyakan metrik performa TI = ratio; persepsi pengguna = ordinal.

### Kriteria Pemilihan Metrik

- **Representative** — Mewakili konsep yang diteliti
- **Sensitive** — Cukup peka menangkap perbedaan bermakna (hindari ceiling effect)
- **Feasible** — Bisa dikumpulkan dalam batasan waktu dan biaya

### Pre-registration

Metrik harus ditentukan **sebelum** eksperimen. Memilih metrik setelah melihat data = **p-hacking**. Metrik tambahan yang ditemukan kemudian dilaporkan sebagai *exploratory*, bukan *confirmatory*.

### Primary vs Secondary Metric

- **Primary Metric** — Langsung terikat ke hipotesis, menentukan kesimpulan
- **Secondary Metric** — Pendukung, dilaporkan di samping primary; statusnya suplementer

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Pemilihan metrik | Berdasarkan kebiasaan/tool yang ada | Berdasarkan construct validity |
| Anomali | Dihapus untuk laporan bersih | Diinvestigasi — bisa jadi temuan |
| Kapan dipilih | Setelah sistem jadi (monitoring) | Sebelum eksperimen (by design) |

### Istilah Penting

- **Operationalization** — Transformasi konsep abstrak menjadi variabel terukur
- **Construct Validity** — Sejauh mana pengukuran benar-benar mengukur konsep yang dimaksud
- **Measurement Scale** — Klasifikasi data (NOIR) yang menentukan analisis valid
- **Multi-metric Evaluation** — Menggunakan beberapa metrik untuk menangkap konsep kompleks

---

## Template A.5 — Definisi Variabel, Metrik & Justifikasi

VARIABLE & METRIC DEFINITION

Research Question: Seberapa besar peningkatan nilai Defect Detection Rate (DDR) dari pengujian Boundary Value Analysis (BVA) dan Equivalence Partitioning (EP) dibandingkan dengan pengujian kasual (ad-hoc) pada Controller MVC Sistem Manajemen Gudang Hasil Tani?

| Variabel           | Tipe | Konsep              | Metrik                           | Skala   | Satuan         | Cara Mengukur                                              | Justifikasi                                                                                   |
|--------------------|------|---------------------|----------------------------------|---------|----------------|------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| Pendekatan Uji     | IV   | Teknik validasi     | Kategorikal: BVA/EP vs Ad-Hoc    | Nominal | —              | Eksekusi sistem menggunakan/tanpa panduan matriks kasus uji| Merupakan perlakuan (intervensi) utama yang membedakan dua kelompok eksperimen.               |
| Efektivitas Uji    | DV   | Kemampuan deteksi   | Defect Detection Rate (DDR)      | Rasio   | Persentase (%) | Rumus: (Jumlah Bug Fungsional / Total Kasus Uji) * 100     | DDR secara objektif menormalkan perhitungan bug terhadap jumlah skenario uji yang dieksekusi. |
| Lingkungan Sistem  | CV   | Kestabilan Artefak  | Kondisi DB & Versi Source Code   | Nominal | —              | Reset database setiap siklus dan pembekuan (freeze) kode   | Memastikan bahwa perbedaan nilai DDR murni karena metode uji, bukan karena sistem yang berubah|

Alignment Check:
  RQ → Concept → Variable → Metric → Data → Result
  [x] Setiap langkah terdokumentasi
  [x] Tidak ada "lompatan logis"
  [x] Metrik mengukur apa yang dimaksud (construct validity)

---

## Latihan 1 — Operationalization Chain

Gunakan RQ dari WS-04. Definisikan variabel dan metriknya.

**RQ:** Seberapa besar peningkatan nilai Defect Detection Rate (DDR) dari pengujian Boundary Value Analysis (BVA) dan Equivalence Partitioning (EP) dibandingkan dengan pengujian kasual (ad-hoc) pada Controller MVC Sistem Manajemen Gudang Hasil Tani?

| Variabel | Tipe | Konsep Abstrak | Metrik Konkret | Skala (NOIR) | Satuan |
|----------|------|---------------|----------------|-------------|--------|
| Pendekatan Uji | IV | Teknik pengujian sistem | Categorical: BVA/EP vs Ad-hoc | Nominal | — |
| Efektivitas Deteksi | DV | Kemampuan membongkar celah logika | Defect Detection Rate (DDR) | Rasio | Persentase (%) |
| Lingkungan Sistem | CV | Kestabilan platform observasi | Kondisi Database & Kode Program | Nominal | Status (Reset/Freeze) |

**Apakah ada lompatan logis dalam rantai?** [ ] Ya / [x] Tidak
> **Jika ya, di mana?** Rantai logikanya solid karena metrik DDR secara matematis dan langsung dihitung berdasarkan respons keluaran dari manipulasi input (IV) yang dimasukkan ke dalam sistem.

---

## Latihan 2 — Evaluasi Metrik

Evaluasi metrik DV yang dipilih di Latihan 1 menggunakan 3 kriteria.

| Kriteria | Skor (1-5) | Justifikasi |
|----------|-----------|-------------|
| Representative | 5 | Metrik DDR secara persis mewakili definisi "efektivitas" dalam SQA karena menunjukkan rasio keberhasilan skenario uji dalam menemukan bug fungsional. |
| Sensitive | 4 | Sangat peka terhadap perbedaan jumlah deteksi. Namun, jika kontrol validasi Controller benar-benar belum ada sama sekali, nilainya bisa mentok. |
| Feasible | 5 | Sangat mudah dan murah dikumpulkan. Hanya membutuhkan pencatatan log hasil eksperimen (Status: Lulus/Gagal) di Test Case Matrix. |

**Apakah perlu secondary metric?** [x] Ya / [ ] Tidak
> **Jika ya, apa dan mengapa?** Ya, Waktu Eksekusi Pengujian (Test Execution Time). Sangat berguna sebagai metrik pendukung untuk melihat apakah tingginya DDR dari metode BVA/EP sepadan dengan waktu yang harus dikorbankan (trade-off antara efektivitas vs efisiensi).

**Contoh kasus ceiling effect untuk metrik ini:**
> Jika lapisan Controller pada aplikasi Hasil Tani sama sekali tidak memiliki pelindung atau filter input dasar, maka baik pengujian Ad-hoc maupun BVA/EP akan sama-sama berhasil menembus 100% kelemahan sistem. Hal ini membuat nilai DDR keduanya mentok di angka 100%, sehingga gagal memperlihatkan metode mana yang sebenarnya lebih baik.

---

## Latihan 3 — Data Quality Check

Bayangkan data yang akan dikumpulkan dari eksperimen. Evaluasi 4 dimensi kualitas data.

| Dimensi | Pertanyaan | Jawaban | Strategi Mitigasi |
|---------|-----------|---------|------------------|
| Completeness | *Apakah semua data point terkumpul?* | Ya, asalkan penguji disiplin mencatat setiap test case. | Membuat formulir (matriks) yang memaksa penguji mencentang status "Berhasil/Gagal" untuk setiap baris sebelum lanjut. |
| Consistency | *Apakah ada kontradiksi internal?* | Sangat rentan jika definisi "bug" tidak sama antara sesi uji. | Menuliskan definisi bug yang kaku sejak awal (misal: bug dihitung JIKA aplikasi crash atau JIKA data lolos tersimpan ke DB). |
| Validity | *Apakah benar-benar mengukur yang dimaksud?* | Bisa bias jika bug antarmuka (UI) ikut dihitung. | Mengunci pencatatan. Bug UI/visual (seperti tombol salah warna) ditolak dari perhitungan; murni hanya menghitung bug fungsional logika logika Controller. |
| Representativeness | *Apakah sampel mewakili populasi target?* | Sangat bergantung pada angka uji coba (dummy data). | Menarik angka batas BVA/EP langsung dari Aturan Bisnis (SOP) nyata gudang Hasil Tani, bukan angka karangan bebas. |

---

## Refleksi

> Mengapa memilih metrik setelah melihat data dianggap p-hacking? Apa bedanya dengan eksplorasi data yang sah?

**Jawaban:**
> Memilih metrik setelah melihat hasil data (p-hacking) adalah sebuah kecurangan eksperimen (cherry-picking) karena peneliti bisa dengan sengaja menyeleksi metrik yang hanya menguntungkan dan mengkonfirmasi hipotesisnya, sambil membuang metrik yang menggagalkan klaimnya. Ini akan menghasilkan temuan bias yang tidak bisa direplikasi.
> 
> Hal ini sangat berbeda dengan "Eksplorasi Data" yang sah. Pada eksplorasi yang sah, metrik utama (seperti DDR) sudah dikunci secara pre-registration sebelum uji dimulai. Jika setelah eksperimen peneliti menemukan pola menarik lainnya di dalam data (misalnya: bug paling sering muncul di form berat barang tertentu), temuan itu dilaporkan secara jujur sebagai "Temuan Eksploratif" atau wawasan tambahan (insight), BUKAN digunakan untuk merekayasa atau mengubah klaim hipotesis utama.