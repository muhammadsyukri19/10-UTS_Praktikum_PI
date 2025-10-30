# 🧠 Multi-Dataset Information Retrieval System (CLI-Based)

**Praktikum Penelusuran Informasi — Universitas Syiah Kuala (2025)**
Sistem pencarian teks berbasis _Command-Line Interface (CLI)_ yang menggabungkan lima dataset berbahasa Indonesia dan menerapkan _Vector Space Model (VSM)_ serta _Cosine Similarity_ untuk melakukan ranking hasil pencarian secara akurat.

## 📜 Deskripsi Singkat

Proyek ini mengimplementasikan sistem **Information Retrieval (IR)** yang mampu mencari dokumen dari berbagai sumber dataset.
Sistem memproses teks mentah menggunakan _Sastrawi_ untuk _case folding_, _stopword removal_, dan _stemming_, lalu membangun _index Whoosh_ dan _Bag-of-Words (BoW)_ menggunakan _CountVectorizer_.

Semua proses dijalankan dari terminal (CLI), mulai dari _preprocessing dataset_, _indexing dokumen_, hingga _pencarian query_ dengan hasil _Top-5 dokumen paling relevan_.

## 🧩 Fitur Utama

✅ Preprocessing teks otomatis (case folding, stopword removal, stemming)
✅ Indexing multi-dataset dengan _Whoosh_
✅ Representasi dokumen berbasis _Vector Space Model (BoW)_
✅ Pencarian interaktif lewat CLI
✅ Ranking hasil dengan _Cosine Similarity_
✅ Modular: setiap tahap (preprocessing, indexing, search) terpisah dalam file berbeda

## 🧠 Arsitektur Sistem

Dataset Mentah (.csv)
│
▼
┌──────────────────────┐
│ preprocessing.py │
│ - Case Folding │
│ - Stopword Removal │
│ - Stemming (Sastrawi)│
└─────────┬────────────┘
│
▼
datasets_clean/\*.csv
(judul, konten, konten_bersih)
│
▼
┌──────────────────────────────┐
│ main.py │
│ - Whoosh Indexing │
│ - CountVectorizer (BoW) │
│ - Cosine Similarity Ranking │
└──────────┬───────────────────┘
│
▼
CLI Input → Query Processing → Ranking → Output Top-5

## 📁 Struktur Direktori

UTS_LAB_PI/
│
├── datasets/ # Dataset mentah (etd_usk, etd_ugm, kompas, tempo, mojok)
├── datasets_clean/ # Hasil preprocessing (judul, konten, konten_bersih)
├── datasets_format/ # Perbaikan CSV tidak standar (opsional)
├── whoosh_index/ # Hasil index Whoosh
│
├── preprocessing.py # Modul preprocessing (Sastrawi)
├── main.py # Sistem IR utama (Index + VSM + CLI)
├── clean_dataset.ipynb # Notebook pembersihan CSV (opsional)
├── requirements.txt # Dependensi Python
└── README.md

## ⚙️ Cara Menjalankan

### 1. Persiapan Lingkungan

```bash
python -m venv venv
venv\Scripts\activate          # Windows
pip install -r requirements.txt
```

### 2. Preprocessing Dataset

Jalankan skrip preprocessing untuk membersihkan teks mentah:

```bash
python preprocessing.py
```

Output tersimpan di folder `datasets_clean/` dengan nama file:

```
etd_usk_clean.csv
etd_ugm_clean.csv
kompas_clean.csv
tempo_clean.csv
mojok_clean.csv
```

### 3. Menjalankan Sistem Pencarian

```bash
python main.py
```

Menu utama akan muncul:

```
========================================
=== INFORMATION RETRIEVAL SYSTEM ===
Status: Perlu Indexing
========================================
[1] Load & Index Dataset
[2] Search Query
[3] Exit
========================================
```

Langkah:
1️⃣ Pilih `[1]` untuk melakukan indexing & membuat VSM
2️⃣ Setelah selesai, pilih `[2]` dan ketik query (contoh: `ekonomi digital Indonesia`)
3️⃣ Sistem menampilkan hasil _Top-5 dokumen_ berdasarkan skor kemiripan tertinggi

