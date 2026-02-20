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
# Data Understanding

Data Understanding adalah tahap awal dalam metode penambangan data untuk mengumpulkan, mengeksplorasi dan memeriksa kualitas data awal. Tujuannya adalah mengenal karakteristik, struktur, tipe serta potensi masalah seperti missing value atau outlier untuk memastikan data relevan untuk menjawab permasalahan bisnis.
Berikut adalah poin-poin utama dari tahapan Data Understanding
1. Mengumpulkan Data Awal: Mendapatkan data dari berbagai sumber dan memahami konteksnya.
2. Mendeskripsikan Data: Memeriksa properti data seperti format, jumlah, dan tipe data.
3. Mengeksplorasi Data: Menggunakan teknik analisis statistik deskriptif dan visualisasi untuk menemukan wawasan awal.
4. Memverifikasi Kualitas Data: Memeriksa konsistensi, kelengkapan, serta menangani missing value atau outlier.
5. Tujuan Akhir: Memastikan data yang digunakan relevan, berkualitas tinggi, dan sesuai untuk menjawab permasalahan bisnis.

```{note}
Data understanding bertindak sebagai landasan analitik untuk memastikan wawasan yang dihasilkan akurat dan tidak terburu-buru, yang dapat menghambat tahap pemodelan.
```

## Pentingnya Memahami Data
Membantu memilih teknik data mining yang tepat, meningkatkan akurasi model prediksi, menghindari kesalahan interpretasi, memastikan hasil yang dapat diandalkan dan dapat ditindaklanjuti.

## Macam Macam Data
1. Data terstruktur (structured)
2. Data tidak terstruktur(unstructured)
3. Data bahasa alami(Natural Language)
4. Data yang dibangkit oleh Mesin (Machined-Generated)
5. Data Audio, Video,Citra
6. Data Streamming
7. Data berbasis Graph(Graph-based)
## Collecting Data
Pada studi kasus ini saya menggunakan Iris Flower Dataset yang didapatkan dari https://www.kaggle.com/datasets/arshid/iris-flower-dataset

Dari sumber dataset tersebut didapatkan sebanyak 5 fitur dengan 150 data, diantaranya sebagai berikut :
```{code-cell}
import pandas as pd
df = pd.read_csv("data/IRIS.csv")
df.head(20)
```
## Describe Data
Dari Dataset tersebut, didapatkan 5 attribute dan kategori attribute sebagai berikut
1. sepal_length (numerik)
2. sepal_width (numerik)
3. petal_length (numerik)
4. petal_width (numerik)
5. species (kategorikal)

Setelah data diambil, kami lakukan visualisasi untuk salah satu fitur yaitu sepal_length pada software orange data mining sehingga didapatkan visual sebagai berikut
![Grafik Data](img/visualisasi-distribusi.png)
![Grafik Data](img/visualisasi-distribusi2.png)

Data  dan fitur yang sama juga diemplementasikan pada python sehingga didapatkan output sebagai berikut
```{code-cell}
import pandas as pd
from scipy import stats
df=pd.read_csv("data/IRIS.csv")
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
## Eksplorasi Data
