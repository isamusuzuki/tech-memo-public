# groupbyメソッドを使いこなす

作成日 2020/10/28

## pandas.DataFrame.groupbyメソッド

[pandas\.DataFrame\.groupby — pandas 1\.1\.3 documentation](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.groupby.html)

戻り値は、GroupBy オブジェクトである

[GroupBy — pandas 1\.1\.3 documentation](https://pandas.pydata.org/pandas-docs/stable/reference/groupby.html)

```python
import pandas as pd

def __nukidashi__(sku: str) -> str:
    split_list = sku.split('-')
    return split_list[0]

df = pd.read_json(json_data, orient='records')
df['item'] = df['sku'].apply(__nukidashi__)
df1 = df[['item', 'sku']]

# 商品コードが何種類あるのか、どういう分布になっているかを知る
print(df1.groupby('item').size())
print(df1.groupby('item').count())
```
