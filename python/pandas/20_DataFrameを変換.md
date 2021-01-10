# DataFrame を変換する

作成日 2021/01/07

## 01. DataFrame を Python の辞書リストに変換する

以下のように、JSON ファイルを読んで、辞書リストとして保持するコードがあった

```python
import json

class HaisouSolver():
    def __init__(self):
        json_file = 'apps/haisou/data.json'
        with open(json_file, mode='r', encoding='utf-8') as f:
            body = f.read()
            self.haisou_data = json.loads(body)
```

JSON ファイルが大きくなって見づらくなってきたので、CSV ファイルに変更した

そのときに、書き換えたコードがこちら

```python
import pandas as pd

class HaisouSolver():
    def __init__(self):
        csv_file = 'apps/haisou/data.csv'
        df = pd.read_csv(csv_file, encoding='utf-8')
        self.haisou_data = df.to_dict(orient='records')
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

もしもカウントするのが目的だったならば、groupbyメソッドが使えるはず

```python
df1 = df[df['数量' > 0]]
df1.groupby('商品コード').count()
```
