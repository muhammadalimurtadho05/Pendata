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
# Describe Data
## Apa itu Describe Data
Describe data adalah proses menjelaskan atau merangkum karakteristik suatu dataset menggunakan statistik deskriptif agar kita memahami isi data secara umum.
Biasanya ini dilakukan setelah collecting data dan saat tahap awal eksplorasi data (EDA).

## Tujuan Describe Data
Describe Data bertujuan untuk :
1. Memahami gambaran umum dataset
2. Menemukan data yang tidak normal
3. Menjadi dasar sebelum membuat model
4. Membantu pengambilan keputusan awal

## Studi Kasus
Dari Dataset yang telah diambil sebelumnya, didapatkan 5 attribute dan kategori attribute sebagai berikut:
1. sepal_length (numerik)
2. sepal_width (numerik)
3. petal_length (numerik)
4. petal_width (numerik)
5. species (kategorikal)


Setelah data diambil, kami lakukan visualisasi untuk salah satu fitur yaitu sepal_length pada software orange data mining sehingga didapatkan visual sebagai berikut
![Grafik Data](../img/visualisasi-distribusi.png)
![Grafik Data](../img/visualisasi-distribusi2.png)

Data  dan fitur yang sama juga diemplementasikan pada python sehingga didapatkan output sebagai berikut
```{code-cell}
import pandas as pd
from scipy import stats
df=pd.read_csv("../data/IRIS.csv")
kolom = df['sepal_length']

print("Jumlah data        :", kolom.count())
print("Rata-rata          :", round(kolom.mean(), 2))
print("Nilai minimal      :", kolom.min())
print("Q1                 :", kolom.quantile(0.25))
print("Q2 (Median)        :", kolom.quantile(0.5))
print("Q3                 :", kolom.quantile(0.75))
print("Nilai maksimal     :", kolom.max())
print("Kemencengan (skew) :", round(kolom.skew(), 2))
mode = stats.mode(kolom, keepdims=True)
print("Nilai modus        :", mode.mode[0])
print("Jumlah modus       :", mode.count[0])

print("Standar deviasi    :", round(kolom.std(), 2))
print("Variansi           :", round(kolom.var(), 2))
```

Dari visualisasi yang ditampilkan, maka didapatkan data sebagai berikut
| Statistik      | Nilai  |
|:--------------:|:------:|
| Mean           | 5.84   |
| Modus          | 5.0    |  
| Median         | 5.8    |   
| Min            | 4.3    |
| Max            | 7.9    |
| Missing Values | 0 (0%) | 
```{note}
Pada step ini, hanya diambil 1 (satu) attribute sebagai sampel yaitu attribute **sepal_length**
```