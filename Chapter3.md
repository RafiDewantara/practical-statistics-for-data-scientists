# Chapter 3 — Statistical Experiments and Significance Testing

**Referensi:** Peter Bruce, Andrew Bruce, dan Peter Gedeck, *Practical Statistics for Data Scientists*, 2nd Edition, O'Reilly Media, 2020.

## 1. Overview

Chapter 3 membahas bagaimana statistical experiments dapat dirancang dan dianalisis untuk menentukan apakah suatu perbedaan yang diamati kemungkinan disebabkan oleh random variation. Beberapa topik yang dibahas meliputi A/B testing, hypothesis tests, resampling dan permutation tests, p-values, t-tests, multiple testing, degrees of freedom, ANOVA, chi-square tests, multi-arm bandits, dan statistical power.

## 2. A/B Testing

A/B testing digunakan untuk membandingkan dua treatment.

Sebagai contoh:

- Group A menggunakan website versi original.
- Group B menggunakan website yang sudah didesain ulang.

Setelah itu, suatu response variable seperti click-through rate atau conversion rate dibandingkan antara kedua kelompok tersebut.

Adanya control group penting karena kelompok ini memberikan baseline yang dapat digunakan sebagai pembanding untuk mengevaluasi treatment yang diberikan.

## 3. Hypothesis Testing

Hypothesis test dimulai dengan **null hypothesis** \(H_0\), yaitu hipotesis yang menggambarkan kondisi acuan atau asumsi bahwa tidak terdapat effect.

Sementara itu, **alternative hypothesis** \(H_A\) menunjukkan adanya perbedaan atau kondisi yang menyimpang dari null hypothesis.

Sebagai contoh:

\[
H_0:\mu_A=\mu_B
\]

\[
H_A:\mu_A\neq\mu_B.
\]

Test statistic digunakan untuk mengukur seberapa besar perbedaan yang diamati dibandingkan dengan random variation yang diharapkan jika null hypothesis benar.

## 4. Permutation Tests

Permutation test menggunakan data yang tersedia untuk membentuk reference distribution berdasarkan null hypothesis.

Misalnya, terdapat dua group yang memiliki mean berbeda. Jika null hypothesis menyatakan bahwa label group tidak memiliki pengaruh, maka label tersebut dapat diacak (**shuffled**) berkali-kali. Pada setiap pengacakan, perbedaan mean kembali dihitung.

```python
import numpy as np

rng = np.random.default_rng(42)

a = np.array([10, 12, 13, 11, 14])
b = np.array([15, 17, 16, 18, 14])

observed = b.mean() - a.mean()
combined = np.concatenate([a, b])

differences = []

for _ in range(10000):
    shuffled = rng.permutation(combined)
    a_perm = shuffled[:len(a)]
    b_perm = shuffled[len(a):]
    differences.append(b_perm.mean() - a_perm.mean())

p_value = np.mean(np.abs(differences) >= abs(observed))

print("Observed difference:", observed)
print("Approximate p-value:", p_value)
```

Permutation tests menarik untuk digunakan karena metode ini membutuhkan lebih sedikit asumsi mengenai distribusi data dibandingkan dengan beberapa classical tests.

## 5. p-Values

**p-value** adalah probability, berdasarkan null model dan asumsi dari suatu test, untuk mendapatkan hasil yang setidaknya sama ekstremnya dengan hasil yang diamati.

p-value **bukan** merupakan probability bahwa null hypothesis benar.

Nilai p-value yang kecil menunjukkan bahwa hasil yang diamati relatif tidak biasa jika null hypothesis benar.

## 6. Statistical Significance Versus Practical Importance

Hasil yang **statistically significant** belum tentu memiliki **practical importance** yang besar.

Dengan sample yang sangat besar, effect yang sangat kecil dapat menghasilkan p-value yang kecil. Sebaliknya, suatu effect yang sebenarnya penting dapat tidak mencapai statistical significance jika sample yang digunakan terlalu kecil.

Oleh karena itu, analisis sebaiknya juga mempertimbangkan:

- effect size,
- uncertainty,
- practical consequences,
- sample size,
- dan experimental context.

Dengan demikian, statistical significance sebaiknya tidak digunakan sebagai satu-satunya dasar untuk menentukan apakah suatu hasil memiliki arti penting dalam praktik.

## 7. Type I and Type II Errors

**Type I error** terjadi ketika true null hypothesis ditolak.

**Type II error** terjadi ketika false null hypothesis tidak ditolak.

Significance level \(\alpha\) digunakan untuk mengontrol intended Type I error rate berdasarkan asumsi dari prosedur testing yang digunakan.

Secara sederhana, Type I error berkaitan dengan kesalahan ketika kita menyimpulkan terdapat effect padahal null hypothesis sebenarnya benar. Sementara itu, Type II error berkaitan dengan kondisi ketika terdapat effect, tetapi hasil pengujian tidak berhasil mendeteksinya.

