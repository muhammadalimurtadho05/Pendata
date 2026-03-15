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
# WKNN
WKNN(_Weighted K-Nearest Neighbor_) adalah pengembangan dari algoritma _K-Nearest Neighbors_ (KNN) yang memberikan bobot berbeda pada setiap tetangga terdekat.

Imputasi menggunakan WKNN digunakan untuk mengisi nilai yang hilang (missing value) pada dataset dengan memanfaatkan data tetangga terdekat dan memberikan bobot berdasarkan jarak.

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

## Normalisasi
Pada dataset tersebut, terdapat satu buah _missing value_ pada objek ke-7 kolom JML. Sebelum melakukan imputasi menggunakan WKNN, data terlebih dahulu dinormalisasi. Pada kasus ini, saya menggunakan metode normalisasi `Min-Max Normalization` sehingga didapatkan tabel seperti berikut

| No. | IPK |   PO  | JML |
|:---:|:---:|:-----:|:---:|
|  1. |  0  |   0   |  0  |
|  2. | 0.5 |  0.5  |  1  |
|  3. |  1  |   0   |  0  |
|  4. |  0  |   0   |  1  |
|  5. | 0.5 |  0.5  |  0  |
|  6. |  1  |   1   |  1  |
|  7. |  0  |  0.5  |  ?  |

## Kemiripan
Setelah data dinormalisasi, perlu dihitung terlebih dahulu kemiripan / kedekatan berdasarkan jarak antara baris data yang memiliki _Missing Value_ dengan baris tetangganya dengan rumus sebagai berikut

$$
1/s_i = \sum_{h_i \in O_i \cap O_j} (y_{ih} - y_{jh})^2
$$

Dimana
- $(y_{ih} - y_{jh})^2$ = kuadrat selisih antara nilai atribut pada baris $i$ (data target) dan baris $j$ (tetangga) `(Mirip Euclidean Distance)`.
- $O_i \cap O_j$ = Penjumlahan ini hanya dilakukan pada kolom (atribut) yang sama-sama tersedia (tidak kosong) di kedua baris tersebut `(Irisan)`
- $1/s_i$ = Semakin kecil nilai jaraknya maka semakin besar nilai $s_i$ nya (semakin mirip).

Menghitung kemiripan objek pertama dan objek kedua pada objek ke-7

$$
1/s_i &= \sum_{h_i \in O_i \cap O_j} (y_{ih} - y_{jh})^2 \\
\\

1/s_1 &= (0-0)^2 + (0.5 - 0)^2 = 0 + 0.25 = 0.25\\
\\
1/0.25 &= 4\\
\\
s_1 &= 4\\
\\

1/s_2 &= (0 - 0.5)^2 + (0.5 - 0.5)^2 = 0.25 + 0 = 0.25\\
\\
1/0.25 &= 4\\
\\
s_2 &= 4
$$

Langkah yang sama dilakukan untuk objek ke-3 sampai objek ke-6 sehingga didapatkan hasil sebagai berikut

$$
s_1 &= 4\\
s_2 &= 4\\
s_3 &= 0.8\\
s_4 &= 4\\
s_5 &= 4\\
s_6 &= 0.8
$$

## Imputasi
Setelah mendapatkan bobot($s_i$) untuk setiap tetangga, kita gunakan rumus berikut untuk mengisi nilai yang kosong.

$$
\hat{y}_{ih} = \frac{\sum_{j \in I_{Kih}} s_i(y_j) y_{jh}}{\sum_{j \in I_{Kih}} s_i(y_j)}
$$

Dimana
- $\hat{y}_{ih}$: Nilai hasil prediksi (imputasi) untuk baris $i$ pada kolom $h$.
- $I_{Kih}$: Himpunan $K$ tetangga terdekat yang memiliki data pada kolom $h$.
- $s_i(y_j) y_{jh}$: Nilai dari tetangga ke-$j$ dikalikan dengan bobotnya. Ini memastikan tetangga yang paling mirip memberikan kontribusi lebih besar.
- Pembagi ($\sum s_i$): Digunakan untuk menormalisasi bobot sehingga hasil akhirnya tetap berada dalam skala yang wajar (rata-rata tertimbang).

Rumus tersebut apabila diterapkan pada dataset diatas, maka akan didapatkan hasil sebagai berikut

$$
\hat{y}_{ih} &= \frac{(0 \times 4)+(1 \times 4)+(0 \times 0.8)+(1 \times 4)+(0 \times 4)+(1 \times 0.8)} {4 + 4 + 0.8 + 4 + 4 + 0.8} \\ 
             \\
             &= \frac{0 + 4 + 0 + 4 + 0 + 0.8}{17.6}\\
             \\
             &= \frac{8.8}{17.6}\\
             \\
             &= 0.5
$$

Jadi nilai prediksi missing value menggunakan WKNN adalah `0.5`, perhitungan diatas apabila diterapkan pada Python akan menghasilkan sebagai berikut

```{code-cell}
:tags: [hide-input]
import pandas as pd
import numpy as np
from sklearn.preprocessing import MinMaxScaler
from sklearn.neighbors import KNeighborsRegressor as KNR

data = {
    'IPK': [2, 3, 4, 2, 3, 4, 2],
    'PO': [2000000, 3000000, 2000000, 2000000, 3000000, 4000000, 3000000],
    'JML': [2, 3, 2, 3, 2, 3, np.nan]
}
df = pd.DataFrame(data)

# Data Training
df_train = df.iloc[:6].copy()

# Data Tes
df_test = df.iloc[6:].copy()

# Normalisasi Min-Max
scaler_features = MinMaxScaler()
scaler_target = MinMaxScaler()

# Normalisasi Fitur (IPK & PO)
train_X = scaler_features.fit_transform(df_train[['IPK', 'PO']])
test_X = scaler_features.transform(df_test[['IPK', 'PO']])

# Normalisasi Target (JML)
train_y = scaler_target.fit_transform(df_train[['JML']])

def custom_weights(distances):
    return 1 / (distances**2)

knn = KNR(n_neighbors=6, weights=custom_weights, metric='euclidean')
knn.fit(train_X, train_y)

pred_scaled = knn.predict(test_X)

pred_final = scaler_target.inverse_transform(pred_scaled)

print(f"Prediksi JML (Ternormalisasi): {pred_scaled[0][0]}")

```