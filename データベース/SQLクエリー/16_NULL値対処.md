# NULL 値のときの対処

作成日 2020/11/10

- `COALESCE(value,...)` ... リストの最初の非 NULL 値を返す。非 NULL 値がない場合は NULL を返す
- `IFNULL(expr1, expr2)` ... expr1 が NULL の場合は expr2 を返す

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
