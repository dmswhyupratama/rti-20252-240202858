# WS-03: Literature Mapping & Gap

> **Bab 3 — Literature Review, Research Gap & Baseline**

---

## Ringkasan Materi

### Literature Review = Positioning, Bukan Ringkasan

Literature review bukan merangkum paper satu per satu. Pendekatan yang benar adalah **concept-centric** — organisasi berdasarkan tema, metode, atau variabel. Tujuan: menemukan **pola, kontradiksi, dan gap**.

**Perbandingan pendekatan Author-centric vs Concept-centric:**

| Aspek | Author-centric (Hindari) | Concept-centric (Gunakan) |
|-------|--------------------------|---------------------------|
| Struktur | Per penulis/paper ("Rahman et al. menyatakan...") | Per konsep/metode ("Pendekatan berbasis transformer") |
| Tujuan | Ringkasan isi paper | Perbandingan metode & identifikasi gap |
| Contoh paragraph | "Rahman (2023) pakai CNN. Lee (2022) pakai LSTM. Zhang (2021) pakai RF." | "Tiga pendekatan dominan: CNN digunakan oleh 4 paper untuk representasi fitur visual; LSTM untuk data sekuensial; RF sebagai baseline klasik." |
| Hasil akhir | Daftar paper | Peta pengetahuan + gap yang teridentifikasi |

### Empat Jenis Research Gap

| Jenis Gap | Deskripsi | Contoh |
|-----------|----------|--------|
| **Performance Gap** | Performa belum memadai | Akurasi deteksi hanya 78% pada kasus tertentu |
| **Method Gap** | Pendekatan belum diterapkan | Belum ada yang pakai transformer untuk task ini |
| **Data Gap** | Dataset terbatas/tidak representatif | Semua studi pakai dataset sintetis |
| **Context Gap** | Belum diuji pada konteks berbeda | Belum ada evaluasi di negara berkembang |

Gap terkuat = kombinasi 2+ jenis.

### Systematic Search Strategy

