# 2 つの DataFrame をジョインする

作成日 2021/01/20

- ジョインは縦方向の結合
- `DataFrame.append()`メソッドが使える
- 最初に空の DataFrame を用意することで繰り返しが使える

```python
import os
import pandas as pd

# 空のDataFrameを用意する
df = pd.DataFrame({'商品コード': [], '商品名': []})

for filename in os.listdir('work210119/data'):
    csv_file = f'work210119/data/{filename}'
    df1 = pd.read_csv(csv_file, encoding='cp932', dtype=object)
    df2 = df1[['商品コード', '商品名']]
    df = df.append(df2, ignore_index=True)

df.to_csv(f'temp/all.csv', encoding='utf-8', index=False)
```
