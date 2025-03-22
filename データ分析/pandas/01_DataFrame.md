# DataFrame

作成日 2020/10/15

## 01. DataFrame とは

DataFrame とは、テーブル形式のスプレッドシート風のデータ構造を持ち、順序付けられた列を持っている。行と列の両方に index を持っている

```python
import pandas as pd

data = {
    'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada'],
    'year': [2000, 2001, 2002, 2001, 2002],
    'pop': [1.5, 1.7, 3.6, 2.4, 2.9]
}
df = pd.DataFrame(data)

print(df)
# =>    year   state  pop
# => 0  2000    Ohio  1.5
# => 1  2001    Ohio  1.7
# => 2  2002    Ohio  3.6
# => 3  2001  Nevada  2.4
# => 4  2002  Nevada  2.9

print(df.shape)
# => (5, 3)

print(df.dtypes)
# => state     object
# => year       int64
# => pop      float64
# => dtype: object
```

Pandas の dtype では、文字列が object になる

## DataFrame のコンストラクタ

ドキュメント => [pandas\.DataFrame — pandas 0\.25\.3 documentation](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html)

構文

```python
df = pd.DataFrame(
    data=None,
    index=None,
    columns=None,
    dtype=None,
    copy=False
)
```

引数

- data ... ndarray, Iterable, dict or DataFrame
- index ... Index or array-like
- columns ... Index or array-like
- dtype ... dtype
- copy ... boolean
