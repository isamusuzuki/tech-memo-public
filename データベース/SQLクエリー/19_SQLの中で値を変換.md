# SQLクエリーの中で値をいろいろ変換する

作成日 2022/07/22

## 日付の表示形式を変更する

DATE_FORMAT関数

- 構文: `DATE_FORMAT(date, format)`
- 内容: 対象の日付を指定のフォーマットに従って整形した文字列に変換する

使い方の例

```sql
SELECT
    mall_code,
    count(category_id) total_count,
    DATE_FORMAT(max(created_at), '%Y-%m-%d') last_updated
FROM
    mall_categories
GROUP BY
    mall_code;
```

## 数字に3桁区切りを入れる

FORMAT関数

- 構文: `FORMAT(X, D)`
- 内容: 対象の数値を指定のフォーマットの形式に整形した文字列に変換する

使い方の例

```sql
SELECT
    mall_code,
    FORMAT(count(category_id), '#,##0') total_count,
    max(created_at) last_updated
FROM
    mall_categories
GROUP BY
    mall_code;
```

## まったく別の値に変更する

CASE文を使う

使い方の例

```sql
SELECT
    CASE
        WHEN a.mall_code = 'r' THEN 1
        WHEN a.mall_code = 'y' THEN 2
        WHEN a.mall_code = 'w' THEN 3
        WHEN a.mall_code = 'p' THEN 4
        WHEN a.mall_code = 'a' THEN 5
    END AS sort_number,
    mall_code,
    count(category_id) total_count,
    max(created_at) last_updated
FROM
    mall_categories
GROUP BY
    sort_number;
```
