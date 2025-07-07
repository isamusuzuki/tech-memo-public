# INSERT 構文

作成日 2020/11/10、更新日 2022/09/12

## 01. 基本形

```sql
INSERT INTO
    users (email, password, groupName, expiredDate)
VALUES
    (
        'user@sample.com',
        'ac7acm',
        'tokyo170928',
        '2019-07-25 03:00:00'
    );
```

## 02. マルチプルインサート

VALUES 後のカッコ `()` をカンマ `,` で区切って繰り返すと、複数のデータを 1 行の SQL クエリーでインサートすることができる

```sql
INSERT INTO
    users (email, password, groupName, expiredDate)
VALUES
    (
        'user@sample.com',
        'ac7acm',
        'tokyo170928',
        '2019-07-25 03:00:00'
    ),
    (
        'user2@sample.com',
        'b4c5g',
        'tokyo170928',
        '2019-07-25 03:00:01'
    );
```

## 03. セレクトインサート

あるテーブルでのセレクト結果を、別のテーブルにインサートすることができる

```sql
INSERT INTO
    items (
        item_code,
        kanri_no,
        joudai_price,
        is_shipping_charge
    )
SELECT
    item_code,
    kanri_no,
    joudai_price,
    is_shipping_charge
FROM
    items_temp t
WHERE
    t.cursor_id = '8196db33'
    AND t.is_found = 0;
```

## 04. インサートアップデート

主キーまたはユニークインデックスに重複した値が挿入される時は、代わりに、古い行の更新が実行される

```sql
INSERT INTO
    sku_images (sku_code,
        file_name,
        gsurl,
        updated_at,
        created_at)
VALUES
    (
        "sku_code1",
        "file_name1",
        "gsurl1",
        "created_at1",
        "created_at1"
    ),
    (
        "sku_code2",
        "file_name2",
        "gsurl2",
        "created_at2",
        "created_at2"
    )
ON DUPLICATE KEY
UPDATE
    file_name = VALUES(file_name),
    gsurl = VALUES(gsurl),
    overwrite_times = overwrite_times + 1,
    updated_at = VALUES(updated_at);
```

### MySQL 8.0 でインサートアップデートを使ったら、警告が出るようになった

```text
'VALUES function' is deprecated and will be removed in a future release. Please use an alias (INSERT INTO ... VALUES (...) AS alias) and replace VALUES(col) in the ON DUPLICATE KEY UPDATE clause with alias.col instead
```

今までの書き方

```sql
INSERT INTO foo (bar, baz) VALUES (1,2)
ON DUPLICATE KEY UPDATE baz=VALUES(baz);
```

新しい書き方

```sql
INSERT INTO foo (bar, baz) VALUES (1,2) AS new_foo
ON DUPLICATE KEY UPDATE baz=new_foo.baz;
```

## 05. よくあるエラー

テーブル内のカラム数とそれに与える VALUE の数が一致していないと、下記のエラーメッセージが表示される

```text
Column count doesn\'t match value count at row 1
```

テーブル内のカラム数とそれに与える VALUE の数が一致していない例

```sql
INSERT INTO
    table1 (column1, column2) VALUES ('test');
```