1. **Database utama**: IEEE Xplore, ACM DL, Scopus
   - Akses IEEE/ACM melalui jaringan kampus atau VPN institusi
   - Alternatif bebas biaya: Google Scholar, ResearchGate ([researchgate.net](https://www.researchgate.net)), arXiv ([arxiv.org](https://arxiv.org))
2. **Boolean query** yang terdokumentasi eksplisit
   - Contoh: `("anomaly detection" OR "intrusion detection") AND ("deep learning" OR "neural network") NOT ("medical imaging")`
   - Gunakan tanda kutip untuk frasa eksak; AND/OR/NOT mengontrol scope
3. **Snowballing** — dua arah:
   - **Backward snowballing**: buka daftar referensi di paper kunci → telusuri paper yang dikutip
   - **Forward snowballing**: di Google Scholar, klik "Cited by" di bawah paper kunci → temukan paper yang mengutipnya
   - Ulangi 1–2 tingkat untuk membangun cakupan komprehensif
4. Klaim "belum ada penelitian" harus didukung **bukti pencarian**

### Baseline Selection — 3 Kriteria

| Kriteria | Pertanyaan |
|----------|-----------|
| **Relevan** | Apakah menyelesaikan masalah yang sama? |
| **Representatif** | Apakah mewakili common practice? |
| **State-of-the-Art** | Apakah terbaru/terbaik? |

Membandingkan deep learning 2024 dengan decision tree sederhana tanpa justifikasi = **straw man comparison** (perbandingan tidak jujur).

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan baca literatur | Mencari solusi yang sudah ada | Memahami apa yang belum terjawab |
| Cara membaca paper | Tutorial, how-to | Metode, limitasi, gap |
| Baseline | Framework terpopuler | State-of-the-art yang rigorous |
| Dokumentasi pencarian | Tidak diperlukan | Wajib (reproducible) |

### Istilah Penting

- **Concept-centric** — Organisasi literatur berdasarkan konsep/metode, bukan per penulis
- **Snowballing** — Backward (telusuri referensi) + Forward (cari yang mengutip paper kunci)
- **Research Position** — Pernyataan eksplisit posisi riset terhadap studi sebelumnya
- **Straw man comparison** — Memilih baseline lemah agar metode sendiri terlihat lebih baik

---

## Template A.3 — Literature Mapping & Gap Identification

```
LITERATURE MAPPING

Topik      : ____________________
Database   : ____________________
Query      : ____________________
Tahun      : ____________________
Hasil awal : ____ paper → Screening → ____ paper final

Literature Matrix (concept-centric):

| Study | Tahun | Method | Data | Result | Limitation |
|-------|-------|--------|------|--------|------------|
|       |       |        |      |        |            |

Pola yang ditemukan:
  Metode dominan     : ____________________
  Dataset umum       : ____________________
  Limitasi berulang  : ____________________

GAP IDENTIFICATION

Gap 1: [Jenis: performance / method / data / context]
  Deskripsi    : ____________________
  Bukti        : ____________________
  Signifikansi : ____________________

Gap 2: [Jenis: ____]
  Deskripsi    : ____________________
  Bukti        : ____________________
  Signifikansi : ____________________

Baseline Selection:
| Baseline | Relevansi | Representatif | Source |
|----------|-----------|---------------|--------|
|          |           |               |        |
```

---

## Latihan 1 — Concept-Centric Literature Table

Gunakan topik riset dari WS-02. Cari minimal 5 paper relevan menggunakan Google Scholar atau database lain.

**Topik riset:** ________________________________________
**Query pencarian:** ____________________________________
**Database:** ___________________________________________

| # | Study | Tahun | Method | Dataset | Result | Limitasi |
|---|-------|-------|--------|---------|--------|----------|
| 1 | *Ekasakti & Pratomo* | *2025* | *Black Box (Manual vs Otomatis)* | *Aplikasi Propertio.id* | *Pengujian manual unggul di test coverage (100%), otomasi lebih cepat 84,54%[cite: 13].* | *Otomasi butuh resource tinggi, pengujian manual kurang efisien waktu[cite: 13].* |
| 2 | *Bong et al.* | *2024* | *Black Box (BVA & EP)* | *Sistem Informasi Stok PT ABC* | *Sistem berfungsi baik namun ada beberapa anomali yang butuh perbaikan[cite: 14].* | *Pengujian murni manual tanpa tools, tidak ada akses ke source code[cite: 14].* |
| 3 | *Uminingsih et al.* | *2022* | *Black Box (EP)* | *Sistem Informasi Perpustakaan* | *Tingkat validitas 75%, ditemukan error (bug syntax) pada form pengembalian[cite: 15].* | *Ruang lingkup sangat terbatas hanya menguji 3 form utama[cite: 15].* |
| 4 | *Suharyono et al.* | *2024* | *Black Box (BVA & EP)* | *Aplikasi E-Ticketing SIADITA* | *Efektivitas mencapai 91,66% (12 skenario invalid), tidak ada batasan karakter pada form[cite: 16].* | *Pengujian hanya dari sisi UI/fungsi tanpa melihat source code sama sekali[cite: 16].* |
| 5 | *Nugraha et al.* | *2025* | *Black Box (EP)* | *Sistem Inventori CV Cahaya Baru* | *18 test case lulus, 1 gagal pada fungsi invoice/detail penjualan[cite: 17].* | *Pengujian hanya difokuskan secara spesifik pada halaman penjualan dan pembelian saja[cite: 17].* |

**Pola yang terlihat — Metode dominan:** Black Box Testing menggunakan pendekatan manual dengan teknik Equivalence Partitioning dan Boundary Value Analysis[cite: 14, 16, 17].
**Limitasi yang berulang:** Ketiadaan akses ke *source code* sehingga pengujian dilakukan terbatas dari sisi *end-user*[cite: 14, 16], proses yang dilakukan murni secara manual[cite: 14, 16], serta ruang lingkup pengujian yang seringkali dibatasi hanya pada sebagian kecil modul/form[cite: 15, 17].

---

## Latihan 2 — Gap Identification

Berdasarkan tabel di Latihan 1, identifikasi gap.

| Jenis Gap | Ditemukan? | Gap Statement |
|-----------|-----------|---------------|
| Performance Gap | [x] Ya / [ ] Tidak | *Pengujian fungsional manual terbukti memakan waktu eksekusi yang kurang efisien dibandingkan otomatis[cite: 13].* |
| Method Gap | [x] Ya / [ ] Tidak | *Mayoritas pengujian BVA dan EP masih dieksekusi secara manual tanpa bantuan otomasi, padahal skenario ekstrim rentan terhadap human-error[cite: 14, 16].* |
| Data Gap | [x] Ya / [ ] Tidak | *Pengujian sering dibatasi hanya pada modul tertentu (misal: hanya 3 form dasar), sehingga tidak merepresentasikan stabilitas sistem secara keseluruhan[cite: 15, 17].* |
| Context Gap | [ ] Ya / [x] Tidak |  |

**Gap utama yang dipilih:** Method Gap & Data Gap (Pengujian manual yang berulang pada form yang terbatas).
**Mengapa gap ini penting (bukan sekadar "belum ada yang meneliti")?**
> Kesalahan input pada form yang krusial (seperti *inbound/outbound* pada inventori gudang) dapat berdampak langsung pada kerugian finansial perusahaan. Mengandalkan pengujian manual yang terbatas hanya pada sebagian kecil modul[cite: 15, 17] berisiko meloloskan *bug* kritikal, sehingga diperlukan pendekatan pengujian yang lebih menyeluruh pada *edge cases* (kasus batas ekstrim).

---

## Latihan 3 — Baseline Selection

Pilih 2 baseline dari literatur yang sudah dibaca.

| # | Baseline | Mengapa Relevan | Mengapa Representatif | Apakah SOTA? | Sumber |
|---|----------|----------------|----------------------|-------------|--------|
| 1 | *Black-Box Manual (EP & BVA)* | *Tujuan sama: mencari cacat perangkat lunak pada form sistem informasi[cite: 14].* | *Pendekatan ini digunakan oleh mayoritas paper pengujian fungsional di Indonesia[cite: 14, 15, 16, 17].* | *Bukan SOTA, ini adalah common practice standar industri.* | *Bong et al., 2024[cite: 14]* |
| 2 | *Black-Box Otomatis (Selenium/Katalon)* | *Menawarkan solusi nyata atas kelemahan efisiensi waktu pada pengujian manual[cite: 13].* | *Mewakili standar pengujian modern untuk mengeksekusi skenario repetitif secara konsisten.* | *Ya, mewakili SOTA dalam otomasi UI testing.* | *Ekasakti & Pratomo, 2025[cite: 13]* |

**Apakah pemilihan baseline ini bisa dianggap straw man?** [ ] Ya / [x] Tidak
> Justifikasi: Pemilihan *baseline* ini sangat realistis dan adil karena mewakili dua spektrum utama dalam industri *Quality Assurance* saat ini. Baseline manual merepresentasikan cakupan (*coverage*) pengujian yang tinggi[cite: 13], sedangkan baseline otomatis merepresentasikan efisiensi waktu[cite: 13]. Membandingkan atau mengembangkan metode usulan dari kedua *baseline* ini membuktikan pemahaman yang utuh terhadap ekosistem pengujian perangkat lunak.

---

## Refleksi

> Apa perbedaan antara "belum ada yang meneliti ini" (klaim tanpa bukti) dengan research gap yang valid? Bagaimana cara membuktikan bahwa sebuah gap benar-benar ada?

**Jawaban:**
> Klaim "belum ada yang meneliti" hanyalah sebuah asumsi buta yang sering muncul karena peneliti kurang membaca literatur. Sebaliknya, *research gap* yang valid didasarkan pada penelusuran sistematis (seperti *Literature Matrix*), di mana kita benar-benar menemukan masalah, kelemahan, atau keterbatasan (*limitation*) yang secara eksplisit diakui oleh para peneliti sebelumnya di dalam *paper* mereka (misalnya, keterbatasan pengujian yang hanya pada modul tertentu[cite: 15, 17]). Cara membuktikannya adalah dengan mengompilasi studi-studi terdahulu yang relevan, menyoroti pola kelemahan metode atau data yang mereka gunakan secara berulang, lalu merumuskannya menjadi celah yang logis untuk disempurnakan.