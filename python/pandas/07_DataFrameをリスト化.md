# DataFrame をリスト化する

作成日 2021/01/07

## 01. DataFrame を Python の辞書リストに変換する

```python
import pandas as pd

csv_file = 'apps/haisou/data.csv'
df1 = pd.read_csv(csv_file, encoding='utf-8')
haisou_data = df1.to_dict(orient='records')
```

## 02. DataFrame の 1 列分をリストに変換する

Series に変換してから、values プロパティをリストに変換する

```python
df = pd.read_excel('temp/data.xlsx)
s = df[df['数量' > 0]]['商品コード']
#=> 戻り値はSeriesになっている

item_list = s.values.tolist()
total_count = len(item_list)
```
