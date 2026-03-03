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
# Kategorikal
Menghitung jarak data binary beberapa sampel dari data dibawah ini
```{code-cell}
:tags: [hide-input]
import pandas as pd
import numpy as np
df = pd.read_csv("../../data/Churn_Modelling.csv")
df.head(5)
```

```{code-cell}
:tags: [hide-input]
from scipy.spatial.distance import jaccard
categorical_cols = ['Geography', 'Gender']
df_cat = df[categorical_cols]
p1 = df_cat.iloc[0]
p2 = df_cat.iloc[1]

distance = np.mean(p1 != p2)
print("Categorical Distance:", distance)
```