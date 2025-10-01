# 2 つの DataFrame をジョインする

作成日 2021/01/20、更新日 2023/03/01

- ジョインは縦方向の結合
- `DataFrame.append()`メソッドは引退するので、使ってはいけない
- 新しく、`pandas.concat()`メソッドを使う

```python
import pandas as pd

df1 = pd.read_csv(csv_file1, encoding='cp932', dtype=object)
df2 = pd.read_csv(csv_file2, encoding='cp932', dtype=object)

df3 = pd.concat([df1, df2])

df3.to_csv(csv_file3, encoding='utf-8', index=False)
```

## `pandas.concat` 公式ドキュメント

[https://pandas.pydata.org/docs/reference/api/pandas.concat.html](https://pandas.pydata.org/docs/reference/api/pandas.concat.html)

構文: `pandas.concat(objs)`

引数解説

objs ... a sequence or mapping of Series or DataFrame objects

- DataFrame は 2 個に限定されず、3 個以上あってもよい
- 戻り値は、新しい DataFrame
