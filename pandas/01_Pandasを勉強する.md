# Pandas を勉強する

作成日 2019/12/12

## 01. Pandas とは

公式サイト => [https://pandas.pydata.org/](https://pandas.pydata.org/)

最新バージョン => v0.25.3 (2019/10/31)

インストール => `pip install pandas`

ドキュメント => [https://pandas.pydata.org/pandas-docs/stable/](https://pandas.pydata.org/pandas-docs/stable/)

Pandas を使う理由 => **for ループを使わないデータ処理**

### 2 つのデータ構造

#### Series とは

1 次元の配列のようなオブジェクト\
Series は、values と index から成り立っている\
values は、Numpy の ndarray そのもの

#### DataFrame とは

テーブル形式のスプレッドシート風のデータ構造を持ち、\
順序付けられた列を持っている\
行と列の両方に index を持っている

## 02. Series

```python
from pandas import Series

obj = Series([4, 7, -5, 3])

print(obj)
# => 0    4
# => 1    7
# => 2   -5
# => 3    3
# => dtype: int64

print(type(obj))
# => <class 'pandas.core.series.Series'>

print(obj[2])
# => -5

print(obj.values)
# => [4 , 7, -5, 3]

print(type(obj.values))
# => <class 'numpy.ndarray'>

print(obj.index)
# => RangeIndex(start=0, stop=4, step=1)

print(type(obj.index))
# => <class 'pandas.core.indexes.range.RangeIndex'>
```

index 付きの Series を作成することも可能\
また配列操作した後も、values と index の関係は保持される

```python
from pandas import Series

obj2 = Series([4, 7, -5, 3], index=['d', 'b', 'a', 'c'])

print(obj2)
# => d    4
# => b    7
# => a   -5
# => c    3
# => dtype: int64

print(obj2['a'])
# => -5

print(obj2[obj2 > 0])
# => d    4
# => b    7
# => c    3
# => dtype: int64

print(obj2 * 2)
# => d     8
# => b    14
# => a   -10
# => c     6
# => dtype: int64
```

### Series のコンストラクタ

[pandas\.Series — pandas 0\.25\.3 documentation](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.html)

構文

```python
s = Series(
    data=None,
    index=None,
    dtype=None,
    name=None,
    copy=False,
    fastpath=False
)
```

引数

- data ... array-like, Iterable, dict or scalar value
- index ... array-like or Index(1d)
- dtype ... str, numpy.dtype or ExtensionDtype
- copy ... bool

## 03. DataFrame

```python
from pandas import DataFrame

data = {
    'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada'],
    'year': [2000, 2001, 2002, 2001, 2002],
    'pop': [1.5, 1.7, 3.6, 2.4, 2.9]
}
df = DataFrame(data)

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

文字列が、Pandas の dtype では object になるのがとても不思議

### DataFrame のコンストラクタ

ドキュメント => [pandas\.DataFrame — pandas 0\.25\.3 documentation](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html)

構文

```python=
df = DataFrame(
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
