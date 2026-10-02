# Chapter 1 — Exploratory Data Analysis

**Referensi:** Peter Bruce, Andrew Bruce, dan Peter Gedeck, *Practical Statistics for Data Scientists*, 2nd Edition, O'Reilly Media, 2020.

## 1. Overview

Exploratory Data Analysis (EDA) adalah proses memeriksa dataset sebelum menggunakan model statistika atau algoritma machine learning. Tujuan utama dari proses ini untuk memahami stukture, values, distribusi, observasi, dan relasi dari variables yang ada.

Chapter satu berfokus kepada struktur dan rectangular data atau jenis data yang direpresentasikan dalam bentuk dua dimensi, yaitu baris untuk records, dan kolom untuk variable. Pada bagian ini juga diperkenalkan perhitungan lokasi variabilitas, visualisasi distribusi, kategori variable, korelasi, scatterplot, dan cara untuk eksplorasi beberapa variabel secara bersamaan.

## 2. Structured Data

Struktur data dapat direpresentasikan pada tabel dengan variabel numerik yang kontinu atau diskrit, dengan kategori variabel yang biner atau ordinal.

- **Kontinu:** dapat mengambil beberapa nilai dalam satu interval.
- **Diskrit:** umumnya merepresentasikan perhitungan/counts.
- **Kategori:** representasi membership pada kumpulan kategori.
- **Biner:** memiliki dua kategori yaitu 0-1 atau iya-tidak.
- **Ordinal:** menilai kategori dengan urutan yang bermakna.

Pada ilmu data, rectangular dataset biasanya disimpan secara pandas `DataFrame`.

```python
import pandas as pd

df = pd.DataFrame({
    "umur": [20, 22, 25, 28, 31],
    "jam_belajar": [2, 4, 5, 7, 8],
    "fakultas": ["FTE", "FIK", "FEB", "FRI", "FIT"],
    "passed": [0, 1, 1, 1, 1]
})

print(df.head())
print(df.info())
```

## 3. Mengukur Lokasi

Lokasi pada buku ini mendeskripsikan nilai yang sentral.

### Mean

Aritmatika mean adalah

\[
\bar{x} = \frac{1}{n}\sum_{i=1}^{n}x_i.
\]

```python
import numpy as np

x = np.array([10, 12, 13, 15, 20])

print("Mean:", np.mean(x))
```

Mean menggunakan setiap observasi, tetapi dapat terpengaruh dengan nilai yang ekstrim.

### Median

Median adalah nilai tengah setelah data observasi diurutkan. Hasilnya lebih resistan pada nilai ekstrim daripada mean.

```python
print("Median:", np.median(x))
```

### Trimmed Mean

A trimmed mean atau mean berkondisi menghapus porsi spesifik dari observasi terbesar dan terkecil sebelum dilakukan kalkulasi. Fungsinya adalah agar nilai ektrim tidak terlalu mempengaruhi mean dan lebih kokoh daripada median.

```python
from scipy.stats import trim_mean

print("10% trimmed mean:", trim_mean(x, 0.1))
```

### Weighted Mean

Saat observasi memiliki kepentingan atau proporsi populasi yang berbeda, lebih baik digunakan weighted mean.

```python
weights = np.array([1, 1, 2, 2, 3])
print(np.average(x, weights=weights))
```

## 4. Mengukur Variabilitas

Lokasi bila sendiri tidak dapat mendeskripsikan sebuah dataset. Dua dataset mungkin memiliki dua mean yang sama, tetapi jumlah penyebaran yang berbeda.

### Variance and Standard Deviation

Sample variance adalah

\[
s^2 = \frac{\sum_{i=1}^{n}(x_i-\bar{x})^2}{n-1}.
\]

Standard deviation adalah

\[
s = \sqrt{s^2}.
\]

```python
print("Variance:", np.var(x, ddof=1))
print("Standard deviation:", np.std(x, ddof=1))
```

### Interquartile Range

The interquartile range adalah

\[
IQR = Q_{75}-Q_{25}.
\]

Ini menjelaskan penyebaran dari tengah (50%) observasi dan tidak terlalu sensitif terhadap nilai ekstrim daripada range.

```python
q1 = np.percentile(x, 25)
q3 = np.percentile(x, 75)
print("IQR:", q3 - q1)
```

