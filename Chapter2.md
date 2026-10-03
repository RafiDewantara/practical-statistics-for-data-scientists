# Chapter 2 — Data dan Sampling Distributions

**Referensi:** Peter Bruce, Andrew Bruce, dan Peter Gedeck, *Practical Statistics for Data Scientists*, 2nd Edition, O'Reilly Media, 2020.

## 1. Overview

Chapter 2 membahas bagaimana sample digunakan untuk mempelajari population dan mengapa hasil estimasi dapat berbeda dari satu sample ke sample lainnya. Beberapa topik penting yang dibahas meliputi random sampling, sample bias, selection bias, regression to the mean, sampling distributions, Central Limit Theorem, standard error, bootstrap methods, confidence intervals, dan probability distributions.

## 2. Population and Sample

Population adalah keseluruhan data atau pengamatan yang menjadi objek penelitian. Sementara itu, sample adalah sebagian data yang diambil dari population tersebut.

Sebagai contoh:

- Population: seluruh mahasiswa di sebuah universitas.
- Sample: 500 mahasiswa yang dipilih untuk mengikuti survei.

Suatu population parameter, seperti population mean, biasanya tidak diketahui secara langsung. Oleh karena itu, sample statistic dihitung berdasarkan data yang diamati dan digunakan untuk memperkirakan parameter tersebut.

## 3. Random Sampling and Bias

Random sampling memberikan mekanisme pemilihan observasi yang jelas dan dapat membantu mengurangi masalah pemilihan data yang sistematis.

Namun, sample dengan jumlah yang besar belum tentu selalu representatif. Jika metode sampling yang digunakan memiliki bias, hasil estimasi tetap dapat menyesatkan meskipun ukuran sample besar.

Beberapa masalah yang umum terjadi antara lain:

- selection bias,
- self-selection,
- undercoverage,
- nonresponse,
- dan pengambilan sample dari population yang berbeda dengan target population.

Oleh karena itu, **kualitas sample dan desain sampling sama pentingnya dengan ukuran sample**.

## 4. Sample Mean and Population Mean

Sample mean dirumuskan sebagai:

\[
\bar{x} = \frac{1}{n}\sum_{i=1}^{n}x_i.
\]

Nilai tersebut digunakan untuk memperkirakan population mean \(\mu\).

Sample yang diambil secara acak biasanya akan menghasilkan sample mean yang berbeda-beda. Perbedaan ini disebut **sampling variability**.

## 5. Sampling Distribution

Sampling distribution menjelaskan bagaimana suatu statistic, seperti sample mean, berperilaku jika kita mengambil sample secara berulang dari population yang sama.

Secara sederhana, prosesnya dapat digambarkan sebagai berikut:

```text
Population
    V
many random samples
    V
calculate a statistic for each sample
    V
distribution of the statistics
```

Konsep ini penting karena membantu kita memahami adanya ketidakpastian dalam suatu estimasi.

## 6. Central Limit Theorem

Central Limit Theorem menyatakan bahwa, dalam kondisi tertentu, distribusi sample mean akan semakin mendekati distribusi normal ketika ukuran sample bertambah, meskipun population awalnya tidak berdistribusi normal.

Jika population memiliki mean \(\mu\) dan standard deviation \(\sigma\), maka sampling distribution dari mean secara pendekatan memiliki:

\[
E(\bar{x}) = \mu
\]

dan

\[
SE(\bar{x}) \approx \frac{\sigma}{\sqrt{n}}.
\]

Hubungan dengan akar kuadrat tersebut menjelaskan mengapa peningkatan ukuran sample dapat mengurangi sampling variability. Namun, semakin besar sample yang digunakan, manfaat tambahan dari setiap penambahan sample akan semakin kecil atau mengalami diminishing returns.

## 7. Standard Error

Standard error digunakan untuk mengukur seberapa besar suatu sample statistic dapat berubah jika proses pengambilan sample dilakukan berulang kali.

Untuk mean:

\[
SE \approx \frac{s}{\sqrt{n}}.
\]

```python
import numpy as np

x = np.array([10, 12, 13, 15, 20])

se = np.std(x, ddof=1) / np.sqrt(len(x))
print("Standard error:", se)
```

Perbedaan pentingnya adalah **standard deviation** menggambarkan variasi antar-observasi dalam suatu sample, sedangkan **standard error** menggambarkan variasi dari suatu estimasi jika sample diambil berulang kali.

## 8. Bootstrap

