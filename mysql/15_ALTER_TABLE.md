# ALTER TABLE クエリー

作成日 2020/11/10

## 01. 基本形

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
