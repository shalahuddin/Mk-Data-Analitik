# Tugas Data Science — Exploratory Data Analysis (EDA) dengan Python

**Jenis:** Praktikum individu | **Media:** Jupyter Notebook / Google Colab | **Durasi yang disarankan:** 1 minggu

## Tujuan pembelajaran
Mahasiswa mampu memuat dataset, menilai kualitas data, melakukan seleksi dan agregasi menggunakan Pandas, membuat visualisasi menggunakan Matplotlib/Seaborn, dan menarik kesimpulan yang didukung hasil analisis. Pendekatannya mengikuti notebook referensi `1_EDA_Intro.ipynb`: *pertanyaan → eksplorasi → grafik → interpretasi → ringkasan temuan*.

## Dataset
1. **California Housing** (WAJIB): `california_housing_train.csv` (alternatif `california_housing_test.csv`). Bila memakai Colab, dataset biasanya ada di `/content/sample_data/`. Sumber alternatif: https://download.mlcc.google.com/mledu-datasets/california_housing_train.csv
2. **Anscombe's Quartet** (WAJIB): `anscombe.json` dari `vega_datasets`, https://github.com/vega/vega-datasets/blob/master/data/anscombe.json ; dapat juga menggunakan `vega_datasets.data.anscombe()` bila terpasang.
3. **MNIST** (BONUS): `mnist_train_small.csv` dari folder `sample_data` Google Colab. File MNIST contoh Colab umumnya **tanpa header**: kolom pertama label, sisanya nilai piksel; periksa bentuk data sebelum memproses.

## Aturan pekerjaan
- Kerjakan secara **individu**; analisis dan narasi harus ditulis sendiri.
- Pakai Python, Pandas, Matplotlib dan/atau Seaborn; NumPy diperbolehkan.
- Setiap grafik wajib memuat judul, label sumbu yang relevan, dan **interpretasi 2–4 kalimat**.
- Minimal **6 visualisasi total**: **4 California Housing** dan **2 Anscombe**. Untuk bonus MNIST tambahkan minimal 2 visualisasi.
- Jangan menghapus nilai kosong/outlier tanpa penjelasan. Jangan menganggap korelasi sebagai hubungan sebab-akibat.
- Pembelajaran ini berfokus EDA; **model prediksi tidak diwajibkan**.
- Cantumkan sumber dataset dan keterbatasan analisis.

## Bagian A — California Housing (60 poin)
### A1. Pengenalan dataset (10 poin)
Muat dataset ke `df_housing`, tampilkan `head()`, `shape`, `columns`, `info()`, `describe()` dan jelaskan arti sekurangnya 5 kolom. Tentukan variabel target analisis Anda, yaitu `median_house_value`.

### A2. Kualitas data (10 poin)
Hitung nilai kosong dan duplikasi; periksa distribusi, rentang, dan nilai ekstrem. Tuliskan keputusan: apakah ada yang perlu ditangani? Jelaskan tindakan yang diambil atau alasan mempertahankannya.

### A3. Analisis dan visualisasi (30 poin)
Jawab pertanyaan berikut dengan Pandas dan grafik:
1. Bagaimana distribusi `median_house_value`? (histogram)
2. Bagaimana hubungan `median_income` dengan `median_house_value`? (scatterplot; opsional korelasi)
3. Bagaimana hubungan `housing_median_age` dengan harga rumah? (boxplot kelompok umur rumah atau plot agregasi)
4. Apa pola geografis harga rumah menurut `longitude` dan `latitude`? (scatterplot dengan warna harga)
5. **Pertanyaan mandiri:** rumuskan 1 pertanyaan tambahan, jawab melalui filtering, grouping, atau agregasi dan sajikan visualisasi yang sesuai.

> Untuk memenuhi syarat minimal empat grafik California Housing, pilih sedikitnya empat dari lima butir di atas, tetapi **semua lima pertanyaan tetap wajib dijawab** (pertanyaan yang tidak dibuat grafik dapat dijawab memakai tabel/ringkasan statistik).

### A4. Kesimpulan California Housing (10 poin)
Tulis minimal **3 temuan berbasis hasil**, 1 kemungkinan keterbatasan dataset (mis. data historis, nilai harga yang dibatasi), dan 1 rekomendasi analisis lanjutan.

## Bagian B — Anscombe's Quartet (25 poin)
1. Muat data dan identifikasi empat kelompok `Series` (atau nama kolom kelompok sesuai struktur file). (5 poin)
2. Hitung ringkasan statistik X dan Y per kelompok: **mean, variance, dan korelasi** (gunakan variasi kolom aktual). (8 poin)
3. Buat grafik scatter untuk **keempat kelompok** dalam satu figur 2×2; tampilkan garis tren linear opsional. (8 poin)
4. Jelaskan dalam 1 paragraf: *Mengapa statistik ringkasan saja tidak cukup untuk memahami data?* (4 poin)

> Satu figur 2×2 berisi empat panel dihitung sebagai **empat panel visualisasi**, namun syarat minimal dua visualisasi pada Anscombe dipenuhi dengan menambahkan **satu grafik perbandingan lain** (misalnya boxplot Y per kelompok) selain figur scatter 2×2.

## Bagian C — BONUS MNIST (maksimum +10 poin)
1. Muat data; tentukan kolom label dan jumlah fitur piksel. Periksa nilai minimum/maksimum piksel dan bentuk data. (+3)
2. Buat bar chart distribusi label digit. (+3)
3. Tampilkan minimal 9 gambar digit menggunakan `imshow` setelah membentuk kembali deret piksel menjadi 28×28 (jika memang ada 784 fitur). (+4)

## Bagian D — Ringkasan dan pelaporan (15 poin)
Buat `README.md` dengan struktur: judul dan identitas, tujuan, dataset & sumber, metodologi ringkas, **minimal 5 insight** lintas California Housing dan Anscombe, keterbatasan, kesimpulan. Sertakan penjelasan cara menjalankan notebook.

## Bobot penilaian
| Komponen | Bobot |
|---|---:|
| A. California Housing | 60 |
| B. Anscombe's Quartet | 25 |
| D. README & komunikasi hasil | 15 |
| **Total** | **100** |
| Bonus MNIST | **+10** |

## Berkas yang dikumpulkan di Classroom
```
NIM_Nama_Tugas_EDA/
├── Tugas_EDA_NIM_Nama.ipynb
└── README.md
```
Kumpulkan dalam **1 file ZIP** atau folder Drive yang dibagikan, sesuai instruksi dosen. Notebook harus dapat dijalankan dari awal sampai akhir (*Runtime → Run all*). Tidak perlu mengunggah salinan dataset publik jika notebook mengambilnya dari sumber terbuka secara konsisten. **Batas pengumpulan:** mengikuti pengumuman dosen di Classroom.

## Checklist sebelum submit
- [ ] Semua sel notebook berjalan tanpa error.
- [ ] Data sumber dan dependensi jelas.
- [ ] Minimal 6 visualisasi disertai interpretasi.
- [ ] Pertanyaan A1–A4 dan Bagian B dijawab.
- [ ] README memuat minimal 5 insight dan keterbatasan.
- [ ] Nama berkas menggunakan NIM dan nama mahasiswa.
