# Analisis Komparatif Model Klasifikasi Teks Deep Learning pada Dataset DBpedia

Proyek ini melakukan evaluasi dan analisis perbandingan terhadap tujuh arsitektur _deep learning_ yang berbeda untuk tugas klasifikasi teks. Tujuannya adalah untuk memahami kekuatan dan kelemahan setiap model pada dataset berskala besar, dengan fokus khusus pada kinerja model usulan, **VDCNN (Very Deep Convolutional Neural Network)**.

---

## 📋 Daftar Isi

1.  [Deskripsi Proyek](#-deskripsi-proyek)
2.  [Model yang Diimplementasikan](#-model-yang-diimplementasikan)
3.  [Dataset](#-dataset)
4.  [Instalasi & Pengaturan](#-instalasi--pengaturan)
5.  [Cara Menjalankan](#-cara-menjalankan)
6.  [Hasil Akhir](#-hasil-akhir)
7.  [Analisis & Kesimpulan Utama](#-analisis--kesimpulan-utama)

---

## 📝 Deskripsi Proyek

Tujuan utama dari proyek ini adalah:
- Mengimplementasikan berbagai model klasifikasi teks standar, mulai dari baseline sederhana hingga arsitektur RNN dan CNN yang kompleks.
- Mengevaluasi model usulan, **VDCNN**, sebuah arsitektur berbasis karakter yang sangat dalam (29-lapisan) dengan _residual connections_.
- Menganalisis secara kritis mengapa beberapa arsitektur berkinerja lebih baik daripada yang lain pada tugas klasifikasi topik.
- Mengoptimalkan pipeline pemrosesan data untuk menangani dataset besar secara efisien dalam lingkungan dengan memori terbatas.

---

## 🤖 Model yang Diimplementasikan

Proyek ini membandingkan 7 arsitektur berbeda:

1.  **Dense (Bag-of-Words)**: Baseline sederhana yang mengabaikan urutan kata.
2.  **WordCNN**: Model CNN yang efektif menangkap pola frasa kunci (n-gram).
3.  **WordRNN**: Model RNN (Bi-LSTM) yang memahami konteks dan urutan kalimat.
4.  **AttentionRNN**: Peningkatan dari WordRNN dengan mekanisme *attention* untuk fokus pada kata-kata penting.
5.  **RCNN**: Model hibrida yang menggabungkan kekuatan RNN dan CNN.
6.  **CharCNN**: Model CNN pembanding yang beroperasi pada level karakter.
7.  **VDCNN (Usulan)**: Model CNN yang sangat dalam (29-lapis) pada level karakter.

---

## 📊 Dataset

Eksperimen ini menggunakan **DBpedia Ontology Dataset**.
- **Jumlah Sampel:** ~560.000 (train), ~70.000 (test)
- **Jumlah Kelas:** 14 (Company, EducationalInstitution, Artist, Athlete, dll.)
- **Tugas:** Klasifikasi topik berdasarkan konten teks dari artikel Wikipedia.

---

## 🛠️ Instalasi & Pengaturan

Proyek ini dirancang untuk berjalan di lingkungan **Google Colaboratory**.

1.  **Kebutuhan Library:**
    Semua pustaka yang diperlukan akan diinstal secara otomatis oleh `Sel 1` di dalam notebook. Pustaka utamanya adalah:
    - `tensorflow`
    - `pandas`
    - `scikit-learn`
    - `nltk`

2.  **Dataset:**
    - Unduh dataset DBpedia (`train.csv` dan `test.csv`).
    - Buat sebuah folder bernama `dbpedia_csv` di direktori root Google Drive Anda.
    - Unggah kedua file `.csv` tersebut ke dalam folder `dbpedia_csv`.

---

## ▶️ Cara Menjalankan

Notebook ini dirancang untuk dijalankan secara berurutan.

1.  **Sel 1: Pengaturan Awal:** Jalankan untuk menginstal dan mengimpor semua pustaka.
2.  **Sel 2: Koneksi Google Drive:** Jalankan dan berikan otorisasi untuk mengakses dataset.
3.  **Sel 3: Definisi Utilitas Data:** Sel ini mendefinisikan semua fungsi pemrosesan data yang telah dioptimalkan.
4.  **Sel 4: Definisi Model:** Sel ini mendefinisikan semua arsitektur model.
5.  **Sel 5: Setup Pipeline Data:** Jalankan sel ini untuk memuat, membersihkan, dan menyiapkan semua data.
6.  **Sel 6: Inisialisasi Hasil:** Jalankan untuk membuat wadah penyimpanan hasil.
7.  **Sel Pelatihan (Sel 7, 8, 9, 10, 11, 12, 13):** Jalankan sel-sel ini satu per satu untuk melatih setiap model.
8.  **Sel 14: Analisis Hasil:** Setelah semua model yang diinginkan selesai dilatih, jalankan sel ini untuk melihat tabel perbandingan, visualisasi, dan analisis lengkap.

---

## 🏆 Hasil Akhir

Berikut adalah rekapitulasi performa akhir dari semua model yang dievaluasi pada data test, diurutkan berdasarkan akurasi.

| Peringkat | Nama Model      | Tipe Metode | Akurasi | Test Loss |
| :-------- | :-------------- | :---------- | :------ | :-------- |
| 1         | **WordCNN** | Pembanding  | 99.22%  | 0.0297    |
| 2         | **AttentionRNN**| Pembanding  | 99.12%  | 0.0324    |
| 3         | **RCNN** | Pembanding  | 99.11%  | 0.0316    |
| 4         | **WordRNN** | Pembanding  | 99.06%  | 0.0358    |
| 5         | **VDCNN** | Usulan      | 98.94%  | 0.0404    |
| 6         | **Dense** | Pembanding  | 98.61%  | 0.0504    |
| 7         | **CharCNN** | Pembanding  | 98.12%  | 0.0659    |

---

## 💡 Analisis & Kesimpulan Utama

1.  **Kesesuaian Arsitektur adalah Kunci:** Hasil eksperimen secara definitif membuktikan bahwa keberhasilan model sangat bergantung pada kesesuaian arsitekturnya dengan sifat alami data dan tugas.

2.  **Kata Mengalahkan Karakter:** Untuk klasifikasi topik, di mana _makna kata_ adalah sinyal utama, model yang beroperasi pada level kata (WordCNN, RNN) secara signifikan lebih unggul daripada model yang bekerja dari level karakter (VDCNN, CharCNN).

3.  **Mengapa VDCNN Tidak Menjadi Juara?**
    - **Kesenjangan Informasi:** VDCNN harus bekerja lebih keras untuk "belajar" membentuk kata dari karakter sebelum dapat memahami topik.
    - **Fokus yang Salah:** Arsitektur VDCNN dirancang untuk ahli dalam _morfologi_ (struktur internal kata), yang kurang relevan untuk tugas klasifikasi topik dibandingkan pemahaman makna kata.
    - **Kompleksitas vs. Manfaat:** Kedalaman 29 lapis tidak memberikan manfaat yang sepadan karena fitur yang diekstraknya (pola karakter rumit) bukanlah sinyal terpenting untuk tugas ini.

Secara keseluruhan, proyek ini berhasil menunjukkan bahwa meskipun canggih, arsitektur yang kompleks tidak selalu menjamin performa terbaik. Pemilihan model yang tepat untuk masalah yang tepat tetap menjadi prinsip paling fundamental dalam _machine learning_.
