# ALTER TABLE 構文

作成日 2020/11/10、更新日 2022/05/20

## 01. ALTER TABLE table ADD column

後からカラムを追加することが可能

`AFTER` を使うと、カラム位置を指定することが可能

既存データにはデフォルト値が入るので、デフォルト値の設定は不可欠

```sql
ALTER TABLE
    m_malls
ADD
    fee double DEFAULT 0
AFTER
    mall_name;

ALTER TABLE
    m_suppliers
ADD
    cutoff_date varchar(255) DEFAULT '' COMMENT '締め日'
AFTER
    follow_days;
```

## 02. ALTER TABLE table RENAME COLUMN column1 TO column2

カラム名を変更する

```sql
ALTER TABLE
    item_master
RENAME
    COLUMN `lg_category` TO `linegift_category`;
```

## 03. ALTER TABLE table MODIFY COLUMN column

コメントを追加したり、カラムの属性を変更する

```sql
ALTER TABLE
    item_master
MODIFY
    COLUMN `linegift_category` varchar(255) DEFAULT '' COMMENT 'LINEギフトブランドコード';
```

## 04. ALTER TABLE table DROP column

テーブルからカラムを削除する

```sql
ALTER TABLE
    m_items
DROP
    status;
```
