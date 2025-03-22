# 2 つの DataFrame をマージする

作成日 2021/03/05

- マージは横方向の結合
- キーとする列が同じ名前の場合は、引数`on`を使う
- キーとする列が違う名前の場合は、引数`left_on`,`right_on`を使う
- `how='inner'` ... 内部結合、インナージョイン
- `how='left'` ... 左結合
- `how='right'` ... 右結合
- `how='outer'` ... 外部結合、アウタージョイン

```python
import pandas as pd

csv_file = 'temp/aaa.csv'
df1 = pd.read_csv(csv_file, encoding='utf-8')
csv_file = 'temp/bbb.csv'
df2 = pd.read_csv(csv_file, encoding='utf-8')

df3 = pd.merge(
    df1, df2, how='left',
    left_on='商品コード', right_on='item_code'
)
print(df3.shape)
```
