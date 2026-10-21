# Pertemuan 8 — Review Terintegrasi dan Persiapan UTS Data Analitik

> **Posisi dalam RPS:** Pertemuan 8 merupakan **UTS**. Dokumen ini adalah **bahan review dan praktikum penguatan**, **bukan naskah soal UTS atau pengganti prosedur penilaian kampus**. Kegiatan ujian resmi harus mengikuti keputusan dosen dan jadwal akademik.

**Sasaran:** Mahasiswa semester 5 Teknologi Informasi. **Durasi referensi:** 150 menit untuk sesi review/persiapan jika tidak sedang berlangsung ujian resmi. **Prasyarat:** pengantar data analitik (P1) hingga inferensi statistik (P7), termasuk pengantar Python, Pandas, cleaning, EDA, dan visualisasi.

## 1. Hasil belajar sesi review

Setelah menelaah bahan ini mahasiswa mampu: (a) menjelaskan alur analitik dari masalah ke rekomendasi; (b) memuat, mengaudit, dan meringkas dataset dengan Python/Pandas; (c) mengomunikasikan EDA sederhana; (d) membedakan deskripsi dari inferensi; (e) menafsirkan interval kepercayaan dan p-value secara hati-hati; (f) menjelaskan batas klaim kausal.

## 2. Peta besar proses analitik

**Pertanyaan → sumber data → pemeriksaan kualitas → transformasi → EDA → analisis/inferensi → komunikasi → keputusan → monitoring**. Setiap tahapan memerlukan keputusan, asumsi, dan dokumentasi. Analitik **deskriptif** menjawab apa yang terjadi; **diagnostik** mengeksplorasi mengapa; **prediktif** memperkirakan apa yang mungkin terjadi; **preskriptif** menyarankan tindakan. Sebuah diagram atau uji signifikansi tidak otomatis menghasilkan rekomendasi yang layak.

**Kasus terpadu:** Dataset Iris memiliki empat ukuran bunga dalam satuan cm dan label spesies. Pertanyaan analitik: *Bagaimana perbedaan panjang petal antarspesies, dan seberapa besar ketidakpastiannya?* Ini dataset **observasional**, bukan sampel acak dari seluruh bunga di dunia. Gunakan hasil untuk belajar metode, bukan klaim generalisasi yang berlebihan.

## 3. Menguasai fondasi Python & Pandas

Variabel menyimpan nilai, `list` menyimpan rangkaian, `dict` menyimpan pasangan kunci-nilai; Pandas `DataFrame` merepresentasikan data tabular.

```python
import pandas as pd
from pathlib import Path
path = Path('praktikum/data/iris.csv')
df = pd.read_csv(path)
print(df.head())
print(df.shape)
print(df.dtypes)
```

Jika menjalankan skrip dari folder lain, sesuaikan path. Pada notebook praktikum sudah disertakan mekanisme pencarian file dan fallback data Iris berlabel sumbernya.

**Memilih & merangkum:**

```python
print(df[['species', 'petalLength']].head())
print(df.groupby('species')['petalLength'].agg(['count', 'mean', 'std']))
print(df.loc[df['species'] == 'setosa', 'sepalLength'].median())
```

**Prinsip audit:** cek `shape`, `info`, `isna().sum()`, `duplicated().sum()`, nilai ekstrem, satuan dan definisi kolom. Baris duplikat **tidak otomatis salah**: dua observasi bunga dapat memiliki pengukuran identik. Jangan `drop_duplicates()` tanpa alasan substantif.

## 4. Statistik deskriptif dan EDA

- **Mean**: rata-rata; sensitif terhadap pencilan.
- **Median**: nilai tengah; lebih tahan pencilan.
- **Standar deviasi sampel**: sebaran di sekitar mean, dihitung dengan penyebut `n−1`.
- **Kuartil/IQR**: ringkasan sebaran robust; IQR = Q3−Q1.
- **Distribusi**: periksa dengan histogram; **perbandingan kelompok**: boxplot; **hubungan numerik**: scatter.

```python
import matplotlib.pyplot as plt
fig, ax = plt.subplots()
for species, group in df.groupby('species'):
    ax.scatter(group['sepalLength'], group['petalLength'], label=species, alpha=.7)
ax.set(xlabel='Panjang sepal (cm)', ylabel='Panjang petal (cm)', title='Eksplorasi Iris')
ax.legend()
plt.show()
```

**Interpretasi:** pemisahan visual kelompok bukan bukti bahwa satu ukuran menyebabkan ukuran lain. Nilai `corr` mengukur asosiasi linear, bukan kausalitas. Perhatikan distribusi per kelompok dan hindari menggabungkan kategori tanpa memeriksa struktur.

## 5. Jembatan menuju inferensi

Statistik sampel dipakai memperkirakan parameter populasi di bawah asumsi tertentu. **Confidence interval (CI) 95%** adalah prosedur yang dalam pengulangan sampling ideal mencakup parameter sebenarnya sekitar 95% waktu; **bukan** probabilitas 95% bahwa parameter yang sudah tetap berada dalam interval khusus ini (pendekatan frequentist).

