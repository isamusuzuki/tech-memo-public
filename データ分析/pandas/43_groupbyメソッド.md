# groupby メソッドを使いこなす

作成日 2020/10/28、更新日 2021/03/16

## 01. groupby メソッドとは

[pandas\.DataFrame\.groupby — pandas 1\.1\.3 documentation](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.groupby.html)

`DataFrame.groupby()` メソッドの戻り値は、GroupBy オブジェクトであり、DataFrame ではない

[GroupBy — pandas 1\.1\.3 documentation](https://pandas.pydata.org/pandas-docs/stable/reference/groupby.html)

`GroupBy.count()`, `GroupBy.size()`, `GroupBy.sum()` いずれのメソッドも DataFrame を返す。groupby された項目が index となり、残った項目のそれぞれ処理された値が values となる。残った項目の列名は変わらない

```python
import pandas as pd

def __nukidashi__(sku: str) -> str:
    split_list = sku.split('-')
    return split_list[0]

df1 = pd.read_json(json_data, orient='records')
df1['item'] = df1['sku'].apply(__nukidashi__)
df2 = df1[['item', 'sku']]

df3 = df2.groupby('item').size()
df4 = df2.groupby('item').count()
df5 = df2.groupby('item').sum()
```

## 02. GROUPBY した後に、その合計値を保存する

shop_name で GROUPBY して、品代金と代引手数料は合計値になっているデータを保存する

```python
import pandas as pd

df1 = pd.read_csv(
    'seisansyo_20201228191541.csv', encoding='utf-8')
df2 = df1[['shop_name', '品代金', '代引手数料']]
df3 = df2.groupby(['shop_name']).sum()
df3.to_csv(
    'seisansyo_20201228191541_group.csv', 
    index_label='shop_name')
```

- `index`引数がないことに注意。`index=False`してしまったら、groupby した値が消えてしまう
- `index_label`引数で、indexにラベルを付けることができる

## 03. GROUPBY したその値だけが欲しい場合

つまり、ある列に登場するユニークな値だけをリスト化したい場合

GROUPBYした後の DataFrame の index をリスト化すればよい

```python
import json
import pandas as pd

df1 = pd.read_csv(
    'seisansyo_20201228191541.csv', encoding='utf-8')
df2 = df1.groupby(['shop_name']).count()
shop_name_list = df2.index.tolist()
with open('shop_name_list.json', mode='w', encoding='utf-8') as f:
    f.write(json.dumps(shop_name_list, ensure_asii=False, indent=4))
```

## 04. GROUPBY した後、index になった項目を列に戻す

`DataFrame.reset_index()` メソッドは、インデックスを0始まりの連番に振りなおすだけでなく、元のインデックスを新しい列に変更する（列名は`index`で固定）

```python
import pandas as pd

df1 = pd.read_csv('aaa.csv', encoding='cp932')
# item_codeをGROUPBYして、残りの列をカウントして、sku_code列だけ残す
df2 = df1.groupby('item_code').count()[['sku_code']]
# インデックスを振りなおす。item_codeは列になる
df3 = df2.reset_index()
# 列名を変更する
df4 = df3.rename(columns={
    'index': 'item_code',
    'sku_code': 'sku_count'
})
sku_count_list = df4.to_dict(orient='records')
```
