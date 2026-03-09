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
# Campuran
Cara menghitung tipe data campuran adalah dengan menghitung jarak tiap jenis data secara terpisah lalu dijumlahkan, sehingga didapatkan seperti berikut

## Jarak Numerik (Euclidean)
```{code-cell}
import pandas as pd
import numpy as np
df = pd.read_csv("../../data/Churn_Modelling.csv")
numeric_cols = [
    'CreditScore',
    'Age',
    'Tenure', 
    'Balance', 
    'NumOfProducts', 
    'EstimatedSalary'
]
df_numeric = df[numeric_cols]
point1 = df_numeric.iloc[0]
point2 = df_numeric.iloc[1]

euclidean_distance = np.sqrt(np.sum((point1 - point2)**2))
print(euclidean_distance)
```

## Binary
```{code-cell}
from scipy.spatial.distance import jaccard
binary_cols = [
    'Gender', 
    'HasCrCard', 
    'IsActiveMember', 
    'Exited'
]
df_binary = df[binary_cols]
p1 = df_binary.iloc[0]
p2 = df_binary.iloc[1]
jaccard_distance = jaccard(p1, p2)

print(jaccard_distance)
```
## Kategorikal
```{code-cell}

cols = ["Geography"]

p1 = df.loc[0, cols].values
p2 = df.loc[1, cols].values

distance = np.mean(p1 != p2)

print(distance)
```

## Hasil
Setelah semua jenis data dihitung jaraknya secara terpisah, kita tinggal menjumlahkan semua jarak tersebut sehingga didapatkan hasil sebagai berikut

```{code-cell}
hasil = euclidean_distance + jaccard_distance + distance
print(hasil)
```

```{note}
Pada implementasi diatas, objek yang digunakan adalah objek pertama dan kedua
```