## 8. t-Tests

**t-test** merupakan metode yang umum digunakan untuk membandingkan mean.

```python
from scipy import stats

group_a = [10, 12, 13, 11, 14]
group_b = [15, 17, 16, 18, 14]

result = stats.ttest_ind(group_a, group_b, equal_var=False)

print(result)
```

t-statistic membandingkan perbedaan yang diamati dengan estimated standard error dari perbedaan tersebut.

Dengan kata lain, t-test membantu menentukan apakah perbedaan mean antara dua group cukup besar jika dibandingkan dengan variability yang ada pada data.

## 9. Multiple Testing

Ketika banyak hypothesis diuji secara bersamaan, probability untuk mendapatkan setidaknya satu hasil yang terlihat significant hanya karena chance akan meningkat.

Sebagai contoh, jika 100 hypothesis yang tidak saling berhubungan diuji dengan nominal significance level sebesar 5%, beberapa p-value yang kecil dapat muncul meskipun seluruh null hypothesis sebenarnya benar.

Multiple-testing procedures dapat digunakan untuk mengurangi masalah tersebut. Metode yang sesuai bergantung pada tujuan dan konteks analisis.

## 10. ANOVA

**Analysis of Variance (ANOVA)** digunakan untuk membandingkan mean dari beberapa group.

F-statistic membandingkan variability antar-group dengan variability di dalam masing-masing group.

Secara konseptual:

\[
F = \frac{\text{between-group variation}}
{\text{within-group variation}}.
\]

F-statistic yang besar menunjukkan bahwa perbedaan antar-group mean lebih besar dibandingkan dengan yang dapat dijelaskan oleh within-group variation saja, dengan tetap mempertimbangkan asumsi dari model yang digunakan.

## 11. Chi-Square Test

Chi-square methods berguna untuk menganalisis **categorical data**. Metode ini dapat digunakan untuk menguji apakah observed counts masih sesuai dengan suatu independence assumption.

Untuk observed count \(O\) dan expected count \(E\):

\[
\chi^2 = \sum \frac{(O-E)^2}{E}.
\]

Contoh penggunaannya dalam Python:

```python
from scipy.stats import chi2_contingency

table = [
    [30, 20],
    [10, 40]
]

chi2, p, dof, expected = chi2_contingency(table)

print("Chi-square:", chi2)
print("p-value:", p)
```

Nilai chi-square menunjukkan seberapa besar perbedaan antara observed counts dan expected counts berdasarkan asumsi yang digunakan.

## 12. Multi-Arm Bandits

Traditional A/B testing biasanya menggunakan experimental design yang sudah ditentukan sejak awal. **Multi-arm bandit methods** memungkinkan proses experimentation dan optimization dilakukan secara bersamaan.

Dalam metode ini, lebih banyak observations dapat dialokasikan kepada treatment yang terlihat lebih menjanjikan berdasarkan hasil yang sudah diperoleh.

Pendekatan ini sangat relevan untuk **online experimentation**, karena sistem dapat terus belajar dari hasil eksperimen sambil menentukan treatment mana yang perlu mendapatkan lebih banyak observations.

## 13. Power and Sample Size

**Statistical power** berkaitan dengan probability untuk mendeteksi suatu effect ketika effect tersebut memang benar-benar ada.

Power dipengaruhi oleh beberapa faktor, yaitu:

- sample size,
- effect size,
- variability,
- significance level,
- dan statistical test yang digunakan.

Sample yang lebih besar umumnya meningkatkan kemampuan untuk mendeteksi effect yang kecil. Namun, menambah jumlah observations tidak dapat memperbaiki masalah jika sejak awal sampling design yang digunakan memiliki bias secara mendasar.

## 14. Conclusion

Statistical significance testing merupakan framework yang digunakan untuk mengukur seberapa mengejutkan suatu hasil yang diamati jika dibandingkan dengan kondisi yang diasumsikan oleh null model.

A/B tests dan permutation tests sangat berguna dalam data science karena menghubungkan statistical reasoning secara langsung dengan experimental process. Namun, **p-values tidak seharusnya dianggap sebagai satu-satunya ukuran untuk menentukan penting atau tidaknya suatu hasil**.

Dalam melakukan analisis, kita juga perlu mempertimbangkan effect size, uncertainty, experimental design, serta practical consequences.

Dengan demikian, hasil statistical test sebaiknya tidak hanya dilihat dari apakah suatu hasil significant atau tidak, tetapi juga dari **seberapa besar effect yang ditemukan, seberapa pasti estimasinya, bagaimana eksperimennya dirancang, dan apakah hasil tersebut memiliki arti secara praktis**.
