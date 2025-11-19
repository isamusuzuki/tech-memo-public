# uuid v7 を主キーにする

作成日 2025/10/31、更新日 2025/11/19

## 1. AIの返答

どちらが正しいのか？ どちらも正しいのか？ 検証が必要

### ひとつめ

```javascript
import { pgTable, uuid, timestamp } from 'drizzle-orm/pg-core';
import { v7 as uuidv7 } from 'uuid';

export const users = pgTable('users', {
    id: uuid('id').primaryKey().defaultRandom(() => uuidv7()),
    name: varchar('name', { length: 256 }).notNull(),
    createdAt: timestamp('created_at').defaultNow(),
});
```

### ふたつめ

```javascript
export const clients = pgTable("clients", {
    id: uuid("id").primaryKey().default(sql`uuid_generate_v7()`)
});
```

あらかじめデータベースに拡張を入れておく必要あり => `CREATE extension pg_uuidv7;`

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

## 4. PostgreSQLのuuid v7サポート状況

[PostgreSQL 18がUUIDv7をサポート](https://masahikosawada.github.io/2025/09/04/UUIDv7-in-PostgreSQL/)

> PostgreSQL 17現在、UUIDv7の生成をサポートしていないので、UUIDv7を利用したい場合は、公開されているextensionを利用する、もしくは自分で実装する必要があります。
>
> PostgreSQL 18では`uuidv7()`SQL関数が導入されるため、すべてのPostgreSQLがユーザがUUIDv7を利用できるようになります！

### Extensionについて調べる

[pg_uuidv7: Create UUIDv7 values in Postgres / PostgreSQL Extension Network](https://pgxn.org/dist/pg_uuidv7/)

> Quickstart
>
> 1. Download the latest `.tar.gz` release and extract it to a temporary directory
> 2. Copy `pg_uuidv7.so` for your Postgres version into the Postgres module directory
> 3. Copy `pg_uuidv7--1.7.sql` and `pg_uuidv7.control` into the Postgres extension directory
> 4. Enable the extension in the database using `CREATE EXTENSION pg_uuidv7;`
