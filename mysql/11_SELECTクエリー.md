# SELECT クエリー

作成日 2020/11/10

## 01. WHERE 句で LIKE を使う

LIKE を使うときは `%` がワイルドカードとなる

```sql
SELECT
    *
FROM
    t_user
WHERE
    code LIKE "2528%";
```

## 02. 変数を使う

```sql
SET
    @item_code = 'cmb532';

SELECT
    *
FROM
    m_items
WHERE
    item_code = @item_code;
```

SET 以外のステートメントでは、`=`は比較演算子として扱われるので、\
割り当て演算子は`:=`を使う

```sql
SET
    @t1 = 1,
    @t2 = 2,
    @t3 = 4;

SELECT
    @t1,
    @t2,
    @t3,
    @t4 := @t1 + @t2 + @t3;
```

## 03. CASE 式を使う

```sql
SELECT
    CASE
        WHEN u.agent_flag = 0 THEN '2-店舗'
        WHEN u.agent_flag = 1 THEN '1-特約店'
        WHEN u.agent_flag = 2 THEN '3-社員'
    END AS type,
    count(l.id) AS total
FROM
    t_loginhistory l
    JOIN t_user u ON l.user_id = u.id
WHERE
    l.login_datetime >= '2019/07/01'
    AND l.login_datetime < '2019/08/01'
    AND u.code NOT IN ('9990', '9991', '9999')
GROUP BY
    type;
```