### Median Absolute Deviation

The median absolute deviation (MAD) berdasarkan jarak dari median dan kokoh terhadap pengukuran varibilitas. 

```python
from statsmodels.robust.scale import mad

print("MAD:", mad(x))
```

## 5. Menjelajahi Distributions

Sebuah distribusi memperlihatkan bagaimana nilai disebarkan terhadap rentang yang dimiliki.

### Histogram

```python
import matplotlib.pyplot as plt

plt.hist(x, bins=5)
plt.xlabel("Value")
plt.ylabel("Frequency")
plt.title("Distribution")
plt.show()
```

### Boxplot

Boxplot menyediakan tampilan tersusun dari median, quartil, dan potensi observasi ekstrim.

```python
plt.boxplot(x)
plt.ylabel("Value")
plt.title("Boxplot")
plt.show()
```

Terutama persentase yang berguna untuk menjelaskan data yang melenceng karena tidak membutuhkan distribusi yang simetris.

## 6. Kategori Data

Untuk kategori variable, frekuensi tabel biasanya lebih informatif daripada mean.

```python
print(df["fakultas"].value_counts())
print(df["lulus"].value_counts(normalize=True))
```

**mode** adalah kategori yang paling sering muncul.

Untuk variabel biner, proporsi observasi dari tiap kategori dapat diinterpretasikan dengan probabilitas empiris.

## 7. Korelasi

Mengukur kekuatan dan arah asosiasi antara variabel numerik.

Pearson's correlation coefficient adalah

\[
r =
\frac{\sum_i(x_i-\bar{x})(y_i-\bar{y})}
{\sqrt{\sum_i(x_i-\bar{x})^2}\sqrt{\sum_i(y_i-\bar{y})^2}}.
\]

bernilai dari -1 sampai +1.

```python
print(df[["umur", "jam_belajar", "passed"]].corr())
```

Korelasi yang positif menandakan nilai variabel yang besar cenderung berasosiasi dengan nilai besar lainnya. Sedangkan korelasi negatif menandakan pola yang sebaliknya.

Korelasi seharusnya tidak otomatis diinterpretasikan sebagai kausalitas, variabel ketiga, efek seleksi, atau mekanisme lainnya yang dijelaskan pada asosiasi yang terobservasi.

## 8. Scatterplots

Scatterplots berguna untuk meneliti hubungan antara dua variabel numerik.

```python
plt.scatter(df["jam_belajar"], df["umur"])
plt.xlabel("Jam Belajar")
plt.ylabel("Umum")
plt.title("Jam Belajar vs Umur")
plt.show()
```

Inspeksi visual dapat memperlihatkan hubungan nonlinear, kluster, outlier, dan perubahan variabilitas.

## 9. Explorasi Beberapa Variable

Dataset nyata memiliki banyak variabel. Tool yang berguna dan dapat digunakan antara lain yaitu:
- correlation matrices,
- grouped summaries,
- scatterplots,
- boxplots,
- hexagonal binning,
- contour plots,
- and multivariable visualizations.

Contoh:

```python
import seaborn as sns

sns.pairplot(df[["age", "study_hours", "passed"]])
plt.show()
```

## 10. EDA Workflow Praktis

Workflow yang praktis adalah:

1. Inspeksi baris and kolom.
2. Identifikasi tipe variabel.
3. Cek nilai yang hilang dan observasi yang tidak biasa.
4. Kalkulasi ukuran dari lokasi.
5. Kalkulasi ukuran dari variabilitas.
6. Visualisasi distribusi.
7. Menyelidiki relasi dari variabel.
8. Investigasi kemungkinan outliers.
9. Melanjutkan ke statistical modeling.

## 11. Conclusion

Pembelajaran utama dari chapter ini yaitu analisis statistika seharusnya dimulai dari pengertian data itu sendiri. Mean dan standar deviasi menyediakan ringkasan yang berguna, sedangkan median, persentase, dan MAD lebih kokoh bila terjadi outlier atau distribusi yang condong terjadi. Metode grafik seperti histograms, boxplots, dan scatterplots memperlihatkan pola yang dapat memperlihatkan statistik lebih jelas

EDA bukan sekedar pendahuluan. Tetapi membantu determinasi metode statistika dan model machine learning mana yang sesuai dengan masalah yang dimiliki.
