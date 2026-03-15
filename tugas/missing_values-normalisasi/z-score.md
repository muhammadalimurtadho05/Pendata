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

# Z-Score Normalization
Z-score adalah metode normalisasi atau standarisasi data digunakan untuk mengubah nilai data sehingga menunjukkan seberapa jauh suatu nilai dari rata-rata data dalam satuan standar deviasi.

Rumus menghitung Z-score adalah 

$$
z = \frac{x - \mu}{\sigma}
$$

Dimana:
- $x$ = Nilai Data
- $\mu$ = Rata Rata(mean)
- $\sigma$ = Standar Deviasi
- $z$ = Nilai Z-Score 

Standar deviasi adalah ukuran statistik yang menunjukkan seberapa jauh data menyebar dari nilai rata-rata (mean). Semakin besar standar deviasi, semakin menyebar data tersebut; semakin kecil standar deviasi, semakin dekat data ke rata-rata.

Rumus Standar Deviasi sebagai berikut:

$$
\sigma = \sqrt{\frac{1}{N}\sum_{i=1}^{N}(x_i-\mu)^2}
$$

Contoh:
| No. | IPK |   PO  | JML |
|:---:|:---:|:-----:|:---:|
|  1. |  2  |2000000|  2  |
|  2. |  3  |3000000|  3  |
|  3. |  4  |2000000|  2  |
|  4. |  2  |2000000|  3  |
|  5. |  3  |3000000|  2  |
|  6. |  4  |4000000|  3  |

Pada data diatas akan dilakukan `Z-Score Normalization` pada kolom IPK. Dari kolom IPK, didapatkan rata-rata(mean) yaitu `3`

Sebelum menghitung `Z-Score` Perlu diketahui terlebih dahulu Standar Deviasi pada kolom IPK

$$

\sigma &= \sqrt{\frac{1}{N}\sum_{i=1}^{N}(x_i-\mu)^2} \\
\\
\sigma &= \sqrt{\frac{1}{6}( (2-3)^2 + (3-3)^2 + (4-3)^2 + (2-3)^2 + (3-3)^2 + (4-3)^2) }\\
\\
\sigma &= \sqrt{\frac{1}{6}( 1 + 0 + 1 + 1 + 0 + 1 ) }\\
\\
\sigma &= \sqrt{\frac{1}{6}(4) } = \sqrt{0.666667} = 0.816497

$$

Rumus diatas apabila diimplementasikan kedalam Python adalah sebagai berikut

```{code-cell}
:tags: [hide-input]
import numpy as np

data = {
    'No': [1, 2, 3, 4, 5, 6],
    'IPK': [2, 3, 4, 2, 3, 4],
    'PO': [2000000, 3000000, 2000000, 2000000, 3000000, 4000000],
    'JML': [2, 3, 2, 3, 2, 3]
}

ipk_data = np.array(data['IPK'])
std_populasi = np.std(ipk_data)
print(f"Standar Deviasi Populasi: {std_populasi}")
```

Setelah nilai Standar Deviasi diperoleh, tinggal memasukkan nilai tersebut untuk melakukan perhitungan Z-Score Normalization

$$
z &= \frac{x - \mu}{\sigma}\\
\\
z1 &= \frac{2 - 3}{0.816497} = \frac{-1}{0.816497} = -1.22474\\
\\
z2 &= \frac{3 - 3}{0.816497} = \frac{0}{0.816497} = 0\\
\\
z3 &= \frac{4 - 3}{0.816497} = \frac{1}{0.816497} = 1.224745\\
\\
z4 &= \frac{2 - 3}{0.816497} = \frac{-1}{0.816497} = -1.22474\\
\\
z5 &= \frac{3 - 3}{0.816497} = \frac{0}{0.816497} = 0\\
\\
z6 &= \frac{4 - 3}{0.816497} = \frac{1}{0.816497} = 1.224745\\
$$
```{code-cell}
:tags: [hide-input]
mean_ipk = np.mean(ipk_data)

z_scores = (ipk_data - mean_ipk) / std_populasi

print(f"Rata-rata IPK: {mean_ipk}")
print(f"Standar Deviasi: {std_populasi:.4f}")
print("-" * 30)
print("No | IPK | Z-Score")
for i in range(len(data['No'])):
    print(f"{data['No'][i]}  | {data['IPK'][i]}   | {z_scores[i]:.4f}")
```