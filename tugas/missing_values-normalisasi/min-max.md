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
# Min-Max Normalization
Mengubah nilai data ke rentang tertentu (biasanya 0–1).

$$
x' = \frac{x - x_{min}}{x_{max} - x_{min}}
$$

Contoh

| No. | IPK |   PO  | JML |
|:---:|:---:|:-----:|:---:|
|  1. |  2  |2000000|  2  |
|  2. |  3  |3000000|  3  |
|  3. |  4  |2000000|  2  |
|  4. |  2  |2000000|  3  |
|  5. |  3  |3000000|  2  |
|  6. |  4  |4000000|  3  |
|  7. |  2  |3000000|  ?  |

Pada data diatas akan dilakukan `Min-Max Normalization` pada kolom IPK, PO dan JML

Berikut merupakan sampel perhitungan manual pada kolom IPK objek kedua

$$
x' = \frac{3 - 2}{4 - 2} = \frac{1}{2} = 0.5
$$

Berikut merupakan sampel perhitungan manual pada kolom PO objek kedua

$$
x' = \frac{3000000 - 2000000}{4000000 - 2000000} = \frac{1000000}{2000000} = 0.5
$$

Berikut merupakan sampel perhitungan manual pada kolom JML objek kedua

$$
x' = \frac{3 - 2}{3 - 2} = \frac{1}{1} = 1
$$

Setelah semua kolom dan objek dilakukan `Min-Max Normalization` maka akan didapatkan hasil sebagai berikut

| No. | IPK |   PO  | JML |
|:---:|:---:|:-----:|:---:|
|  1. |  0  |   0   |  0  |
|  2. | 0.5 |  0.5  |  1  |
|  3. |  1  |   0   |  0  |
|  4. |  0  |   0   |  1  |
|  5. | 0.5 |  0.5  |  0  |
|  6. |  1  |   1   |  1  |
|  7. |  0  |  0.5  |  ?  |

Rumus diatas apabila diimplementasikan kedalam Python adalah sebagai berikut

```{code-cell}
:tags: [hide-input]
import pandas as pd

data = {
    'No': [1, 2, 3, 4, 5, 6],
    'IPK': [2, 3, 2, 3, 2, 3],
    'PO': [2000000, 3000000, 2000000, 2000000, 3000000, 4000000],
    'JML': [2, 3, 2, 3, 2, 3]
}
df = pd.DataFrame(data)
kolom = ['IPK', 'PO', 'JML']
data_normalize = pd.DataFrame(data)
for col in kolom:
    min_val = df[col].min()
    max_val = df[col].max()
    
    data_normalize[col] = (df[col] - min_val) / (max_val - min_val)

print("Data Sebelum Dinormalisasi")
print(df)
print()
print("Data Setelah Dinormalisasi")
print(data_normalize)

```