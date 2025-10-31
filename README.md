# 🧠 Implementasi Multi-Dataset Information Retrieval System (CLI-Based) Menggunakan Whoosh dan Vector Space Model

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![Whoosh](https://img.shields.io/badge/Indexing-Whoosh-informational)
![VSM](<https://img.shields.io/badge/Model-VSM%20(BoW)-orange>)
![Cosine](https://img.shields.io/badge/Similarity-Cosine-green)
![Sastrawi](https://img.shields.io/badge/NLP-Sastrawi-lightgrey)

Sistem IR berbasis **Command-Line Interface (CLI)** yang menggabungkan lima dataset Indonesia dan menerapkan _Vector Space Model (VSM)_ serta _Cosine Similarity_ untuk melakukan ranking hasil pencarian secara akurat.

---

## 📜 Deskripsi Singkat

Proyek ini mengimplementasikan sistem **Information Retrieval (IR)** yang mampu mencari dokumen dari berbagai sumber dataset.
Sistem memproses teks mentah menggunakan _Sastrawi_ untuk _case folding_, _stopword removal_, dan _stemming_, lalu membangun _Whoosh Index_ dan _Bag-of-Words (BoW)_ dengan _CountVectorizer_.

Semua proses dijalankan dari terminal (CLI), mulai dari preprocessing dataset, indexing dokumen, hingga pencarian query dengan hasil _Top-5 dokumen paling relevan_.

---

## ✨ Fitur Utama

- ✅ Preprocessing otomatis (case folding, stopword removal, stemming)
- ✅ Indexing multi-dataset dengan _Whoosh_
- ✅ Representasi dokumen berbasis _Vector Space Model (BoW)_
- ✅ Pencarian interaktif lewat CLI
- ✅ Ranking hasil dengan _Cosine Similarity_
- ✅ Modular — tiap tahap (preprocessing, indexing, search) dipisah dalam file sendiri

---

## 🧠 Arsitektur Sistem

```mermaid
flowchart LR
  A(["Dataset Mentah (5 CSV)"]) --> B(["Pra-Pemrosesan\nclean_dataset.ipynb"])
  B --> C(["datasets_format/*.csv"])
  C --> D(["Preprocessing\npreprocessing.py"])
  D --> E(["datasets_clean/*_clean.csv"])
  E --> F(["Indexing\nWhoosh"])
  F --> G(["whoosh_index/"])
  G --> H(["VSM\nCountVectorizer"])
  H --> I(["CLI Query"])
  I --> J(["preprocess_query()"])
  J --> K(["Cosine Similarity"])
  K --> L(["Top-5 Ranking\ndi Terminal"])
```

**Versi ASCII (fallback GitHub tanpa mermaid)**

```
Dataset Mentah → Pra-Pemrosesan (clean_dataset.ipynb) → datasets_format/*.csv
     ↓
Preprocessing (case folding, stopword, stemming) → datasets_clean/*_clean.csv
     ↓
Indexing (Whoosh: judul, konten, konten_bersih) → whoosh_index/
     ↓
VSM (CountVectorizer → Term-Document Matrix)
     ↓
CLI Query → preprocess_query() → Cosine Similarity → Top-5 Result
```

---

## 📁 Struktur Direktori

```text
UTS_LAB_PI/
├─ datasets/            # Dataset mentah (etd_usk, etd_ugm, kompas, tempo, mojok)
├─ datasets_format/     # CSV hasil perapian format (opsional)
├─ datasets_clean/      # Hasil preprocessing (judul, konten, konten_bersih)
├─ whoosh_index/        # Index hasil Whoosh
│
├─ preprocessing.py     # Modul preprocessing (Sastrawi)
├─ main.py              # CLI utama (Index + VSM + Search)
├─ clean_dataset.ipynb  # Notebook perbaikan CSV
├─ requirements.txt     # Dependensi Python
└─ README.md
```

---

## ⚙️ Cara Menjalankan

### 1️⃣ Setup Lingkungan

```bash
python -m venv venv
venv\Scripts\activate        # Windows
# source venv/bin/activate   # Linux/macOS

pip install -r requirements.txt
```

### 2️⃣ Preprocessing Dataset

```bash
python preprocessing.py
```

Output akan tersimpan di folder:

```
datasets_clean/*.csv
```

### 3️⃣ Menjalankan Sistem Pencarian

```bash
python main.py
```

Akan muncul menu:

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

1. Pilih `[1]` untuk indexing & membangun VSM
2. Setelah selesai, pilih `[2]` dan masukkan query (contoh: _ekonomi digital Indonesia_)
3. Sistem akan menampilkan hasil _Top-5 dokumen paling relevan_

---

## 🔎 Contoh Output

```text
Masukkan Query Pencarian Anda: politik identitas dan polarisasi media

Ditemukan 8529 dokumen relevan (dari 47432 total).
Ranking selesai dalam 0.4346 detik.

=== TOP 5 HASIL PENCARIAN (Cosine Similarity) ===
[1] Skor: 0.4356 | Judul: PERAN MEDIA SOSIAL SEBAGAI SARANA SOSIALISASI POLITIK ... (ETD_USK)
[2] Skor: 0.4348 | Judul: Analisis Kemitraan Pemerintah Swasta Dalam Pengelolaan Sampah ... (ETD_UGM)
[3] Skor: 0.3350 | Judul: PENGARUH IKLAN POLITIK DI MEDIA SOSIAL ... (ETD_USK)
[4] Skor: 0.3291 | Judul: Strategi Kampanye Partai ... (ETD_UGM)
[5] Skor: 0.3267 | Judul: PENGEMBANGAN MEDIA TELUR MISTERI ... (ETD_USK)
==================================================
Query Bersih: kebijakan pemerintah tanggul inflasi
```

---

## 🧮 Dataset yang Digunakan

| Sumber    | Deskripsi                   |     Jumlah |
| --------- | --------------------------- | ---------: |
| etd_usk   | Skripsi/Tesis USK           |     10.000 |
| etd_ugm   | Skripsi/Tesis UGM           |      7.948 |
| kompas    | Artikel berita nasional     |     10.000 |
| tempo     | Artikel berita ekonomi      |     10.000 |
| mojok     | Artikel opini & esai ringan |      9.484 |
| **Total** | —                           | **47.432** |

---

## 🧰 Teknologi

| Library          | Fungsi                                       |
| ---------------- | -------------------------------------------- |
| **pandas**       | Membaca & menggabungkan CSV besar            |
| **Whoosh**       | Membuat & menyimpan index teks               |
| **scikit-learn** | CountVectorizer + Cosine Similarity          |
| **Sastrawi**     | Stopword removal & stemming Bahasa Indonesia |
| **regex (re)**   | Membersihkan karakter non-standar            |

---

## 🔍 Pipeline Sistem

1. **Formatting Dataset** → memperbaiki CSV tidak standar (`clean_dataset.ipynb`)
2. **Preprocessing** → normalisasi teks (`preprocessing.py`)
3. **Indexing + VSM** → Whoosh + CountVectorizer (`main.py`)
4. **Query Search** → Cosine Similarity → Ranking
5. **Output** → hasil tampil di terminal (judul, sumber, skor)

---

## 📈 Hasil & Evaluasi

- Total dokumen berhasil di-index: **≈47K+**
- Preprocessing berhasil menghasilkan teks bersih & seragam
- Waktu indexing: ±10–15 menit untuk seluruh dataset
- Waktu pencarian: <1 detik setelah index siap
- Akurasi tinggi berkat stemming & stopword removal Bahasa Indonesia

---

## 🚀 Rencana Pengembangan

- Gunakan **TF-IDF weighting** agar relevansi skor meningkat
- Tambahkan **preview snippet** hasil pencarian
- Buat antarmuka **berbasis web (Flask/Next.js)**
- Optimasi indexing via **multi-threading**

---

## 👥 Tim Pengembang

| Nama             | NIM            |
| ---------------- | -------------- |
| Muhammad Syukri  | 23081017010060 |
| Halim Elsa Putra | 23081017010062 |
| M Milan Ramadhan | 23081017010064 |

---

## 🏫 Lisensi & Atribusi

Proyek ini dikembangkan untuk mata kuliah **Praktikum Penelusuran Informasi – Universitas Syiah Kuala (2025)**.
Distribusi hanya untuk keperluan akademik.
