# Random Forest

## Contents

- [Dataset](#dataset)
- [Implementasi Pada KNIME](#implementasi-pada-knime)
  - [Workflow](#workflow)
  - [Partisi](#partisi)
  - [Preprocessing: Category to Number](#preprocessing-category-to-number)
  - [Column Filter](#column-filter)
  - [Training: Python Script (Random Forest)](#training-python-script-random-forest)
  - [Testing: Python Script (Prediksi)](#testing-python-script-prediksi)
  - [Scorer](#scorer)

---

## Dataset

Dataset yang digunakan adalah **Mushroom Classification Dataset**, sama dengan yang digunakan pada implementasi Decision Tree sebelumnya. Dataset ini berisi deskripsi sampel hipotetis dari 23 spesies jamur berjenis insang (*gilled mushrooms*) dari famili Agaricus dan Lepiota.

**Link:** [Mushroom Classification Dataset](https://www.kaggle.com/datasets/uciml/mushroom-classification)

Dataset memiliki:

- **8.124 baris** data
- **22 fitur** bertipe kategorikal
- **1 label** bernama `class` dengan 2 nilai:
  - `e` → *Edible* (dapat dimakan)
  - `p` → *Poisonous* (beracun)

Berikut seluruh fitur beserta nilai kategorinya:

| No | Nama Fitur | Deskripsi | Nilai Kategori |
| --- | --- | --- | --- |
| 1 | cap-shape | Bentuk tudung | bell, conical, convex, flat, knobbed, sunken |
| 2 | cap-surface | Permukaan tudung | fibrous, grooves, scaly, smooth |
| 3 | cap-color | Warna tudung | brown, buff, cinnamon, gray, green, pink, purple, red, white, yellow |
| 4 | bruises | Ada memar? | bruises, no |
| 5 | odor | Bau jamur | almond, anise, creosote, fishy, foul, musty, none, pungent, spicy |
| 6 | gill-attachment | Cara insang menempel | attached, descending, free, notched |
| 7 | gill-spacing | Jarak antar insang | close, crowded, distant |
| 8 | gill-size | Ukuran insang | broad, narrow |
| 9 | gill-color | Warna insang | black, brown, buff, chocolate, gray, green, orange, pink, purple, red, white, yellow |
| 10 | stalk-shape | Bentuk batang | enlarging, tapering |
| 11 | stalk-root | Akar batang | bulbous, club, cup, equal, rhizomorphs, rooted, missing(?) |
| 12 | stalk-surface-above-ring | Permukaan batang atas ring | fibrous, scaly, silky, smooth |
| 13 | stalk-surface-below-ring | Permukaan batang bawah ring | fibrous, scaly, silky, smooth |
| 14 | stalk-color-above-ring | Warna batang atas ring | brown, buff, cinnamon, gray, orange, pink, red, white, yellow |
| 15 | stalk-color-below-ring | Warna batang bawah ring | brown, buff, cinnamon, gray, orange, pink, red, white, yellow |
| 16 | veil-type | Tipe selubung | partial, universal |
| 17 | veil-color | Warna selubung | brown, orange, white, yellow |
| 18 | ring-number | Jumlah cincin | none, one, two |
| 19 | ring-type | Tipe cincin | cobwebby, evanescent, flaring, large, none, pendant, sheathing, zone |
| 20 | spore-print-color | Warna cetak spora | black, brown, buff, chocolate, green, orange, purple, white, yellow |
| 21 | population | Pola pertumbuhan | abundant, clustered, numerous, scattered, several, solitary |
| 22 | habitat | Habitat tumbuh | grasses, leaves, meadows, paths, urban, waste, woods |

---

## Implementasi Pada KNIME

Workflow ini dirancang menggunakan KNIME untuk membangun model klasifikasi **Random Forest** berbasis Python (`scikit-learn`). Tujuannya adalah menentukan apakah suatu jenis jamur dapat dimakan atau beracun.

### Workflow

```{figure} ../img/random-forest/node.png
:name: workflow-rf
:align: center

Workflow KNIME untuk klasifikasi Random Forest
```

Workflow terdiri dari node-node berikut:

1. **CSV Reader** digunakan untuk membaca dataset
2. **Table Partitioner** digunakan untuk membagi data training dan testing
3. **Category to Number** digunakan untuk mengonversi fitur kategorikal menjadi numerik (training)
4. **Category to Number (Apply)** digunakan untuk menerapkan konversi yang sama pada data testing
5. **Column Filter** digunakan untuk memilih fitur yang digunakan
6. **Python Script** (training) digunakan untuk melatih model Random Forest
7. **Python Script** (testing) digunakan untuk melakukan prediksi menggunakan model
8. **Scorer** digunakan untuk mengevaluasi hasil prediksi

---

### Partisi

```{figure} ../img/random-forest/partisi.png
:name: partisi-rf
:align: center

Konfigurasi Table Partitioner
```

Data dibagi dengan proporsi:

- **80%** → data training
- **20%** → data testing

---

### Preprocessing: Category to Number

```{figure} ../img/random-forest/transformasi.png
:name: cat2num-rf
:align: center

Konfigurasi Category to Number
```

Karena semua fitur pada dataset Mushroom bertipe **kategorikal**, perlu dikonversi menjadi nilai numerik agar bisa diproses oleh model `scikit-learn`.

- Node **Category to Number** digunakan pada data **training**, sekaligus menyimpan mapping konversi.
- Node **Category to Number (Apply)** menerapkan mapping yang sama pada data **testing**, sehingga konsisten.

---

### Column Filter

```{figure} ../img/random-forest/filter.png
:name: colfilter-rf
:align: center

Konfigurasi Column Filter
```

Node ini digunakan untuk memilih kolom yang akan diteruskan ke Python Script. Kolom yang tidak diperlukan (seperti kolom asli sebelum konversi) dapat dibuang di sini.

---

### Training: Python Script (Random Forest)

```{figure} ../img/random-forest/python-training.png
:name: py-training-rf
:align: center

Konfigurasi Python Script untuk Training
```

Model Random Forest dilatih menggunakan library `scikit-learn`. Setelah training selesai, model disimpan menggunakan `pickle` agar bisa digunakan kembali pada node testing.

```python
import knime.scripting.io as knio
from sklearn.ensemble import RandomForestClassifier
import pickle
import os

# Ambil data training dari KNIME
xtrain = knio.input_tables[0].to_pandas()

# Pisahkan fitur dan label
X = xtrain.drop(columns=['class_number'])
y = xtrain['class_number']

# Training model Random Forest
model = RandomForestClassifier(n_estimators = 1000, max_depth=2, random_state=0)
model.fit(X, y)

# Simpan model ke file pickle
pickle_filename = 'random_forest_model.pkl'
with open(pickle_filename, 'wb') as file:
    pickle.dump(model, file)

print(f"Model disimpan di: {os.path.abspath(pickle_filename)}")

knio.output_tables[0] = knio.input_tables[0]
```

**Parameter yang digunakan:**

| Parameter | Nilai | Keterangan |
| --- | --- | --- |
| `n_estimators` | 1000 | Jumlah pohon yang terbentuk |
| `max_depth` | 2 | Kedalaman maksimum setiap pohon |
| `random_state` | 0 | Seed untuk reproducibility |

---

### Testing: Python Script (Prediksi)

```{figure} ../img/random-forest/python-testing.png
:name: py-testing-rf
:align: center

Konfigurasi Python Script untuk Testing
```

Model yang telah disimpan di-load kembali menggunakan `pickle`, lalu diterapkan pada data testing untuk menghasilkan prediksi.

```python
import knime.scripting.io as knio
import pickle

# Load data testing dari KNIME
xtest = knio.input_tables[0].to_pandas()

# Load model
pickle_filename = 'random_forest_model.pkl'
with open(pickle_filename, 'rb') as file:
    loaded_model = pickle.load(file)
print("Model loaded successfully.")

# Pisahkan fitur 
X_test = xtest.drop(columns=['class_number'], errors='ignore')

# Prediksi
predictions = loaded_model.predict(X_test)
probabilities = loaded_model.predict_proba(X_test)

# Tambahkan hasil ke dataframe
xtest['predicted_class'] = predictions
xtest['confidence'] = probabilities.max(axis=1).round(4)

# Output ke KNIME
knio.output_tables[0] = knio.Table.from_pandas(xtest)
```

Kolom hasil yang ditambahkan:

- `predicted_class` → hasil prediksi kelas (0 = edible, 1 = poisonous)
- `confidence` → probabilitas tertinggi dari model, menunjukkan seberapa yakin model terhadap prediksinya

---

### Scorer

```{figure} ../img/random-forest/scorer.png
:name: scorer-rf
:align: center

Hasil Scorer
```

Node **Scorer** digunakan untuk mengevaluasi performa model dengan membandingkan hasil prediksi terhadap label asli pada data testing.

#### Confusion Matrix

```{figure} ../img/random-forest/confusion.png
:name: confusion-rf
:align: center

Confusion Matrix
```

#### Akurasi

<!-- TEMPEL SCREENSHOT AKURASI DI SINI -->
```{figure} ../img/random-forest/akurasi.png
:name: akurasi-rf
:align: center

Hasil Akurasi Model
```