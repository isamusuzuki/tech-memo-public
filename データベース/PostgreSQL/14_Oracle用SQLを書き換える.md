# Oracle用SQLをPostgreSQL用に書き換える

作成日 2025/05/27

## 1. GitHub Copoilotが教えてくれた、主なポイント

- Oracleの「nvl2」はPostgreSQLでは「CASE WHEN ... THEN ... ELSE ... END」または「COALESCE」と「NULLIF」などで代用します。
- Oracleの「nvl」はPostgreSQLでは「COALESCE」に置き換えます。
- Oracleの「+」による外部結合はPostgreSQLでは「LEFT JOIN ... ON ...」に書き換えます。
- 文字列連結は「||」でOK（PostgreSQLも同じ）。
- プレースホルダやコメントはそのままでも問題ありません。

## 2. 文字列連結の「||」の解釈が違うような気がする

PostgreSQLの公式ドキュメント => [9.4. 文字列関数と演算子](https://www.postgresql.jp/document/10/html/functions-string.html)

```sql
select 'text' || '123';
-- text123

select 'test' || null;
-- [NULL]
```

Oracleの公式ドキュメント => [連結演算子](https://docs.oracle.com/cd/F19136_01/sqlrf/Concatenation-Operator.html)

> Oracleは、長さが0(ゼロ)の文字列をNULLとして処理しますが、長さが0(ゼロ)の文字列を別のオペランドと連結すると、その結果は常にもう一方のオペランドになります。結果がNULLになるのは、2つのNULL文字列を連結したときのみです。
