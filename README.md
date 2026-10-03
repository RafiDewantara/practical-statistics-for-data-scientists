# Practical Statistics for Data Scientists

Repository ini berisi rangkuman, penjelasan teori, dan implementasi menggunakan Python yang mengacu pada buku ***Practical Statistics for Data Scientists, 2nd Edition*** karya Peter Bruce, Andrew Bruce, dan Peter Gedeck, yang diterbitkan oleh O'Reilly.

Repository ini disusun berdasarkan bab. Setiap bab berisi penjelasan mengenai konsep-konsep statistik utama, rumus penting, serta contoh penerapan menggunakan Python.

Untuk saat ini, repository ini mencakup **Chapter 1–4**.

## Repository Contents

### Chapter 1 — Exploratory Data Analysis

**Exploratory Data Analysis (EDA)** berfokus pada proses memahami dan mengeksplorasi dataset sebelum menerapkan statistical models atau machine learning algorithms.

Pada bab ini dibahas beberapa teknik dasar untuk memahami data, seperti:

- Types of data and variables
- Estimates of location, seperti mean dan median
- Estimates of variability, seperti variance dan standard deviation
- Percentiles dan quantiles
- Data distributions
- Histograms dan frequency tables
- Boxplots
- Correlation
- Scatterplots
- Exploring categorical dan numerical data

Tujuan utama dari bab ini adalah mendapatkan gambaran awal mengenai sebuah dataset serta menemukan pola, data yang tidak biasa, dan kemungkinan masalah pada data.

Contoh menggunakan Python diberikan untuk menunjukkan bagaimana konsep-konsep tersebut dapat diterapkan dengan library seperti **Pandas, NumPy, dan Matplotlib**.

---

### Chapter 2 — Data and Sampling Distributions

**Data and Sampling Distributions** membahas konsep dasar mengenai bagaimana data dikumpulkan dan bagaimana sebuah sample berhubungan dengan population yang lebih besar.

Pada bab ini dibahas:

- Population dan sample
- Sampling procedures
- Sampling bias
- Random sampling
- Selection bias
- Sampling distributions
- The Central Limit Theorem
- Standard error
- Bootstrap sampling
- Confidence intervals
- Normal and related distributions

Salah satu fokus utama bab ini adalah memahami bahwa statistical values yang dihitung dari sebuah sample dapat berbeda antara satu sample dengan sample lainnya. Sampling distributions digunakan untuk memahami variasi tersebut.

Bab ini juga memperkenalkan **bootstrap methods**, yaitu metode yang memungkinkan sampling distributions dan confidence intervals diperkirakan secara computational.

Contoh Python digunakan untuk menunjukkan proses sampling, simulation, bootstrap procedures, dan statistical distributions.

---

### Chapter 3 — Statistical Experiments and Significance Testing

**Statistical Experiments and Significance Testing** menjelaskan bagaimana sebuah eksperimen dapat dirancang dan bagaimana metode statistik digunakan untuk menentukan apakah perbedaan atau hubungan yang ditemukan memberikan cukup bukti terhadap null hypothesis.

Bab ini membahas:

- Controlled experiments
- Treatment dan control groups
- Randomization
- Hypothesis testing
- Null dan alternative hypotheses
- Permutation tests
- P-values
- Statistical significance
- Type I dan Type II errors
- t-tests
- ANOVA
- Chi-square tests

Bab ini menekankan pentingnya membedakan antara hasil yang diamati dengan bukti bahwa hasil tersebut secara statistik signifikan.

Contoh Python digunakan untuk menunjukkan penerapan permutation tests, hypothesis testing, dan berbagai statistical procedures lainnya.

---

### Chapter 4 — Regression and Prediction

**Regression and Prediction** memperkenalkan regression sebagai metode untuk memahami hubungan antarvariabel dan membuat predictions.

Bab ini membahas:

- Simple linear regression
- Multiple linear regression
- Least squares
- Regression coefficients
- Residuals
- Standard error
- Model evaluation
- Correlation and regression
- Confounding variables
- Polynomial dan categorical variables
- Regression diagnostics

Simple regression model dapat dituliskan sebagai:

\[
Y = b_0 + b_1X + \epsilon
\]

where:

- \(Y\) is the response or dependent variable
- \(X\) is the predictor variable
- \(b_0\) is the intercept
- \(b_1\) is the regression coefficient
- \(\epsilon\) represents the error

Bab ini menjelaskan bahwa regression tidak hanya dapat digunakan untuk prediction, tetapi juga untuk memahami hubungan antara variabel.

Implementasi menggunakan Python menunjukkan bagaimana membuat regression models, menginterpretasikan coefficients, mengevaluasi predictions, dan menganalisis residuals.

---

## Chapter Overview

Empat bab pertama memberikan dasar statistik yang penting untuk data science:

| Chapter | Main Topic | Main Purpose |
| ------- | ---------- | ------------ |
| 1 | Exploratory Data Analysis | Memahami dan mengeksplorasi dataset |
| 2 | Data and Sampling Distributions | Memahami sampling dan statistical variation |
| 3 | Statistical Experiments and Significance Testing | Mengevaluasi evidence menggunakan statistical tests |
| 4 | Regression and Prediction | Memodelkan hubungan dan membuat predictions |

Secara keseluruhan, keempat bab ini membahas proses yang dimulai dari **memahami data**, kemudian **memahami sampling**, dilanjutkan dengan **menguji statistical evidence**, dan akhirnya **membangun hubungan antarvariabel untuk melakukan prediction**.

## Reference

Bruce, P., Bruce, A., & Gedeck, P. (2020). *Practical Statistics for Data Scientists: 50+ Essential Concepts Using R and Python* (2nd ed.). O'Reilly Media.