## 📊 Contoh Hasil Pencarian

Masukkan Query Pencarian Anda: kebijakan pemerintah menanggulangi inflasi

Ditemukan 214 dokumen relevan (dari 49484 total).
Ranking selesai dalam 0.1425 detik.

=== TOP 5 HASIL PENCARIAN (Cosine Similarity) ===
[1] Skor: 0.8877 | Judul: Pemerintah Kendalikan Inflasi Lewat Subsidi Energi (KOMPAS)
[2] Skor: 0.8452 | Judul: Langkah BI Menekan Inflasi Pasca Kenaikan BBM (TEMPO)
[3] Skor: 0.8139 | Judul: Analisis Kebijakan Fiskal Terhadap Inflasi di Indonesia (ETD_UGM)
[4] Skor: 0.7912 | Judul: Evaluasi Program Stabilisasi Harga Pangan (ETD_USK)
[5] Skor: 0.7591 | Judul: Dampak Inflasi pada Sektor Konsumsi Nasional (MOJOK)
==================================================
Query Bersih: kebijakan pemerintah tanggul inflasi
==================================================

## 🧮 Dataset yang Digunakan

| Sumber    | Deskripsi                      | Jumlah Dokumen     |
| --------- | ------------------------------ | ------------------ |
| etd_usk   | Skripsi/Tesis USK              | 10.000             |
| etd_ugm   | Skripsi/Tesis UGM              | 10.000             |
| kompas    | Artikel berita nasional        | 10.000             |
| tempo     | Artikel berita ekonomi/politik | 10.000             |
| mojok     | Artikel opini & esai ringan    | 9.484              |
| **Total** | —                              | **49.484 dokumen** |

## 🧰 Teknologi yang Digunakan

| Library          | Fungsi                                             |
| ---------------- | -------------------------------------------------- |
| **pandas**       | Membaca dan memproses CSV besar                    |
| **Whoosh**       | Membuat dan menyimpan index teks                   |
| **scikit-learn** | _CountVectorizer_ & _Cosine Similarity_            |
| **Sastrawi**     | _Stopword removal_ dan _stemming_ Bahasa Indonesia |
| **regex (re)**   | Pembersihan karakter non-standar                   |

## 🔍 Pipeline Sistem

1. **Formatting Dataset** → memperbaiki CSV tidak standar (`clean_dataset.ipynb`)
2. **Preprocessing** → normalisasi teks dengan _Sastrawi_ (`preprocessing.py`)
3. **Indexing + VSM** → _Whoosh_ + _CountVectorizer_ (`main.py`)
4. **Query Search** → _Cosine Similarity_ → urutkan hasil
5. **Output** → tampil di terminal (judul, sumber, skor)

## 📈 Hasil & Evaluasi

- Total dokumen yang berhasil di-index: **49.484**
- Preprocessing berhasil menghasilkan file bersih bebas karakter anomali.
- Waktu indexing ±10–15 menit untuk seluruh dataset.
- Pencarian rata-rata <1 detik setelah index terbentuk.
- Akurasi hasil tinggi untuk query berbahasa Indonesia berkat stemming & stopword removal.

## 🚀 Pengembangan Selanjutnya

💡 Rencana fitur lanjutan:

- Menggunakan **TF-IDF weighting** untuk meningkatkan relevansi skor.
- Menambahkan **text snippet preview** untuk hasil pencarian.
- Implementasi **interface berbasis web (Flask/Next.js)**.
- Analisis performa multi-threading untuk indexing.

## 👥 Tim Pengembang

| Nama                         | NIM | Peran |
| ---------------------------- | --- | ----- |
| (Isi sesuai kelompok kalian) |     |       |

## 🏫 Lisensi & Atribusi

Proyek ini dikembangkan untuk kebutuhan akademik mata kuliah **Praktikum Penelusuran Informasi** di **Departemen Informatika, FMIPA, Universitas Syiah Kuala (USK)**.
