# WS-06: System-Experiment Mapping

> **Bab 6 — System Design sebagai Experimental Artifact**

---

## Ringkasan Materi

### Sistem = Instrumen Pengujian, Bukan Produk

Seorang engineer bertanya "apakah sistem bekerja?" — seorang peneliti bertanya "apa yang bisa dibuktikan sistem ini?" Sistem dalam riset adalah **artifact** — objek yang sengaja dibuat untuk menguji klaim spesifik.

### System as Experiment Model

```
RQ → Variable → System Component → Experimental Setup → Output
```

Setiap komponen sistem harus bisa ditelusuri ke variabel riset (top-down), dan setiap pengukuran harus menjawab RQ (bottom-up).

### Mapping Variabel ke Komponen

| Tipe Variabel | Peran di Sistem | Contoh |
|---------------|----------------|--------|
| **IV** (Independent) | Modul yang bisa di-toggle/swap | Algoritma A vs B |
| **DV** (Dependent) | Modul pengukuran | Logger, metrics collector |
| **CV** (Control) | Config yang dikunci | Dataset, parameter tetap |

Jika variabel tidak bisa di-map ke komponen apapun → arsitektur perlu didesain ulang.

### 4 Prinsip Desain Eksperimental

| Prinsip | Pertanyaan Kunci |
|---------|-----------------|
| **Traceability** | Komponen ini melayani variabel yang mana? |
| **Modularity** | Bisakah IV diubah tanpa memengaruhi yang lain? |
| **Controllability** | Apakah CV dieksternalisasi ke config file? |
| **Measurability** | Apakah sistem otomatis menghasilkan data yang dibutuhkan? |

### Variable Isolation melalui Arsitektur

- **Modular architecture** — Pisahkan berdasarkan variabel
- **Configuration-driven** — Ubah config (YAML/JSON), bukan code
- **Feature toggles** — On/off flag untuk ablation study

  Contoh config YAML dengan feature toggles:
  ```yaml
  model:
    type: cnn          # IV: ganti "rf" untuk kondisi baseline
  features:
    use_temporal: true  # toggle komponen temporal
    use_normalization: true  # toggle preprocessing
  experiment:
    seed: 42
    runs: 5
  ```
  Dengan pendekatan ini, berbeda kondisi eksperimen = berbeda satu baris config, **tanpa mengubah kode**.

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan sistem | Memenuhi kebutuhan user | Menguji hipotesis, menghasilkan bukti |
| Arsitektur | Optimasi performa & skalabilitas | Optimasi isolasi variabel & reprodusibilitas |
| Konfigurasi | Sering hardcoded | Dieksternalisasi ke config file |
| Fitur tambahan | Menambah nilai user | Menambah noise jika tidak terkait RQ |

### Istilah Penting

- **Artifact** — Objek yang sengaja dibuat untuk memecahkan masalah atau menguji proposisi
- **Traceability** — Kemampuan menelusuri hubungan RQ → variabel → komponen → output
- **Variable Isolation** — Mengubah hanya satu variabel sambil menahan yang lain konstan
- **Ablation Study** — Menguji kontribusi tiap komponen dengan melepasnya satu per satu
- **Configuration-driven Execution** — Semua parameter di config file, bukan hardcoded

---

## Template A.6 — Mapping RQ ke Arsitektur Sistem

SYSTEM-EXPERIMENT MAPPING

**Research Question:** Seberapa besar peningkatan nilai Defect Detection Rate (DDR) dari pengujian Boundary Value Analysis (BVA) dan Equivalence Partitioning (EP) dibandingkan dengan pengujian kasual (ad-hoc) pada Controller MVC Sistem Manajemen Gudang Hasil Tani?

**Variable → Component Mapping:**

| Variabel | Tipe | Komponen Sistem | Cara Manipulasi/Pengukuran |
|---|---|---|---|
| Pendekatan Uji | IV | Dokumen Uji & Form Input (View) | Mengubah instruksi input penguji (bebas/ad-hoc vs terpandu matriks BVA/EP) |
| Efektivitas (DDR) | DV | Response Output & Error Log (Controller) | Menghitung rasio jumlah output Gagal/Error valid terhadap total skenario uji |
| Lingkungan Sistem | CV | Database MySQL & Source Code PHP | Eksekusi script TRUNCATE database sebelum berganti fase uji & membekukan kode |

**4 Prinsip Desain:**
- [x] Traceability — Setiap komponen bisa ditelusuri ke variabel
- [x] Variable Isolation — IV bisa diubah tanpa mengubah CV
- [x] Measurement Integration — Pengukuran DV built-in
- [x] Reproducibility — Setup bisa direkonstruksi

**Experimental Setup:**
- **Input data:** Data *dummy* berat barang organik (kg) dan kuantitas pengeluaran (FEFO)
- **Parameter:** Nilai normal, nilai minus, nilai nol, huruf, dan nilai ekstrim batas atas/bawah
- **Output format:** Status akhir pengujian (Lulus/Gagal) dan persentase metrik DDR
---
## Latihan 1 — Variable-to-Component Mapping

Gunakan RQ dan variabel dari WS-05. Petakan ke komponen sistem.

**RQ:** Seberapa besar peningkatan nilai Defect Detection Rate (DDR) dari pengujian Boundary Value Analysis (BVA) dan Equivalence Partitioning (EP) dibandingkan dengan pengujian kasual (ad-hoc) pada Controller MVC Sistem Manajemen Gudang Hasil Tani?

