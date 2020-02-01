# SQL クエリーを使う

作成日 2019/12/10

## 01. SELECT クエリー

LIKE を使うときは `%` がワイルドカードとなる

```sql
SELECT * FROM t_user WHERE code LIKE "2528%"
```

## 02. INSERT クエリー

```sql
INSERT INTO users (email, password, groupName, expiredDate)
VALUES ('user@sample.com', 'ac7acm', 'tokyo170928', '2019-07-25 03:00:00')
```

### マルチプルインサート

VALUES 後のカッコ `()` をカンマ `,` で区切って繰り返すと、
複数のデータを 1 行の SQL クエリーでインサートすることができる

```sql
INSERT INTO users (email, password, groupName, expiredDate)
VALUES ('user@sample.com', 'ac7acm', 'tokyo170928', '2019-07-25 03:00:00')
, ('user2@sample.com', 'b4c5g', 'tokyo170928', '2019-07-25 03:00:01')
, ('user3@sample.com', 'cqx1z', 'tokyo170928', '2019-07-25 03:00:02')
```

### セレクトインサート

あるテーブルでのセレクト結果を、別のテーブルにインサートすることができる

```sql
INSERT INTO items
(item_code, kanri_no, joudai_price, is_shipping_charge)
SELECT item_code, kanri_no, joudai_price, is_shipping_charge
FROM   items_temp t
WHERE  t.cursor_id = '8196db33-b775-4cda-a2bb-0c13fce6c0a5'
AND    t.is_found = 0
```

## 03. UPDATE クエリー

```sql
UPDATE users SET password = '123456', expiredDate = '2019-12-31 23:59:59'
WHERE email = 'user@sample.com'
```

### ジョインアップデート

2 つのテーブルをジョインさせて、マッチした行のデータ更新を行う

```sql
UPDATE items_temp t
INNER JOIN items i
ON t.kanri_no = i.kanri_no
SET t.is_found = 1
WHERE t.cursor_id = '8196db33-b775-4cda-a2bb-0c13fce6c0a5'

UPDATE items i
INNER JOIN items_temp t
ON i.kanri_no = t.kanri_no
SET i.joudai_price = t.joudai_price
, i.is_shipping_charge = t.is_shipping_charge
WHERE t.cursor_id = '8196db33-b775-4cda-a2bb-0c13fce6c0a5'
```

## 04. ユーザー定義変数を使う

```sql
set @item_code = 'cmb532';
delete from m_item_packing where item_code = @item_code;
delete from m_items where item_code = @item_code;
```

SET 以外のステートメントでは、`=`は比較演算子として扱われるので、\
割り当て演算子は`:=`を使う

```sql
set @t1=1, @t2=2, @t3=4;
select @t1, @t2, @t3, @t4 := @t1+@t2+@t3;
```

## 05. CASE 式を使う

```sql
select
case
  when u.agent_flag = 0 then '2-店舗'
  when u.agent_flag = 1 then '1-特約店'
  when u.agent_flag = 2 then '3-社員'
end as type
, count(l.id) as total
from t_loginhistory as l
join t_user as u on l.user_id = u.id
where l.login_datetime >= '2019/07/01'
and l.login_datetime < '2019/08/01'
and u.code not in ('9999','9999-999','9999-998','9990','9991')
group by type;
```
