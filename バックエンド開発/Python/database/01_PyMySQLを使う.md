# PyMySQL を使う

作成日 2019/11/28、更新日 2020/03/04

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
```
