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

# Decimal Scaling
Decimal scaling adalah salah satu teknik normalisasi data yang dilakukan dengan cara menggeser titik desimal dari nilai-nilai pada suatu atribut.

Tujuan utamanya adalah mengubah skala nilai data sehingga seluruh nilai tersebut berada dalam rentang antara -1 hingga 1. Teknik ini sangat berguna dalam tahap preprocessing data mining atau machine learning, terutama jika algoritma yang digunakan sensitif terhadap perbedaan skala antar variabel

Rumus untuk menghitung Decimal Scaling adalah sebagai berikut

$$
v' = \frac{v}{10^j}
$$

Dimana
- $v'$ = Nilai hasil normalisasi
- $v$ = Nilai asli sebelum dilakukan normalisasi
- $j$ = jumlah digit dari nilai absolut terbesar

Contoh

| No. | IPK |   PO  | JML |
|:---:|:---:|:-----:|:---:|
|  1. |  2  |2000000|  2  |
|  2. |  3  |3000000|  3  |
|  3. |  4  |2000000|  2  |
|  4. |  2  |2000000|  3  |
|  5. |  3  |3000000|  2  |
|  6. |  4  |4000000|  3  |

Pada data diatas akan dilakukan `Decimal Scaling` pada kolom PO

Pada data tersebut, nilai absolut terbesar pada kolom PO adalah 4000000 yang memiliki 7 digit angka sehingga, $j = 7$.

Berikut merupakan perhitungan Decimal Scaling pada kolom PO

$$
v' &= \frac{v}{10^j}\\
\\
10^7 &= 10000000\\
\\
v1 &= \frac{2000000}{10000000} = 0.2 \\
\\
v2 &= \frac{3000000}{10000000} = 0.3 \\
\\
v3 &= \frac{2000000}{10000000} = 0.2\\
\\
v4 &= \frac{2000000}{10000000} = 0.2 \\
\\
v5 &= \frac{3000000}{10000000} = 0.3 \\
\\
v6 &= \frac{4000000}{10000000} = 0.4
$$

```{code-cell}
:tags: [hide-input]
import numpy as np

data = {
    'No': [1, 2, 3, 4, 5, 6],
    'IPK': [2, 3, 4, 2, 3, 4],
    'PO': [2000000, 3000000, 2000000, 2000000, 3000000, 4000000],
    'JML': [2, 3, 2, 3, 2, 3]
}

po_array = np.array(data['PO'])

max_val = np.max(np.abs(po_array))

j = len(str(int(max_val)))

po_scaled = po_array / (10**j)

print(f"Data Asli PO: {data['PO']}")
print(f"Nilai j: {j}")
print(f"Hasil Decimal Scaling: {po_scaled}")

```