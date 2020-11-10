# NULL 値のときの対処

作成日 2020/11/10

[MySQL :: MySQL 5\.6 リファレンスマニュアル :: 12\.1 関数と演算子のリファレンス](https://dev.mysql.com/doc/refman/5.6/ja/func-op-summary-ref.html)

- `COALESCE(value,...)` ... リストの最初の非NULL値を返す。非NULL値がない場合はNULLを返す
- `IFNULL(expr1, expr2)` ... expr1 が NULLの場合は expr2 を返す

```sql
SELECT COALESCE(NULL,1);
-- 1
SELECT COALESCE(NULL,NULL,NULL);
-- NULL

SELECT IFNULL(1, 0);
-- 1
SELECT IFNULL(NULL, 10);
-- 10
```
