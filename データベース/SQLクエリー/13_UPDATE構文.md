# UPDATE 構文

作成日 2020/11/10

## 01. 基本形

```sql
UPDATE
    users
SET
    password = '123456',
    expiredDate = '2019-12-31 23:59:59'
WHERE
    email = 'user@sample.com';
```

## 02. ジョインアップデート

2 つのテーブルをジョインさせて、マッチした行のデータ更新を行う

```sql
UPDATE
    items i
    INNER JOIN items_temp t ON i.kanri_no = t.kanri_no
SET
    i.joudai_price = t.joudai_price,
    i.is_shipping_charge = t.is_shipping_charge
WHERE
    t.cursor_id = '8196db33';
```

## 03. 主キーを更新する

外部キー制約を一時的に無効にすれば可能

```sql
SET
    FOREIGN_KEY_CHECKS = 0;

UPDATE
    m_items
SET
    item_code = 'wubenlt35'
WHERE
    item_code = 'WUBENLT35';

UPDATE
    m_item_packing
SET
    item_code = 'wubenlt35'
WHERE
    item_code = 'WUBENLT35';

SET
    FOREIGN_KEY_CHECKS = 1;
```
