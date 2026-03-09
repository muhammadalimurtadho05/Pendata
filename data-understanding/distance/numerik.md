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
# Numerik
Menghitung jarak data numerik dari salah satu sampel data dibawah ini
```{code-cell}
:tags: [hide-input]
import pandas as pd
import numpy as np
df = pd.read_csv("../../data/Churn_Modelling.csv")
df.head(5)
```

Data diatas didapatkan dari platform [Kaggle](https://www.kaggle.com/datasets/marslinoedward/bank-customer-churn-prediction). Data tersebut memiliki 14 fitur dan 10000 Data.
## Euclidean
Kita ambil sampel data pertama dan kedua untuk menghitung jarak dari kedua data tersebut. implementasi pertama menggunakan Python dengan sintaks `df.select_dtypes(include=[np.number])` dari modul `numpy` untuk memilih fitur numerik yang ada pada dataset tersebut. Fitur dengan tipe data numerik dapat dilihat sebagai berikut
```{code-cell}
df_numeric = df.select_dtypes(include=[np.number])
print(df_numeric.dtypes)
```
Dari fitur diatas, diambil data pertama dan kedua untuk dihitung jaraknya, dan didapatkan hasilnya sebagai berikut 

$$
d(\mathbf{p}, \mathbf{q}) = \sqrt{\sum_{i=1}^{n} (p_i - q_i)^2}
$$

$$
d(\mathbf{1}, \mathbf{2}) = \sqrt{(1-2)^2 + (15634602-15647311)^2 + (619-608)^2 + (42-41)^2 + (2-1)^2 + (0-83807.86)^2 + (1-1)^2 + (1-0)^2 + (1-1)^2 + (101348.88-112542.58)^2 + (1-0)^2}
$$

$$
d(\mathbf{1}, \mathbf{2}) = \sqrt{1+161518681+121+1+1+7023757398+0+1+0+125298919.7+1}
$$

$$
d(\mathbf{1}, \mathbf{2}) = \sqrt{7310575124} = 85501.9013
$$

Rumus diatas apabila diimplementasikan kedalam program Python sebagai berikut
```{code-cell}
:tags: [hide-input]
df_numeric = df.select_dtypes(include=[np.number])
point1 = df_numeric.iloc[0]
point2 = df_numeric.iloc[1]

euclidean_distance = np.sqrt(np.sum((point1 - point2)**2))
print("Jarak Euclidean:", euclidean_distance)
```

Gambar dibawah ini menunjukkan hasil dari implementasi pada Orange Data Mining
![Grafik Data](../../img/distance/euclidean.png)

## Manhattan
Perbedaan rumus antara Euclidean dan Manhattan yaitu hasil pengurangan dari setiap fitur tidak dipangkatkan kemudian tidak di akarkan, sehingga jika dihitung akan mendapatkan hasil sebagai berikut 

$$
    d(\mathbf{x}, \mathbf{y}) = \sum_{i=1}^{n} |x_i - y_i|
$$

$$
    d(\mathbf{1}, \mathbf{2}) = |1-2| + |15634602-15647311| + |619-608| + |42-41| + |2-1| + |0-83807.86| + |1-1| + |1-0| + |1-1| + |101348.88-112542.58| + |1-0|
$$

$$
    d(\mathbf{1}, \mathbf{2}) = 1	+ 12709	+ 11	+ 1 +	1	+ 83807.86	+ 0	+ 1 +	0	+ 11193.7 +	1
$$

$$
d(\mathbf{1}, \mathbf{2}) = 107726.56
$$

Rumus diatas jika diimplementasikan pada Python akan mendapatkan hasil sebagai berikut: 

```{code-cell}
:tags: [hide-input]
manhattan_distance = np.sum(abs(point1 - point2))
print("Jarak Manhattan:", manhattan_distance)
```
Gambar dibawah ini menunjukkan hasil dari implementasi pada Orange Data Mining
![Grafik Data](../../img/distance/manhattan.png)

```{note}
Pada implementasi diatas, data yang digunakan adalah data pertama dan kedua
```