| Variabel | Tipe | Komponen Sistem | Cara Manipulasi / Pengukuran |
|----------|------|-----------------|---------------------------|
| *Pendekatan Uji* | *IV* | *Mekanisme Input Skenario* | *Ganti metode (Ad-hoc ↔ Matriks BVA/EP)* |
| *Efektivitas Deteksi* | DV | *Logika Validasi Controller* | *Pencatatan status temuan bug fungsional untuk rumus DDR* |
| *Lingkungan Sistem* | CV | *Database & State Aplikasi* | *Jalankan query reset database setiap iterasi metode uji* |

**Apakah semua variabel bisa di-map?** [x] Ya / [ ] Tidak
> **Jika tidak, komponen apa yang perlu ditambahkan?** Rantai arsitektur sudah tertutup. IV dimanipulasi lewat input, sistem (CV) memproses secara konstan, dan DV diukur langsung dari respons sistem.

---

## Latihan 2 — 4 Prinsip Desain

Evaluasi desain sistem terhadap 4 prinsip.

| Prinsip | Status | Bukti / Penjelasan |
|---------|--------|-------------------|
| Traceability | ✅ | Form input mewakili IV (tempat manipulasi variabel), Controller mewakili CV (objek konstan), dan output layar mewakili DV (pengukuran bug). |
| Modularity | ✅ | Metode pengujian (IV) diisolasi murni di level *tester* dan dokumen uji, tanpa perlu mengubah satu baris pun kode pada sistem (CV). |
| Controllability | ✅ | Kondisi awal sistem bisa dikontrol mutlak melalui skrip pembersihan *database* sehingga tidak ada sisa *state* dari pengujian sebelumnya. |
| Measurability | ✅ | Sistem memberikan respons keluaran (sukses/pesan error/crash) yang dapat langsung dicatat sebagai basis perhitungan kuantitatif DDR. |

**Prinsip mana yang paling sulit dipenuhi?** Measurability (Keterukuran langsung dari sistem).
**Strategi untuk mengatasinya:**
> Terkadang aplikasi PHP native yang *crash* hanya memunculkan "Blank White Screen" sehingga sulit dibedakan antara *bug* fungsional atau *server timeout*. Strateginya adalah menyalakan fitur `error_reporting(E_ALL)` pada PHP selama masa eksperimen agar setiap kegagalan logika di *Controller* langsung tercetak jelas di layar dan mudah diklasifikasikan untuk metrik DDR.

---

## Latihan 3 — Ablation Study Planning

Jika sistem memiliki 3 komponen utama, rencanakan ablation study. 
*(Konteks SQA: Komponen desain intervensi uji pada arsitektur MVC).*

> **Panduan jumlah kondisi:** Untuk 3 komponen (A, B, C), kondisi minimal yang direkomendasikan:
> Full + (-A) + (-B) + (-C) = **4 kondisi dasar**. Jika waktu memungkinkan, tambahkan kombinasi ganda: (-A,-B), (-A,-C), (-B,-C) = **7 kondisi**. Sesuaikan dengan *computational cost* dan tenggat waktu penelitian.

| Kondisi | Komponen A | Komponen B | Komponen C | Hasil yang Diharapkan |
|---------|-----------|-----------|-----------|----------------------|
| Full | ✅ *Skenario BVA (Batas Nilai)* | ✅ *Skenario EP (Partisi Tipe Data)* | ✅ *Bypass UI Validation (Hit API/Controller Langsung)* | *DDR maksimal (Controller teruji penuh)* |
| – A | ❌ *(Tanpa BVA)* | ✅ | ✅ | *Bug matematis/logika FEFO lolos* |
| – B | ✅ | ❌ *(Tanpa EP)* | ✅ | *Bug input karakter/simbol aneh lolos* |
| – C | ✅ | ✅ | ❌ *(Tanpa Bypass UI, tertahan validasi HTML5 front-end)* | *Nilai DDR palsu (Controller seolah kebal, padahal bug hanya terblokir di View)* |

**Komponen mana yang diprediksi paling berkontribusi?** Komponen A (Skenario BVA) dan Komponen C (Bypass UI Validation).
**Mengapa?**
> Dalam logika operasional pergudangan (FEFO dan Inbound), cacat sistem paling fatal biasanya bersembunyi di perhitungan matematis pada nilai batas ekstrem (seperti kuantitas pengeluaran lebih besar 1 angka dari stok nyata, atau input berat bernilai minus). BVA dirancang khusus untuk memukul titik ini. Komponen C juga krusial karena menguji *Controller* tidak akan valid jika *tester* masih terhalang oleh restriksi `<input type="number">` di sisi antarmuka (*View*).

---

## Refleksi

> Apa risiko jika sistem dibangun seperti produk (monolitik, fitur lengkap) lalu baru dilakukan eksperimen? Mengapa arsitektur modular penting untuk riset?

**Jawaban:**
> Jika eksperimen pengujian langsung dilakukan pada sistem monolitik tanpa isolasi arsitektur, akan terjadi "bias perancu" (*confounding bias*). Ketika sebuah input *error* terjadi, peneliti tidak akan tahu pasti apakah sistem gagal karena kode di *Front-End (View)*, cacat algoritma di *Back-End (Controller)*, atau gagalnya koneksi di level *Database (Model)*.
>
> Arsitektur yang modular (seperti pemisahan ketat pada konsep MVC) sangat esensial dalam riset rekayasa perangkat lunak. Hal ini memungkinkan peneliti untuk mengisolasi variabel intervensinya. Misalnya, dengan arsitektur modular, kita bisa menonaktifkan (*bypass*) lapisan *View*, lalu menembak input pengujian langsung ke dalam logika *Controller*. Hasilnya, data metrik (DDR) yang diperoleh murni merepresentasikan keandalan otak pemrosesan data, bukan sekadar ketahanan tampilan visualnya.