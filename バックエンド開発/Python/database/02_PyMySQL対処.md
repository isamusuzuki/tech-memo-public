# PyMySQL トラブルに対処する

作成日 2021/02/01

## 01. 日付時刻が入っているカラムからデータを取り出す

### 01-1. 問題発生

PyMySQL が取得した辞書リストを、JSON データに変換しようとしたら、以下のようなエラーが出た

`TypeError: Object of type datetime is not JSON serializable`

このエラーの原因は、PyMySQL が日付時刻データを Python の Datetime クラスに変換するため

### 01-2. 解決方法 その 1

`json.dumps()` メソッドに、ヘルパーメソッドを与える

```python
from datetime import datetime
import json

import pymysql

conn = pymysql.connect()

sql = (
    'select id, created_at'
    ' from logs order by id desc limit 50;'
)
with conn.cursor() as cursor:
    cursor.execute(sql)
    results = cursor.fetchall()

def helper(obj):
    if isinstance(obj, datetime):
        return obj.strftime('%Y-%m-%d %H:%M:%S')
    raise TypeError(f'Type {type(obj)} is not JSON serializable')

with open('temp/test.json', mode='w', encoding='utf-8') as f:
    f.write(
        json.dumps(results, ensure_ascii=False, 
        indent=4, default=helper))
```

### 01-3. 解決方法 その 2

SQL クエリーの中で、DATE_FORMAT 関数を使って、日付時刻データを文字列に変換する

```python
sql = (
    'select id,'
    ' DATE_FORMAT(created_at, "%Y-%m-%d %H:%i:%s") created_at'
    ' from logs order by id desc limit 50;'
)
```

## 02. 集計したカラムからデータを取り出す

### 02-1. 問題発生

PyMySQL が取得した辞書リストを、JSON データに変換しようとしたら、以下のようなエラーが出た

日付時刻データは含まれていないが、SUM などを使って集計している

`TypeError: Object of type Decimal is not JSON serializable`

このエラーの原因は、PyMySQL が集計データを Python の Decimal クラスに変換するため

### 02-2. 解決方法

SQL クエリーの中で、CAST 関数または CONVERT 関数を使って、集計データを整数に変換する

```python
sql = (
    'select id, cast(sum(success) as signed)'
    ' from logs group by id'
)
```
