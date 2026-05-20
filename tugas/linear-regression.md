---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Regresi Linear

## Contents

- [Dataset](#dataset)
- [Perhitungan Manual](#perhitungan-manual)
  - [Matriks X dan Y](#matriks-x-dan-y)
  - [Menghitung $X^T X$](#menghitung-xtx)
  - [Menghitung $X^T Y$](#menghitung-xty)
  - [Menghitung Invers $(X^T X)^{-1}$](#menghitung-invers)
  - [Menghitung $\hat{\beta}$](#menghitung-beta)
- [Implementasi Python](#implementasi-python)
  - [Visualisasi Scatter Plot](#visualisasi-scatter-plot)
  - [Model Regresi Linear](#model-regresi-linear)
  - [Visualisasi Garis Regresi](#visualisasi-garis-regresi)

---

## Dataset

Data yang digunakan terdiri dari 7 observasi dengan 1 variabel independen $x$ dan 1 variabel dependen $y$.

| $i$ | $x$ | $y$ |
| :---: | :---: | :---: |
| 1 | 2 | 2 |
| 2 | 4 | 3 |
| 3 | 3 | 5 |
| 4 | 3 | 4 |
| 5 | 3 | 3 |
| 6 | 4 | 5 |
| 7 | 5 | 6 |

---

## Perhitungan Manual

Persamaan regresi linear sederhana memiliki bentuk:

$$\hat{y} = \beta_0 + \beta_1 x$$

Koefisien $\hat{\beta}$ dihitung menggunakan metode **Ordinary Least Squares (OLS)** dengan rumus:

$$\hat{\beta} = (X^T X)^{-1} X^T Y$$

---

### Matriks X dan Y

Matriks $X$ dibentuk dengan menambahkan kolom konstanta 1 untuk menghitung intercept $\beta_0$:

$$X = \begin{bmatrix} 1 & 2 \\ 1 & 4 \\ 1 & 3 \\ 1 & 3 \\ 1 & 3 \\ 1 & 4 \\ 1 & 5 \end{bmatrix}, \qquad Y = \begin{bmatrix} 2 \\ 3 \\ 5 \\ 4 \\ 3 \\ 5 \\ 6 \end{bmatrix}$$

---

### Menghitung $X^T X$

$$X^T X = \begin{bmatrix} n & \sum x \\ \sum x & \sum x^2 \end{bmatrix}$$

Nilai masing-masing elemen:

$$n = 7$$

$$\sum x = 2 + 4 + 3 + 3 + 3 + 4 + 5 = 24$$

$$\sum x^2 = 4 + 16 + 9 + 9 + 9 + 16 + 25 = 88$$

Sehingga:

$$X^T X = \begin{bmatrix} 7 & 24 \\ 24 & 88 \end{bmatrix}$$

---

### Menghitung $X^T Y$

$$X^T Y = \begin{bmatrix} \sum y \\ \sum xy \end{bmatrix}$$

Nilai masing-masing elemen:

$$\sum y = 2 + 3 + 5 + 4 + 3 + 5 + 6 = 28$$

$$\sum xy = (2)(2) + (4)(3) + (3)(5) + (3)(4) + (3)(3) + (4)(5) + (5)(6) = 4 + 12 + 15 + 12 + 9 + 20 + 30 = 102$$

Sehingga:

$$X^T Y = \begin{bmatrix} 28 \\ 102 \end{bmatrix}$$

---

### Menghitung Invers $(X^T X)^{-1}$

Untuk matriks $2 \times 2$:

$$A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}, \qquad A^{-1} = \frac{1}{\det(A)} \begin{bmatrix} d & -b \\ -c & a \end{bmatrix}$$

Hitung determinan:

$$\det(X^T X) = (7)(88) - (24)(24) = 616 - 576 = 40$$

Sehingga:

$$(X^T X)^{-1} = \frac{1}{40} \begin{bmatrix} 88 & -24 \\ -24 & 7 \end{bmatrix}$$

---

### Menghitung $\hat{\beta}$

$$\hat{\beta} = (X^T X)^{-1} X^T Y = \frac{1}{40} \begin{bmatrix} 88 & -24 \\ -24 & 7 \end{bmatrix} \begin{bmatrix} 28 \\ 102 \end{bmatrix}$$

**Menghitung $\beta_0$ (intercept):**

$$\beta_0 = \frac{(88)(28) + (-24)(102)}{40} = \frac{2464 - 2448}{40} = \frac{16}{40} = 0.4$$

**Menghitung $\beta_1$ (slope):**

$$\beta_1 = \frac{(-24)(28) + (7)(102)}{40} = \frac{-672 + 714}{40} = \frac{42}{40} = 1.05$$

**Persamaan regresi linear yang diperoleh:**

$$\boxed{\hat{y} = 0.4 + 1.05x}$$

---

## Implementasi Python

Berikut implementasi regresi linear menggunakan Python dengan library `scikit-learn` dan `matplotlib`.

```{code-cell}
import pandas as pd
import numpy as np
from sklearn.linear_model import LinearRegression
import seaborn as sns
import matplotlib.pyplot as plt

x = [2, 4, 3, 3, 3, 4, 5]
y = [2, 3, 5, 4, 3, 5, 6]
```

---

### Visualisasi Scatter Plot

Sebelum membuat model, data divisualisasikan terlebih dahulu menggunakan scatter plot untuk melihat pola hubungan antara $x$ dan $y$.

```{code-cell}
plt.figure(figsize=(8, 6))
sns.scatterplot(x=x, y=y, color='blue', label='Data Points')
plt.title('Scatter Plot of X vs Y')
plt.xlabel('X (Independent Variable)')
plt.ylabel('Y (Dependent Variable)')
plt.grid(True)
plt.legend()
plt.show()
```

```{figure} ../img/linear-regression/scatter-plot.png
:name: scatter-plot-lr
:align: center

Scatter Plot X vs Y
```

---

### Model Regresi Linear

Model regresi linear dibangun menggunakan `LinearRegression` dari `scikit-learn`.

```{code-cell}
# Reshape x menjadi array 2D
X_reshaped = np.array(x).reshape(-1, 1)

# Buat dan latih model
model = LinearRegression()
model.fit(X_reshaped, y)

# Ambil koefisien hasil training
slope = model.coef_[0]
intercept = model.intercept_

print(f"Linear Regression Equation: y = {slope:.2f}x + {intercept:.2f}")
```

Output:

```
Linear Regression Equation: y = 1.05x + 0.40
```

Hasil ini **konsisten** dengan perhitungan manual yang menghasilkan:

$$\hat{y} = 0.4 + 1.05x$$

---

### Visualisasi Garis Regresi

Garis regresi ditambahkan ke scatter plot untuk memperlihatkan hasil model secara visual.

```{code-cell}
y_pred = model.predict(X_reshaped)

plt.figure(figsize=(8, 6))
sns.scatterplot(x=x, y=y, color='blue', label='Data Points')
plt.plot(x, y_pred, color='red', label=f'Regression Line: y = {slope:.2f}x + {intercept:.2f}')
plt.title('Linear Regression of X vs Y')
plt.xlabel('X (Independent Variable)')
plt.ylabel('Y (Dependent Variable)')
plt.grid(True)
plt.legend()
plt.show()
```

```{figure} ../img/linear-regression/line.png
:name: regression-line-lr
:align: center

Garis Regresi Linear pada Data
```