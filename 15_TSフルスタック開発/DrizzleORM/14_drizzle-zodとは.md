# drizzle-zodとは

作成日 2025/09/30

## 1. 解説記事を読む

[drizzle schemaから型を作成し、特定のフィールドを選択・除外・追加する方法](https://zenn.dev/keishi815/articles/7042d8dabea65d)

```javascript
import { userSchema } from "@/db/schema";
import { createSelectSchema, createInsertSchema } from 'drizzle-zod';

// DrizzleスキーマからZodスキーマを作成する
const UserSelectSchema = createSelectSchema(userSchema);

// 特定の情報だけを選ぶ（pickを使う）
const SelectedUserSchema = UserSelectSchema.pick({
  id: true,
  name: true
});

// 秘密の情報を抜く（omitを使う）
const WithoutPasswordSchema = UserSelectSchema.omit({
  password: true
});


// 新しい型を作成する
const extendedUserSchema = {
  ...userSchema,
  role: 'string'
};

// 新しいスキーマを作成する
const ExtendedUserInsertSchema = createInsertSchema(extendedUserSchema);
```

### createSelectSchemaとcreateInsertSchemaの違いとは？

- createSelectSchema ... SELECTクエリーで受け取ったデータをチェックする
- createInsertSchema ... INSERTクエリーに送るデータをチェックする

## 2. 公式サイト（英語）を読む

[Drizzle ORM - drizzle-zod](https://orm.drizzle.team/docs/zod)

```bash
# インストールする
npm i drizzle-zod
```
