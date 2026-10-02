# Chapter 1 — Exploratory Data Analysis

**Reference:** Peter Bruce, Andrew Bruce, and Peter Gedeck, *Practical Statistics for Data Scientists*, 2nd Edition, O'Reilly Media, 2020.

## 1. Overview

Exploratory Data Analysis (EDA) is the process of examining a dataset before applying statistical models or machine-learning algorithms. The main purpose is to understand the structure, typical values, variability, distributions, unusual observations, and relationships among variables.

The chapter begins with structured data and focuses mainly on rectangular data: rows represent records and columns represent variables or features. It then introduces measures of location and variability, methods for visualizing distributions, categorical variables, correlation, scatterplots, and methods for exploring several variables together.

## 2. Structured Data

Structured data can be represented in a table. Numeric variables can be continuous or discrete, while categorical variables can be binary or ordinal.

- **Continuous:** can take many values within an interval.
- **Discrete:** generally represents counts.
- **Categorical:** represents membership in a finite set of categories.
- **Binary:** has two categories, such as 0/1 or yes/no.
- **Ordinal:** categorical values with a meaningful order.

In data science, a rectangular dataset is often stored as a pandas `DataFrame`.

```python
import pandas as pd

df = pd.DataFrame({
    "age": [20, 22, 25, 28, 31],
    "study_hours": [2, 4, 5, 7, 8],
    "major": ["CE", "CE", "EE", "CE", "EE"],
    "passed": [0, 1, 1, 1, 1]
})

print(df.head())
print(df.info())
```

## 3. Measures of Location

A measure of location describes a typical or central value.

### Mean

The arithmetic mean is

\[
\bar{x} = \frac{1}{n}\sum_{i=1}^{n}x_i.
\]

```python
import numpy as np

x = np.array([10, 12, 13, 15, 20])

print("Mean:", np.mean(x))
```

The mean uses every observation, but it can be strongly affected by extreme values.

### Median

The median is the middle value after sorting the observations. It is more resistant to outliers than the mean.

```python
print("Median:", np.median(x))
```

### Trimmed Mean

A trimmed mean removes a specified proportion of the smallest and largest observations before calculating the mean. It provides a compromise between the ordinary mean and more robust measures such as the median.

```python
from scipy.stats import trim_mean

print("10% trimmed mean:", trim_mean(x, 0.1))
```

### Weighted Mean

When observations have different importance or represent different population proportions, a weighted mean can be appropriate.

```python
weights = np.array([1, 1, 2, 2, 3])
print(np.average(x, weights=weights))
```

## 4. Measures of Variability

Location alone does not describe a dataset. Two datasets may have the same mean but very different amounts of spread.

### Variance and Standard Deviation

Sample variance is

\[
s^2 = \frac{\sum_{i=1}^{n}(x_i-\bar{x})^2}{n-1}.
\]

Standard deviation is

\[
s = \sqrt{s^2}.
\]

```python
print("Variance:", np.var(x, ddof=1))
print("Standard deviation:", np.std(x, ddof=1))
```

### Interquartile Range

The interquartile range is

\[
IQR = Q_{75}-Q_{25}.
\]

It describes the spread of the middle 50% of observations and is less sensitive to extreme values than the range.

```python
q1 = np.percentile(x, 25)
q3 = np.percentile(x, 75)
print("IQR:", q3 - q1)
```

### Median Absolute Deviation

The median absolute deviation (MAD) is based on distances from the median and is a robust measure of variability.

```python
from statsmodels.robust.scale import mad

print("MAD:", mad(x))
```

## 5. Exploring Distributions

A distribution shows how values are spread across their possible range.

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

A boxplot provides a compact view of the median, quartiles, and potential extreme observations.

```python
plt.boxplot(x)
plt.ylabel("Value")
plt.title("Boxplot")
plt.show()
```

Percentiles are especially useful for describing skewed data because they do not require the distribution to be symmetric.

## 6. Categorical Data

For categorical variables, frequency tables are often more informative than means.

```python
print(df["major"].value_counts())
print(df["passed"].value_counts(normalize=True))
```

The **mode** is the most frequently occurring category.

For a binary variable, the proportion of observations in each category can be interpreted as an empirical probability.

## 7. Correlation

Correlation measures the strength and direction of association between numerical variables.

Pearson's correlation coefficient is

\[
r =
\frac{\sum_i(x_i-\bar{x})(y_i-\bar{y})}
{\sqrt{\sum_i(x_i-\bar{x})^2}\sqrt{\sum_i(y_i-\bar{y})^2}}.
\]

Its value ranges from -1 to +1.

```python
print(df[["age", "study_hours", "passed"]].corr())
```

A positive correlation means that larger values of one variable tend to be associated with larger values of the other. A negative correlation indicates the opposite pattern.

Correlation should not automatically be interpreted as causation. A third variable, selection effects, or other mechanisms may explain an observed association.

## 8. Scatterplots

Scatterplots are useful for examining relationships between two numerical variables.

```python
plt.scatter(df["study_hours"], df["age"])
plt.xlabel("Study Hours")
plt.ylabel("Age")
plt.title("Study Hours vs Age")
plt.show()
```

Visual inspection can reveal nonlinear relationships, clusters, outliers, and changing variability.

## 9. Exploring Multiple Variables

Real datasets contain many variables. Useful tools include:

- correlation matrices,
- grouped summaries,
- scatterplots,
- boxplots,
- hexagonal binning,
- contour plots,
- and multivariable visualizations.

Example:

```python
import seaborn as sns

sns.pairplot(df[["age", "study_hours", "passed"]])
plt.show()
```

## 10. Practical EDA Workflow

A practical workflow is:

1. Inspect the rows and columns.
2. Identify variable types.
3. Check missing values and unusual observations.
4. Calculate measures of location.
5. Calculate measures of variability.
6. Visualize distributions.
7. Examine relationships among variables.
8. Investigate possible outliers.
9. Only then proceed to statistical modeling.

## 11. Conclusion

The main lesson of this chapter is that statistical analysis should begin with understanding the data. Mean and standard deviation provide useful numerical summaries, while median, percentiles, and MAD can be more robust when outliers or skewed distributions are present. Graphical methods such as histograms, boxplots, and scatterplots reveal patterns that a single statistic may hide.

EDA is therefore not merely a preliminary step. It helps determine which statistical methods and machine-learning models are appropriate for the problem.

## 12. References

Bruce, P., Bruce, A., & Gedeck, P. (2020). *Practical Statistics for Data Scientists* (2nd ed.). O'Reilly Media.
