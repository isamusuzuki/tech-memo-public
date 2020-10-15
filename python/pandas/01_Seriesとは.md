# Series

作成日 2020/10/15

## 01. Series とは

1 次元の配列のようなオブジェクト\
Series は、values と index から成り立っている\
values は、Numpy の ndarray そのもの

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

## 02. Series のコンストラクタ

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
