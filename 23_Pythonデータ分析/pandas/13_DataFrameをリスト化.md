# DataFrame をリスト化する

作成日 2021/01/07、更新日 2021/03/17

## 01. DataFrame を Python の辞書リストに変換する

```python
import pandas as pd

csv_file = 'apps/haisou/data.csv'
df1 = pd.read_csv(csv_file, encoding='utf-8')
haisou_list = df1.to_dict(orient='records')
total_count = len(haisou_list)
```

## 02. DataFrame の 1 列分をリストに変換する

Series に変換してから、values プロパティをリストに変換する

```python
import pandas as pd

excel_file = 'temp/data/xlsx'
df = pd.read_excel(excel_file, sheet_name=0)
s = df[df['数量' > 0]]['商品コード']
#=> 戻り値はSeriesになっている
item_list = s.values.tolist()
total_count = len(item_list)
```
