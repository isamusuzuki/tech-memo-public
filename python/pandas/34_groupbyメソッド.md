# groupbyメソッドを使いこなす

作成日 2020/10/28、更新日 2021/01/07

## 01. pandas.DataFrame.groupbyメソッド

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

## 02. GROUPBYした後に、その合計値を保存する

shop_nameでGROUPBYされ、品代金と代引手数料は合計値になっているデータを保存する

```python
    df = pd.read_csv(
        'seisansyo_20201228191541.csv', encoding='utf-8')
    df1 = df[['shop_name', '品代金', '代引手数料']]
    df2 = df1.groupby(['shop_name']).sum()
    df2.to_csv(
        'seisansyo_20201228191541_group.csv')
```
