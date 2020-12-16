# ALTER TABLE 構文

作成日 2020/11/10、更新日 2020/12/04

## 01. ALTER TABLE table ADD column

後からカラムを追加することが可能

`AFTER` を使うと、カラム位置を指定することが可能

```sql
ALTER TABLE
    m_malls
ADD
    fee double NOT NULL DEFAULT 0
AFTER
    mall_name;

ALTER TABLE
    m_suppliers
ADD
    cutoff_date varchar(255) DEFAULT NULL COMMENT '締め日'
AFTER
    follow_days;
```

## 02. ALTER TABLE table DROP column

テーブルからカラムを削除する

```sql
ALTER TABLE
    m_items
DROP
    status;
```
