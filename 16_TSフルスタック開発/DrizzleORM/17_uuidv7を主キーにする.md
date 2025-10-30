# uuid v7 を主キーにする

作成日 2025/10/31

## 1. AIの返答

どちらが正しいのか？ どちらも正しいのか？ 検証が必要

```javascript
import { pgTable, uuid, timestamp } from 'drizzle-orm/pg-core';
import { v7 as uuidv7 } from 'uuid';

export const users = pgTable('users', {
    id: uuid('id').primaryKey().defaultRandom(() => uuidv7()),
    name: varchar('name', { length: 256 }).notNull(),
    createdAt: timestamp('created_at').defaultNow(),
});
```

```javascript
export const clients = pgTable("clients", {
    id: uuid("id").primaryKey().default(sql`uuid_generate_v7()`)
});
```

あらかじめ、データベースに拡張を入れておく => `CREATE extension pg_uuidv7;`

## 2. 公式サイト（英語）を読む

[uuid - npm](https://www.npmjs.com/package/uuid)

インストール => `npm install uuid`

UUIDを生成する

```javascript
import { v7 as uuidv7 } from 'uuid';

uuidv7(); // ⇨ '01695553-c90c-745a-b76f-770d7b3dcb6d'
```

## 3. uuid v7のメリット

[七夕だからUUID v7について語る #rfc - Qiita](https://qiita.com/cvusk/items/b21f4847ac09eeb7fe5c)

> UUID v7は、UUID v6と同様に時間ベースのUUIDですが、より現代的なニーズに対応するために設計されました。その特徴は、UNIXエポックタイム（ミリ秒精度）を基盤としつつ、ランダム性を含めていることです。

- ミリ秒精度のタイムスタンプにより、生成順に完全にソート可能
- タイムスタンプに加えて十分なランダムビットが含まれるため、UUID v4に近い高いユニーク性
