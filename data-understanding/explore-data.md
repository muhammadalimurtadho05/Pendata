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
# Eksplorasi Data
Eksplorasi data (atau _Exploratory Data Analysis / EDA_) adalah proses mengeksplorasi dan memahami data sebelum dilakukan analisis lebih lanjut atau pembuatan model.
Eksplorasi data bertujuan untuk mengetahui pola dalam data, distribusi data, hubungan antar variabel/atribut, nilai yang hilang (_Missing Values_) dan _Outlier_.

# Studi Kasus
## Visualisasi Data
### Kelas
Kelas _(class)_ adalah kategori atau label target dalam sebuah dataset yang digunakan dalam proses klasifikasi.
Dari dataset yang telah diambil sebelumnya, terdapat 3 kelas pada kolom species, yaitu sebagai berikut
1. Iris-setosa
2. Iris-versicolor
3. Iris-virginica

Berikut merupakan Histogram untuk kelas tersebut
```{figure} ../img/distribusi-frekuensi-species.png
---
width: 60%
align: center
---
Histogram Kelas
```
### Distribusi Frekuensi
Berikut merupakan histogram distribusi frekuensi untuk 5 fitur yang ada pada Iris Flower Dataset, Visualisasi ini dilakukan pada software Orange Data Mining
<!-- Figure -->
```{figure} ../img/distribusi-frekuensi-sepal_length.png
---
width: 60%
align: center
---
Histogram Distribusi Frekuensi Fitur sepal_length
```
```{figure} ../img/distribusi-frekuensi-sepal_width.png
---
width: 60%
align: center
---
Histogram Distribusi Frekuensi Fitur sepal_width
```
```{figure} ../img/distribusi-frekuensi-petal_length.png
---
width: 60%
align: center
---
Histogram Distribusi Frekuensi Fitur petal_length
```
```{figure} ../img/distribusi-frekuensi-petal_width.png
---
width: 60%
align: center
---
Histogram Distribusi Frekuensi Fitur petal_width
```
### Scatter Plot
Scatter plot adalah grafik yang digunakan untuk menampilkan hubungan antara dua variabel numerik dalam bentuk titik-titik pada bidang koordinat.
Untuk menampilkan Scatter plot pada Tools Orange Data Mining, kita dapat menggunakan widget **Scatter Plot**
```{figure} ../img/eksplorasi-data/scatter-plot-widget.png
---
width: 60%
align: center
---
Widget Scatter Plot
```
```{figure} ../img/eksplorasi-data/scatter-plot.png
---
width: 60%
align: center
---
Scatter Plot
```

Berikut jika diemplementasikan menggunakan code program Python
```{code-cell}
:tags: [hide-input]
import pandas as pd
import matplotlib.pyplot as plt
df=pd.read_csv("../data/IRIS.csv")
print("Korelasi petal_length dan petal_width: ",round(df['petal_width'].corr(df['petal_length']), 2))
plt.scatter(df['petal_length'], df['petal_width'])
plt.xlabel('petal_length')
plt.ylabel('petal_width')
plt.title('Scatter Plot petal_length vs petal_width')
plt.show()
```

Berdasarkan scatter plot antara petal_length dan petal_width, terlihat adanya korelasi positif yang sangat kuat dengan nilai r = 0,96. Hal ini menunjukkan bahwa semakin tinggi nilai petal_length, maka petal_width juga cenderung meningkat secara linear.
```{note}
Koefisien korelasi menunjukkan arah dan kekuatan hubungan antar variabel. Nilai positif menandakan hubungan searah, artinya jika satu variabel meningkat maka variabel lainnya juga meningkat. Nilai negatif menunjukkan hubungan berlawanan arah, yaitu ketika satu variabel meningkat maka variabel lainnya menurun. Semakin mendekati +1 atau −1, maka hubungan tersebut semakin kuat.
```

### Outlier
Outlier adalah data yang memiliki nilai sangat berbeda atau menyimpang jauh dari sebagian besar data lainnya dalam suatu dataset. Nilai ini bisa jauh lebih tinggi atau lebih rendah dibanding pola umum yang terbentuk. Outlier dapat muncul karena kesalahan pencatatan, kesalahan pengukuran, atau memang merupakan kejadian langka yang nyata. Keberadaan outlier penting untuk diperhatikan karena dapat memengaruhi hasil analisis, terutama pada perhitungan rata-rata dan pembuatan model statistik.

Untuk menampilkan Outlier pada Tools Orange Data Mining, kita dapat menggunakan widget **Outliers**
```{figure} ../img/eksplorasi-data/outlier-widget.png
---
width: 60%
align: center
---
Widget Outlier
```
```{figure} ../img/eksplorasi-data/outlier.png
---
width: 60%
align: center
---
Outlier
```