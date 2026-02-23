# Eksplorasi Data
Eksplorasi data (atau _Exploratory Data Analysis / EDA_) adalah proses mengeksplorasi dan memahami data sebelum dilakukan analisis lebih lanjut atau pembuatan model.
Eksplorasi data bertujuan untuk mengetahui pola dalam data, distribusi data, hubungan antar variabel/atribut, nilai yang hilang (_Missing Values_) dan _Outlier_.

# Studi Kasus
## Visualisasi Data
### Histogram
Histogram adalah grafik yang digunakan untuk menampilkan distribusi atau penyebaran data numerik dalam bentuk batang (bar).
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
```{figure} ../img/distribusi-frekuensi-species.png
---
width: 60%
align: center
---
Histogram Distribusi Frekuensi Fitur species
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