# SQL Formatter

作成日 2020/11/20

## 01. SQL Formatter とは

この機能拡張をインストールすると、SQL ファイル（`.sql` 拡張子）で、フォーマットをかけたとき（右クリック ＞ `Format Document` コマンド）に、キレイに整うようになる

[SQL Formatter \- Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=adpyke.vscode-sql-formatter)

## 02. 懸念点を確認する

### PyMySQL ライブラリの作法

PyMySQL ライブラリの Curosor オブジェクトが持つ `execute()` メソッドにおいて、第一引数のクエリー文字列に 第二引数のタプル・リスト・辞書を挿入することができる。そのとき、クエリー文字列の中に プレースフォルダとして `%s` を書いておくようにと指示されている

```python
import pymysql

conn = pymysql.connect(
    # 省略
)

sql = 'SELECT * FROM m_sku where item_code = %s;'
item_code 'abc123'

with conn.cursor() as cursor:
    cursor.execute(sql, (item_code))
    db_results = cursor.fetchall()
```

[Cursor Objects — PyMySQL 0\.7\.2 documentation](https://pymysql.readthedocs.io/en/latest/modules/cursors.html)

### SQL Formatter の勝手な動き

ところが、SQL Formatter は、この `%s` を `% s` と修正してしまう。これを止めさせる設定を探したが見つからなかった

`%s` が `% s` になっても、PyMySQL は正しく動作するのかすごく心配になった。心配していても仕方ないので、実験してみることにした

get_sku.sql

```sql
SELECT
    *
FROM
    m_sku
where
    item_code = % s;
```

connect.py

```python
import pymysql

conn = pymysql.connect(
    # 省略
)

sql_file = 'get_sku.sql'
with open(sql_file, mode='r', encoding='utf-8') as f:
    sql = f.read()

item_code 'abc123'

with conn.cursor() as cursor:
    cursor.execute(sql, (item_code))
    db_results = cursor.fetchall()
```

=>  結論: 問題ない
