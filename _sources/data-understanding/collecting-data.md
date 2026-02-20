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

# Collecting Data
Collecting data adalah proses mengumpulkan data atau informasi yang relevan untuk menjawab suatu pertanyaan, melakukan analisis, atau mendukung pengambilan keputusan. Collecting data merupakan langkah awal dalam penelitian atau analisis dimana kita mengambil informasi, mencatat fakta, mengumpulkan angka atau observasi maupun mendokumentasikan kejadian.
## Tujuan
1. Mendapatkan informasi yang akurat.
2. Menjawab pertanyaan penelitian.
3. Menguji hipotesis.
4. Mendukung keputusan.

## Contoh dalam Penambangan Data
1. Mengambil data penjualan dari database.
2. Mengunduh dataset dari internet.
3. Mengumpulkan data pelanggan dari survei.
4. Scrapping data dari internet.

## Studi Kasus
Pada studi kasus ini saya menggunakan Iris Flower Dataset yang didapatkan dari https://www.kaggle.com/datasets/arshid/iris-flower-dataset

Dari sumber dataset tersebut didapatkan sebanyak 5 fitur dengan 150 data, diantaranya sebagai berikut :
```{code-cell}
import pandas as pd
df = pd.read_csv("../data/IRIS.csv")
df.head(20)
```

Dari sumber dataset tersebut didapatkan sebanyak 5 fitur dengan 150 data, diantaranya sebagai berikut :
```{code-cell}
import pandas as pd
df = pd.read_csv("../data/IRIS.csv")
df.head(20)
```