# Chapter 4 — Regression and Prediction

**Referensi:** Peter Bruce, Andrew Bruce, dan Peter Gedeck, *Practical Statistics for Data Scientists*, 2nd Edition, O'Reilly Media, 2020.

## 1. Overview

Regression models digunakan untuk menjelaskan hubungan antara predictor variables dengan numerical outcome. Chapter ini membahas simple dan multiple linear regression, kemudian dilanjutkan dengan prediction, categorical variables, correlated predictors, multicollinearity, confounding, interactions, diagnostics, polynomial regression, splines, dan generalized additive models.

## 2. Simple Linear Regression

Model dasarnya adalah:

\[
Y = \beta_0 + \beta_1X + \epsilon.
\]

Keterangan:

- \(Y\) adalah outcome,
- \(X\) adalah predictor,
- \(\beta_0\) adalah intercept,
- \(\beta_1\) adalah slope,
- \(\epsilon\) menunjukkan unexplained variation.

Fitted line dipilih menggunakan metode **least squares**.

```python
import numpy as np
from sklearn.linear_model import LinearRegression

X = np.array([[1], [2], [3], [4], [5]])
y = np.array([52, 55, 61, 65, 70])

model = LinearRegression()
model.fit(X, y)

print("Intercept:", model.intercept_)
print("Slope:", model.coef_[0])
```

Dalam model ini, intercept menunjukkan nilai perkiraan \(Y\) ketika \(X=0\), sedangkan slope menunjukkan perubahan rata-rata pada \(Y\) untuk setiap perubahan satu unit pada \(X\).

## 3. Fitted Values and Residuals

**Fitted value** adalah nilai yang diprediksi oleh model:

\[
\hat{y}_i.
\]

Sedangkan **residual** adalah:

\[
e_i = y_i-\hat{y}_i.
\]

Residual menunjukkan seberapa jauh nilai pengamatan sebenarnya dari nilai yang diprediksi oleh model.

```python
pred = model.predict(X)
residuals = y - pred

print(residuals)
```

Jika residual bernilai positif, berarti nilai aktual lebih besar daripada nilai prediksi. Sebaliknya, residual negatif menunjukkan bahwa nilai aktual lebih kecil daripada nilai prediksi.

## 4. Least Squares

Pendekatan **least squares** memilih coefficients yang dapat meminimalkan residual sum of squares:

\[
RSS = \sum_i(y_i-\hat{y}_i)^2.
\]

Residual dikuadratkan sehingga residual positif dan negatif sama-sama memberikan kontribusi positif. Selain itu, error yang lebih besar akan memiliki pengaruh yang lebih besar terhadap hasil perhitungan.

Dengan cara ini, model berusaha mendapatkan fitted line yang secara keseluruhan memiliki error sekecil mungkin.

## 5. Multiple Linear Regression

Jika terdapat beberapa predictor, model dapat ditulis sebagai:

\[
Y = \beta_0+\beta_1X_1+\beta_2X_2+\cdots+\beta_pX_p+\epsilon.
\]

Sebagai contoh:

```python
import pandas as pd
from sklearn.linear_model import LinearRegression

df = pd.DataFrame({
    "study_hours": [2, 4, 5, 7, 8, 9],
    "attendance": [70, 80, 82, 90, 92, 95],
    "score": [55, 62, 68, 78, 84, 88]
})

X = df[["study_hours", "attendance"]]
y = df["score"]

model = LinearRegression()
model.fit(X, y)

print(model.intercept_)
print(model.coef_)
```

Setiap coefficient menggambarkan hubungan predictor dengan outcome ketika predictor lainnya dianggap tetap (**holding the other predictors fixed**).

Sebagai contoh, coefficient untuk `study_hours` menunjukkan hubungan antara jumlah jam belajar dan score dengan mempertahankan `attendance` tetap.

## 6. Assessing a Model

Beberapa ukuran yang umum digunakan untuk menilai model antara lain:

- residual analysis,
- residual standard error,
- \(R^2\),
- RMSE,
- dan cross-validation.

RMSE dirumuskan sebagai:

\[
RMSE = \sqrt{\frac{1}{n}\sum_i(y_i-\hat{y}_i)^2}.
\]

Contoh penggunaannya dalam Python:

```python
from sklearn.metrics import mean_squared_error
import numpy as np

pred = model.predict(X)
rmse = np.sqrt(mean_squared_error(y, pred))

print("RMSE:", rmse)
```

RMSE menunjukkan seberapa besar rata-rata error prediksi model dalam satuan yang sama dengan outcome. Semakin kecil nilai RMSE, semakin kecil error prediksi yang dihasilkan model pada data yang digunakan.

## 7. Cross-Validation

**Cross-validation** digunakan untuk mengevaluasi bagaimana model bekerja pada data yang tidak digunakan ketika proses fitting dilakukan.

```python
from sklearn.model_selection import cross_val_score

scores = cross_val_score(
    LinearRegression(),
    X,
    y,
    cv=5,
    scoring="neg_mean_squared_error"
)

print(np.sqrt(-scores))
```

Cross-validation sangat berguna ketika ingin membandingkan beberapa predictive models dan mendeteksi **overfitting**.

Pada dasarnya, metode ini membantu melihat apakah performa model tetap baik ketika model diberikan data yang tidak digunakan saat training.

## 8. Prediction and Extrapolation

Sebuah model dapat melakukan **interpolation**, yaitu membuat prediksi dalam range data yang sudah diamati. Namun, ketika model digunakan untuk membuat prediksi jauh di luar range tersebut, kondisi ini disebut **extrapolation**.

