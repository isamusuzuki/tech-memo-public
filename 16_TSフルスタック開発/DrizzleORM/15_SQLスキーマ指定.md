# SQLスキーマを指定する

作成日 2025/10/07

## 公式ガイド（英語）を読む

[Table Schemas](https://orm.drizzle.team/docs/schemas)

Drizzle ORMは、PostgreSQLやMySQLとの接続で、SQLスキーマを宣言できるAPIを用意している

```javascript
import { serial, text, pgSchema } from "drizzle-orm/pg-core";

export const mySchema = pgSchema("my_schema");

export const mySchemaUsers = mySchema.table('users', {
  id: serial('id').primaryKey(),
  name: text('name'),
});
```

ただしpublicスキーマを使う場合は、デフォルトなので、スキーマを指定しない

```javascript
import { serial, text, pgTable } from "drizzle-orm/pg-core";

export const mySchemaUsers = pgTable.table('users', {
  id: serial('id').primaryKey(),
  name: text('name'),
});
```
