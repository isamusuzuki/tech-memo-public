# INSERT クエリー

作成日 2020/11/10

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

## 04. よくあるエラー

テーブル内のカラム数とそれに与える VALUE の数が一致していないと、下記のエラーメッセージが表示される

```text
Column count doesn\'t match value count at row 1
```

テーブル内のカラム数とそれに与える VALUE の数が一致していない例

```sql
INSERT INTO
    your_table (column1, column2) VALUE ('test');
```