Extrapolation dapat berisiko karena hubungan yang terlihat pada training range belum tentu tetap berlaku di luar range tersebut.

Oleh karena itu, prediksi di luar range data yang tersedia perlu dilakukan dengan hati-hati.

## 9. Factor Variables

Categorical variables dapat direpresentasikan menggunakan **indicator variables** atau **dummy variables**.

```python
import pandas as pd

data = pd.DataFrame({
    "hours": [2, 4, 5, 7],
    "major": ["CE", "CE", "EE", "EE"],
    "score": [55, 65, 70, 80]
})

X = pd.get_dummies(data[["hours", "major"]], drop_first=True)
y = data["score"]
```

Salah satu category digunakan sebagai **reference level**, sedangkan coefficients dari category lainnya menunjukkan perbedaan relatif terhadap reference tersebut.

Dengan pendekatan ini, categorical variables dapat dimasukkan ke dalam regression model meskipun datanya tidak berbentuk angka secara langsung.

## 10. Correlated Predictors and Multicollinearity

Predictors dapat memiliki hubungan atau correlation satu sama lain. Jika correlation antar-predictor terlalu kuat, individual regression coefficients dapat menjadi tidak stabil dan sulit untuk diinterpretasikan.

Masalah ini disebut **multicollinearity**.

Dalam kondisi seperti ini, sebuah model masih dapat memberikan hasil prediction yang baik. Namun, interpretasi terhadap masing-masing coefficient dapat menjadi sulit.

Artinya, kemampuan model dalam melakukan prediction dan kemampuan model dalam menjelaskan hubungan setiap predictor tidak selalu sama.

## 11. Confounding

**Confounding variable** adalah variable yang memiliki hubungan dengan predictor dan outcome sekaligus sehingga dapat mengubah atau mendistorsi hubungan yang terlihat antara keduanya.

Regression dapat digunakan untuk mengontrol measured variables tertentu. Namun, regression saja tidak secara otomatis membuktikan adanya **causation**.

Dengan kata lain, menemukan hubungan antara dua variables melalui regression tidak berarti bahwa salah satu variable pasti menyebabkan perubahan pada variable lainnya.

## 12. Interactions

**Interaction** terjadi ketika effect dari suatu predictor bergantung pada predictor lainnya.

Sebagai contoh:

\[
Y=\beta_0+\beta_1X_1+\beta_2X_2+\beta_3X_1X_2+\epsilon.
\]

Interaction coefficient menunjukkan bagaimana hubungan yang berkaitan dengan satu variable dapat berubah bergantung pada nilai variable lainnya.

Hal ini berguna ketika pengaruh suatu predictor terhadap outcome tidak sama untuk semua kondisi predictor lainnya.

## 13. Regression Diagnostics

Beberapa diagnostics penting dalam regression antara lain:

- unusual residuals,
- outliers,
- influential observations,
- heteroskedasticity,
- non-normal residual patterns,
- correlated errors,
- dan nonlinearity.

Residual plot dapat dibuat menggunakan kode berikut:

```python
import matplotlib.pyplot as plt

plt.scatter(pred, residuals)
plt.axhline(0)
plt.xlabel("Fitted value")
plt.ylabel("Residual")
plt.title("Residual Plot")
plt.show()
```

Pola tertentu yang muncul pada residual dapat menunjukkan bahwa model belum menangkap suatu structure penting dalam data.

Karena itu, regression diagnostics penting dilakukan untuk mengetahui apakah terdapat masalah pada model yang digunakan.

## 14. Polynomial and Spline Regression

Linear regression tidak selalu berarti bahwa hubungan antara predictor dan outcome harus terlihat sebagai garis lurus ketika **polynomial** atau **spline terms** digunakan.

Polynomial model dapat memasukkan:

\[
X,\ X^2,\ X^3,\ldots
\]

Dengan menambahkan polynomial terms, model dapat menggambarkan hubungan yang lebih kompleks antara predictor dan outcome.

**Splines** membagi range predictor menjadi beberapa region dan kemudian membuat smooth functions pada region tersebut.

Kedua metode ini memungkinkan model menangani hubungan yang lebih fleksibel, tetapi tetap menggunakan regression framework.

## 15. Generalized Additive Models

**Generalized additive models (GAMs)** mengembangkan konsep regression dengan memungkinkan penggunaan smooth functions dari predictor:

\[
Y = \beta_0+f_1(X_1)+f_2(X_2)+\cdots+\epsilon.
\]

GAMs berguna ketika hubungan antara predictor dan outcome bersifat nonlinear, tetapi masih dapat direpresentasikan menggunakan additive structure.

Dengan demikian, GAM dapat memberikan model yang lebih fleksibel dibandingkan linear regression biasa ketika hubungan dalam data tidak berbentuk garis lurus.

## 16. Conclusion

Regression memberikan framework yang dapat digunakan untuk prediction maupun menjelaskan hubungan antara predictor dan numerical outcome.

**Simple regression** memperkenalkan hubungan antara satu predictor dan satu outcome, sedangkan **multiple regression** memungkinkan beberapa predictors dianalisis secara bersamaan.

Namun, regression coefficients harus diinterpretasikan dengan hati-hati karena beberapa faktor seperti correlation antar-predictors, confounding, outliers, nonlinearity, dan extrapolation dapat memengaruhi kesimpulan yang diperoleh.

Workflow regression yang baik tidak hanya bergantung pada proses fitting model atau satu angka performa saja. Model juga perlu dianalisis menggunakan **diagnostics** dan **validation** agar kita dapat mengetahui apakah model bekerja dengan baik dan apakah terdapat masalah pada data atau asumsi model.