Bootstrap adalah metode resampling yang digunakan untuk memperkirakan variability dari suatu statistic tanpa sepenuhnya bergantung pada distribusi teoritis.

Prosedur dasarnya adalah:

1. Mulai dengan sample yang sudah diperoleh.
2. Ambil sample secara acak dari sample tersebut **with replacement**.
3. Hitung statistic yang ingin dianalisis.
4. Ulangi proses tersebut berkali-kali.
5. Amati distribusi dari statistic yang dihasilkan.

```python
import numpy as np

x = np.array([10, 12, 13, 15, 20])
rng = np.random.default_rng(42)

bootstrap_means = []

for _ in range(10000):
    sample = rng.choice(x, size=len(x), replace=True)
    bootstrap_means.append(sample.mean())

print(np.mean(bootstrap_means))
```

Bootstrap methods dapat digunakan untuk memperkirakan standard errors dan confidence intervals.

## 9. Confidence Intervals

Confidence interval memberikan suatu rentang nilai yang masuk akal untuk population parameter yang tidak diketahui berdasarkan prosedur statistik tertentu.

Salah satu cara sederhana untuk membuat bootstrap percentile interval adalah:

```python
lower = np.percentile(bootstrap_means, 2.5)
upper = np.percentile(bootstrap_means, 97.5)

print(lower, upper)
```

Prosedur confidence 95% dirancang sehingga, jika proses sampling dilakukan berulang kali dan asumsi dari metode tersebut terpenuhi, sekitar 95% dari interval yang dihasilkan akan mencakup true parameter.

## 10. Important Probability Distributions

Chapter ini juga memperkenalkan beberapa probability distributions yang umum digunakan dalam statistical analysis.

### Normal Distribution

Normal distribution memiliki bentuk yang simetris dan menyerupai lonceng. Distribusi ini ditentukan oleh mean dan standard deviation.

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.random.normal(loc=50, scale=10, size=1000)

plt.hist(x, bins=30)
plt.show()
```

### Student's t-Distribution

t-distribution digunakan ketika melakukan estimasi terhadap mean dan population standard deviation tidak diketahui, terutama ketika ukuran sample relatif kecil. Distribusi ini memiliki tails yang lebih berat dibandingkan standard normal distribution.

### Binomial Distribution

Binomial distribution digunakan untuk menggambarkan jumlah keberhasilan (**successes**) dalam sejumlah percobaan (**trials**) yang jumlahnya tetap, dengan setiap percobaan bersifat independen dan memiliki probability of success yang sama.

### Chi-Square and F Distributions

Distribusi Chi-Square dan F digunakan dalam berbagai prosedur inference yang berkaitan dengan variance, categorical data, serta perbandingan variability.

### Poisson Distribution

Poisson distribution digunakan untuk memodelkan jumlah kejadian yang muncul dalam suatu interval tertentu ketika event rate dapat digunakan untuk menggambarkan proses tersebut.

### Exponential and Weibull Distributions

Exponential dan Weibull distributions dapat digunakan untuk memodelkan waiting times dan failure processes.

## 11. Regression to the Mean

Regression to the mean terjadi ketika suatu pengamatan yang nilainya sangat tinggi atau sangat rendah cenderung diikuti oleh pengamatan yang nilainya lebih dekat dengan nilai rata-rata. Hal ini terutama dapat terjadi ketika hasil pengukuran memiliki komponen yang bersifat random.

Fenomena ini tidak boleh langsung dianggap sebagai bukti bahwa suatu intervention menyebabkan perubahan tersebut.

## 12. Conclusion

Sampling distributions menjadi penghubung antara sample yang diamati dengan population-level inference. Central Limit Theorem menjelaskan mengapa normal approximation sering digunakan dalam statistical analysis, sedangkan standard error digunakan untuk mengukur variability dari suatu estimasi.

Bootstrap methods memberikan pendekatan komputasional yang fleksibel untuk memperkirakan uncertainty, terutama ketika penggunaan rumus teoritis tidak praktis atau cukup sulit.

Hal penting yang perlu dipahami adalah bahwa **sample statistic merupakan sebuah estimasi, bukan gambaran yang sepenuhnya pasti mengenai population**. Oleh karena itu, statistical analysis perlu mempertimbangkan sampling variability dan uncertainty.

## 13. References

Bruce, P., Bruce, A., & Gedeck, P. (2020). *Practical Statistics for Data Scientists* (2nd ed.). O'Reilly Media.
