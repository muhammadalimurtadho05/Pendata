# Decission Tree
## Dataset
Dataset yang digunakan untuk klasifikasi menggunakan metode naive bayes adalah `Mushroom Classification Dataset`. Dataset ini berisi deskripsi sampel hipotetis dari 23 spesies jamur berjenis insang (gilled mushrooms) dari famili Agaricus dan Lepiota. Setiap spesies diidentifikasi sebagai dapat dimakan (edible), beracun (poisonous), atau tidak diketahui edibilitasnya. Kelas terakhir digabung ke kelas poisonous.

Link : [Mushroom Classification Dataset](https://www.kaggle.com/datasets/uciml/mushroom-classification)

Dataset ini miliki 8.124 baris data, 22 fitur yang bertipe kategorikal dan 1 label yang bernama `class` yang memiliki 2 nilai, yaitu `e` atau _Edible_ yang berarti jamur bisa dimakan dan `p` atau _Poisonous_ yang berarti jamur beracun atau tidak bisa dimakan.

Berikut seluruh fitur beserta nilai kategorinya:

|No| Nama Fitur                     | Deskripsi                          | Nilai Kategori                                                                 |
|----|-------------------------------|------------------------------------|--------------------------------------------------------------------------------|
| 1  | cap-shape                    | Bentuk tudung                      | bell, conical, convex, flat, knobbed, sunken                                  |
| 2  | cap-surface                  | Permukaan tudung                   | fibrous, grooves, scaly, smooth                                               |
| 3  | cap-color                    | Warna tudung                       | brown, buff, cinnamon, gray, green, pink, purple, red, white, yellow          |
| 4  | bruises                      | Ada memar?                         | bruises, no                                                                   |
| 5  | odor                         | Bau jamur                          | almond, anise, creosote, fishy, foul, musty, none, pungent, spicy             |
| 6  | gill-attachment              | Cara insang menempel               | attached, descending, free, notched                                           |
| 7  | gill-spacing                 | Jarak antar insang                 | close, crowded, distant                                                       |
| 8  | gill-size                    | Ukuran insang                      | broad, narrow                                                                 |
| 9  | gill-color                   | Warna insang                       | black, brown, buff, chocolate, gray, green, orange, pink, purple, red, white, yellow |
| 10 | stalk-shape                  | Bentuk batang                      | enlarging, tapering                                                           |
| 11 | stalk-root                   | Akar batang                        | bulbous, club, cup, equal, rhizomorphs, rooted, missing(?)                    |
| 12 | stalk-surface-above-ring     | Permukaan batang atas ring         | fibrous, scaly, silky, smooth                                                 |
| 13 | stalk-surface-below-ring     | Permukaan batang bawah ring        | fibrous, scaly, silky, smooth                                                 |
| 14 | stalk-color-above-ring       | Warna batang atas ring             | brown, buff, cinnamon, gray, orange, pink, red, white, yellow                 |
| 15 | stalk-color-below-ring       | Warna batang bawah ring            | brown, buff, cinnamon, gray, orange, pink, red, white, yellow                 |
| 16 | veil-type                    | Tipe selubung                      | partial, universal                                                            |
| 17 | veil-color                   | Warna selubung                     | brown, orange, white, yellow                                                  |
| 18 | ring-number                  | Jumlah cincin                      | none, one, two                                                                |
| 19 | ring-type                    | Tipe cincin                        | cobwebby, evanescent, flaring, large, none, pendant, sheathing, zone          |
| 20 | spore-print-color            | Warna cetak spora                  | black, brown, buff, chocolate, green, orange, purple, white, yellow           |
| 21 | population                   | Pola pertumbuhan                   | abundant, clustered, numerous, scattered, several, solitary                   |
| 22 | habitat                      | Habitat tumbuh                     | grasses, leaves, meadows, paths, urban, waste, woods                          |

## Implementasi Pada KNime
Workflow ini dirancang menggunakan tools KNIME untuk membangun model klasifikasi Decision Tree. Bertujuan untuk menentukan apakah jenis jamur bisa dikonsumsi atau tidak.

![Grafik Data](../img/decission-tree/workflow.png)

### Partisi
Langkah awal setelah membaca dataset adalah melakukan partisi, yaitu untuk membagi data training dan data testing. Dalam kasus ini, saya menggunakan data sebanyak 70% sebagai data training dan 30% sebagai data testing.

![Grafik Data](../img/decission-tree/partisi.png)

### Decission Tree Learner
![Grafik Data](../img/decission-tree/learner.png)

Dikarenakan saya menggunakan `Gain Ratio` pada _Quality Measure_, maka perlu menghitung `Gain Ratio` tertinggi untuk menentukan _Root_ atau akar dari `Decission Tree` dengan rumus sebagai berikut.

$$
\begin{aligned}
\\[10pt]
GainRATIO_{split} &= \frac{Gain_{split}}{SplitINFO} \\[10pt]
Gain_{split} &= Entropy(p) - (\sum_{i=1}^{k} {\frac{n_{i}}{n}Entropy(i)})  \\[10pt]
Entropy(t) &= - \sum{p(j|t) log(p(j|t))}  \\[10pt]
SplitINFO &= - \sum_{i=1}^k{\frac{n_{i}}{n}Log\frac{n_{i}}{n}}
\end{aligned}
$$

Dimana :
1.  $GainRATIO_{split}$ merupakan nilai rasio gain untuk suatu atribut pemisah (split). Semakin tinggi nilainya, semakin baik atribut tersebut dipilih sebagai node pemisah.
2. $Gain_{split}$ merupakan selisih entropy sebelum dan sesudah pemisahan data berdasarkan atribut tertentu. Mengukur seberapa besar pengurangan ketidakpastian setelah data dipecah.
    - $Entropy(p)$ merupakan Entropy dari dataset induk (parent) sebelum split
    - $k$ merupakan Jumlah cabang/subset hasil pemisahan 
    - $i$ merupakan Indeks cabang ke-i (dari 1 sampai k)
    - $n$ merupakan Jumlah total data di node induk
    - $n_{i}$ merupakan Jumlah data pada cabang/subset ke-i
    - $Entropy(i)$ merupakan Entropy dari subset ke-i setelah split
3. $Entropy(t)$ Mengukur tingkat ketidakmurnian (impurity) atau ketidakpastian pada node $t$
    - $t$ Node yang sedang dihitung
    - $j$ Label kelas ke-j
    - $p(j|t)$ Probabilitas kelas j pada node t
    - $log$ Logaritma basis 2 ($log_{2}$)
```{note}
Entropy = 0 berarti data murni (satu kelas)

Entropy = 1 berarti data paling tidak pasti (seimbang antar kelas)
```

4. $SplitINFO$ Mengukur seberapa luas dan merata suatu atribut membagi data. Digunakan sebagai pembagi (denominator) agar atribut dengan banyak cabang tidak selalu dipilih.
    - $k$ Jumlah cabang hasil split
    - $n_{i} / n$ Proporsi data pada cabang ke-i
    - $Log(n_{i} / n)$ Logaritma basis 2 dari proporsi tersebut

#### Hitung Entropy Root
- Jumlah kelas `e` = 4208
- Jumlah kelas `p` = 3916
- Jumlah Data = 8124

$$
\begin{aligned}
Entropy(s) &= -\frac{4208}{8124} \log_2 \left(\frac{4208}{8124}\right) 
             - \frac{3916}{8124} \log_2 \left(\frac{3916}{8124}\right) \\[1em]
Entropy(s) &= -0.5182 \times (-0.9488) - 0.4818 \times (-1.0536) = 0.9991
\end{aligned}
$$

### Hitung Gain, Split Info, dan Gain Ratio tiap Atribut
1. Atribut `Odor`
    