Untuk mean sampel dengan simpangan baku `s`, standar error `SE = s/√n`. Interval t satu sampel: `x̄ ± t_(0.975, n−1) × SE`, bila pengamatan layak dianggap independen dan asumsi metode cukup terpenuhi. Contoh manual untuk `n=25`, `x̄=4.00`, `s=1.00`, dan `t*≈2.064`: `SE=0.20`; CI 95% ≈ `[3.587, 4.413]`.

**Hipotesis:** `H0: μ_A = μ_B`; `H1: μ_A ≠ μ_B`. Untuk dua kelompok independen dengan potensi ragam berbeda, **Welch t-test** sering menjadi pilihan, tetapi selalu periksa independensi, bentuk distribusi dan apakah desain pengambilan sampel memungkinkan inferensi. `p-value` adalah peluang mendapatkan statistik uji setidaknya seekstrem hasil pengamatan **dengan asumsi H0 dan model pengujian benar**; bukan peluang H0 benar. Aturan `p < 0.05` adalah ambang keputusan konvensional, bukan ukuran dampak atau kepastian.

```python
from scipy import stats
a = df.loc[df.species == 'setosa', 'petalLength']
b = df.loc[df.species == 'versicolor', 'petalLength']
t_stat, p_value = stats.ttest_ind(a, b, equal_var=False)
print(f'Welch t = {t_stat:.3f}; p-value = {p_value:.3g}')
```

**Penting:** Iris adalah data observasional. Uji di atas mengilustrasikan *perbedaan ukuran antar-kelompok*, **bukan** A/B testing dan bukan bukti bahwa label spesies menjadi sebab tunggal. A/B test sah memerlukan **randomisasi perlakuan** ke unit yang tepat dan hasil yang diukur setelah intervensi.

## 6. Memahami A/B testing tanpa menyamakan dengan Iris

Dalam eksperimen A/B, peserta diacak ke perlakuan A dan B, metrik dipilih sebelum melihat hasil, durasi eksperimen dan sampel direncanakan, lalu dampak dan interval dilaporkan. Pada *simulasi* dapat membagi 200 pengguna sintetis secara acak ke dua versi tampilan dan membandingkan rasio klik. Itu **DATA SIMULASI**, bukan hasil eksperimen mahasiswa atau data nyata.

**Checklist validitas:** unit randomisasi, pencatatan metrik, missing data, potensi pengujian berulang, ukuran efek, faktor pengganggu, dan relevansi praktis. Keputusan yang etis juga mempertimbangkan privasi dan fairness.

## 7. Alur komunikasi temuan

1. Ulangi pertanyaan bisnis/akademik. 2. Sebutkan asal, unit, dan jumlah data. 3. Ringkas kualitas dan batasannya. 4. Tampilkan grafik berjudul jelas dan satuan. 5. Jelaskan hasil statistik dan ketidakpastian. 6. Pisahkan temuan, interpretasi, dan rekomendasi. 7. Hindari klaim kausal bila desain tak mendukung.

**Contoh narasi:** “Pada dataset Iris yang digunakan untuk pembelajaran, kelompok setosa memiliki panjang petal rata-rata lebih kecil daripada versicolor. Temuan ini tidak dengan sendirinya menggambarkan seluruh populasi Iris atau membuktikan pengaruh kausal spesies.”

## 8. Kekeliruan umum

| Kekeliruan | Perbaikan konsep |
|---|---|
| Data bersih berarti semua nilai duplikat harus dibuang | Duplikat harus dinilai berdasar unit observasi |
| Korelasi tinggi berarti sebab-akibat | Korelasi hanya asosiasi |
| `p < 0.05` berarti H0 pasti salah | Nilai p bersyarat pada H0/asumsi |
| CI 95% artinya parameter berpeluang 95% dalam interval ini | Interpretasi frequentist melalui cakupan prosedur |
| Iris setosa vs versicolor adalah A/B testing | Itu perbandingan observasional, bukan randomisasi perlakuan |
| Perbedaan signifikan pasti penting | Ukuran efek dan konteks praktis tetap perlu dilihat |

## 9. Panduan belajar mandiri (tanpa tugas pengumpulan)

Jalankan notebook langkah demi langkah; saat setiap output muncul, coba jelaskan: apa yang diukur, bagaimana data diproses, mengapa visualisasi dipilih, dan apa yang **tidak bisa** disimpulkan. Cocokkan hasil dengan diskusi kelas, bukan menghafal satu p-value.

## Referensi

- RPS Data Analitik (dokumen mata kuliah, sumber utama urutan pertemuan; P8 = UTS).
- Data Science Indonesia, kurikulum: https://github.com/datascienceid/datasciencecurriculum
- Vega Datasets (referensi skema Iris): https://github.com/altair-viz/vega_datasets
- scikit-learn dataset Iris: https://scikit-learn.org/stable/auto_examples/datasets/plot_iris_dataset.html
- SciPy `ttest_ind`: https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.ttest_ind.html
