# INFORMATION_SCHEMA を使う

作成日 2019/12/19

## 01. INFORMATION_SCHEMA とは

スキーマの定義情報が保存されているデータベース

[MySQL :: MySQL 5\.6 リファレンスマニュアル :: 21 INFORMATION_SCHEMA テーブル](https://dev.mysql.com/doc/refman/5.6/ja/information-schema.html)

### INFORMATION_SCHEMA.TABLES テーブル情報

```sql
SELECT
    table_schema
  , table_name
  , table_type
  , engine
  , version
  , create_time
  , update_time
  , table_collation
from
  information_schema.tables
where
  table_schema = 'database_name'
and
  table_name = 'table_name'
```

テーブルごとの大きさを調べることができる

```sql
SELECT
    table_rows
  , avg_row_length
  , floor((DATA_LENGTH + INDEX_LENGTH) / 1024 / 1024) AS SIZE
  , floor((DATA_LENGTH) / 1024 / 1024) AS DATA_SIZE
  , floor((INDEX_LENGTH) / 1024 / 1024) AS INDEX_SIZE
from
  information_schema.tables
where
  table_schema = 'database_name'
and
  table_name = 'table_name'
```

### INFORMATION_SCHEMA.COLUMNS カラム情報

```sql
SELECT
    table_name
  , column_name
  , column_type
  , is_nullable
  , column_key
  , column_default
  , extra
from
  information_schema.columns
where
  table_schema = 'cvg'
and
  table_name = 'm_items'
```
