# Seaborn

作成日 2020/04/26

## 01. 公式ドキュメントを読む

トップ => [https://seaborn.pydata.org/index.html](https://seaborn.pydata.org/index.html)

API => [https://seaborn.pydata.org/api.html](https://seaborn.pydata.org/api.html)

## 02. 作成できる図の種類

- [seaborn\.distplot](https://seaborn.pydata.org/generated/seaborn.distplot.html) ... ヒストグラム
- [seaborn\.scatterplot](https://seaborn.pydata.org/generated/seaborn.scatterplot.html) ... 分布図
- [seaborn\.regplot](https://seaborn.pydata.org/generated/seaborn.regplot.html)
- [seaborn\.jointplot](https://seaborn.pydata.org/generated/seaborn.jointplot.html)
- [seaborn\.FacetGrid](https://seaborn.pydata.org/generated/seaborn.FacetGrid.html)
- [seaborn\.boxplot](https://seaborn.pydata.org/generated/seaborn.boxplot.html)
- [seaborn\.violinplot](https://seaborn.pydata.org/generated/seaborn.violinplot.html)

```python
%matplotlib inline
import matplotlib.pyplot as plt
import seaborn as sns; sns.set()
import pandas as pd
import numpy as np

da = pd.read_csv('nhanes_2015_2016.csv')

sns.distplot(da['BPXDI1'].dropna())
sns.dispplot(da['BPXDI1'].dropna(), kde=False)
# => kdeは、ヒストグラムの中に引くスムーズな曲線のこと

ax = sns.scatterplot(x='BMXLEG', y='BMXARML', data=da)

sns.regplot(x='BMXLEG', y='BMXARML', data=da,
    fit_reg=False, scatter_kws={"alpha": 0.2})

sns.jointplot(x='BMXLEG', y='BMXARML', kind='kde', data=da)

da['RIAGENDRx'] = da.RIAGENDR.replace({1: 'Male', 2: 'Femail'})
sns.FacetGrid(da, col='RIAGENDRx').map(plt.scatter, 'BMXLEG', 'BMXARML', alpha=0.4).add_legend()

plt.figure(figsize=(12,4))
a = sns.boxplot(db.DMDMARTLx, db.RIDAGEYR)

plt.figure(figsize=(12,4))
a = sns.violinplot(da.DMDMARTLx, db.RIDAGEYR)
```
