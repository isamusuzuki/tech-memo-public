# SQLite3 を使う

作成日 2020/06/08

## SQLite3について

SQLite3 はパッケージを追加インストールする必要がない

## テーブルを作成してデータを挿入する

```python
import sqlite3

db_file = 'temp/sample.sqlite'
conn = sqlite3.connect(db_file)
cursor = conn.cursor()

cursor.execute('DROP TABLE IF EXISTS sample;')
cursor.execute(
    'CREATE TABLE IF NOT EXISTS sample '
    '(id INTEGER PRIMARY KEY, name TEXT);')
cursor.execute('INSERT INTO sample VALUES(1, "鈴木 男")')
cursor.execute('INSERT INTO sample VALUES(2, "佐藤 女")')
cursor.execute('INSERT INTO sample VALUES(3, "加藤 男")')
cursor.execute('INSERT INTO sample VALUES(4, "佐々木 女")')
cursor.execute('INSERT INTO sample VALUES(5, "高橋 男")')
cursor.execute('INSERT INTO sample VALUES(6, "橋本 女")')
cursor.execute('INSERT INTO sample VALUES(7, "伊藤 男")')
cursor.execute('INSERT INTO sample VALUES(8, "山田 女")')
cursor.execute('INSERT INTO sample VALUES(9, "田中 男")')
cursor.execute('INSERT INTO sample VALUES(10, "渡辺 女")')

conn.commit()
conn.close()
print('done')
```

## データを取り出す

```python
import sqlite3

db_file = 'temp/sample.sqlite'
conn = sqlite3.connect(db_file)
cursor = conn.cursor()

query = 'SELECT id, name FROM sample;'
query = 'SELECT id, name FROM sample WHERE name LIKE "%藤%";'
query = 'SELECT id, name FROM sample WHERE name LIKE "%藤%" AND name LIKE "%男%";'
query = 'SELECT id, name FROM sample WHERE name LIKE "%藤%" OR name LIKE "%男%";'
query = (
    'SELECT id, name FROM sample '
    'WHERE (name LIKE "%藤%" AND name LIKE "%男%") '
    'AND name NOT LIKE "%田中%";')
query = (
    'SELECT id, name FROM sample '
    'WHERE ((name LIKE "%男%") OR name LIKE "%佐々木%" OR name LIKE "%橋本%") '
    'AND name NOT LIKE "%田中%" AND name NOT LIKE "%伊藤%";')

for row in cursor.fetchall():
    print(row)

conn.close()
```
