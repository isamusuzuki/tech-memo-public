# PyMySQL を使いこなす

作成日 2019/11/28、更新日 2020/02/25

## 01. PyMySQL とは

MySQL データベースに接続するライブラリ

ドキュメント => [https://pymysql.readthedocs.io/en/latest/](https://pymysql.readthedocs.io/en/latest/)

## 02. データを読む

```python
import os

import pymysql

HOST_NAME = os.environ.get('HOST_NAME')
USER_NAME = os.environ.get('USER_NAME')
PASSWORD = os.environ.get('PASSWORD')
DB_NAME = os.environ.get('DB_NAME')


def select_data(item_code):
    conn = pymysql.connect(
        host=HOST_NAME,
        user=USER_NAME,
        password=PASSWORD,
        db=DB_NAME,
        charset='utf8mb4',
        cursorclass=pymysql.cursors.DictCursor
    )
    try:
        with conn.cursor() as cursor:
            sql = (
                "select attribute_name from item_attribute "
                "where item_code = %s and attribute_type = '2' "
                "order by sort_no;"
            )
            cursor.execute(sql, (item_code))
            results = cursor.fetchall()
            i = 1
            for r in results:
                print(f'{i} => {r["attribute_name"]}')
                i += 1
    except pymysql.Error as e:
        print(f'ERROR: {e}')
    finally:
        conn.close()
```

## 03. データを書き込む

```python
import os

import pymysql

HOST_NAME = os.environ.get('HOST_NAME')
USER_NAME = os.environ.get('USER_NAME')
PASSWORD = os.environ.get('PASSWORD')
DB_NAME = os.environ.get('DB_NAME')

def insert_data(no, code, status, message)):
    conn = pymysql.connect(
        host=HOST_NAME,
        user=USER_NAME,
        password=PASSWORD,
        db=DB_NAME,
        charset='utf8mb4',
        cursorclass=pymysql.cursors.DictCursor
    )
    try:
        with conn.cursor() as cursor:
            sql = (
                'INSERT INTO `処理履歴` '
                '(`管理番号`, `処理コード`, `処理結果`, `処理コメント`) '
                'VALUES (%s, %s, %s, %s);'
            )
            cursor.execute(sql, (no, code, status, message))
            conn.commit()
            print('done')
    except pymysql.Error as e:
            print(f'ERROR: {e}')
    finally:
        conn.close()
```

## 04. pymysql のエラーに対処する

`pymysql.err.InternalError: (1046, 'No database selected')`

解決方法: SQL 文のテーブルに、データベース名を指定する

| before              | after                  |
| ------------------- | ---------------------- |
| `INSERT temp_items` | `INSERT db.temp_items` |

## 05. SQL クエリーに埋め込むパラメーターについて

ドキュメント => [Cursor Objects — PyMySQL 0\.7\.2 documentation](https://pymysql.readthedocs.io/en/latest/modules/cursors.html)

- 構文: execute(query, args=None)
- 引数 1: query ... str
- 引数 2: args ... tuple,list or dict
- 戻り: 影響があった行の数 int

> If args is a list or tuple, %s can be used as a placeholder in the query.\
> If args is a dict, %(name)s can be used as a placeholder in the query.

%s しか使えないということがわかった\
整数をわざわざ文字列にしておく必要はない\
文字列の場合は、SQL クエリーの中で括弧で囲まれる\
整数の場合は、括弧がつかない\
PyMySQL がよしなに、やってくれている

整数を埋め込みたい SQL クエリーの例

```sql
select
  sku_code,
  zeinuki_genka,
  is_shobunchuu,
  is_haiban,
  uploaded_on,
  abc_rank
from m_sku
where
  is_deleted is False
limit
  %s offset %s
```

この SQL クエリーを実行するときは、整数のタプルを与える

```python
with conn.cursor() as cursor:
    cursor.execute(sql, (10, 400))
    result = cursor.fetchall()
    df = DataFrame(result)
```

## 06. 日付時刻が入っているカラムからデータを取り出す

### エラーになるコード

created_at カラムが、timestamp を使って自動生成している場合、\
以下のコードは、辞書リストを JSON データに変換したところで TypeError を起こす

`TypeError: Object of type datetime is not JSON serializable`

このエラーの原因は、PyMySQLが日付時刻データをPythonのDatetimeクラスに変換するため

```python
import json

import pymysql

conn = pymysql.connect(
    # 略
)

sql = (
    'select id, created_at'
    ' from logs order by id desc limit 50;'
)
with conn.cursor() as cursor:
    cursor.execute(sql)
    db_results = cursor.fetchall()

with open('temp/test.json', mode='w', encoding='utf-8') as f:
    f.write(json.dumps(response, ensure_ascii=False, indent=4))
print('done')
```

### 解決方法のひとつ

SQLクエリーの中で、DATE_FORMAT関数を使って、日付時刻データを文字列に変換する

[MySQL :: MySQL 5\.7 Reference Manual :: 12\.6 Date and Time Functions](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html)

```python
sql = (
    'select id,'
    ' DATE_FORMAT(created_at, "%Y-%m-%d %H:%i:%s") created_at'
    ' from logs order by id desc limit 50;'
)
```
