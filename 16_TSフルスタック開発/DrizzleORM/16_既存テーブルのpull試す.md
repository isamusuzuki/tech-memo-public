# 既存テーブルのpullを試す

作成日 2025/10/09

## 1. プロジェクトのセットアップ

```bash
mkdir gohan
cd gohan
npm init -y

npm i -D typescript @types/node
npx tsc --init
```

package.jsonを変更 => `{"type": "module"}`

tsconfig.jsonを編集する

```json
{
    "compilerOptions": {
        "rootDir": "./src",  // コメント外す
        "outDir": "./dist",  // コメント外す
        "module": "esnext",  // <= nodenext
        "moduleResolution": "bundler",  // NEW
        // "types": [],      // コメントアウト
        "lib": ["esnext"],   // コメント外す
        "types": ["node"]    // コメント外す
    },
    "include": ["src"],
    "exclude": ["node_modules", "dist", "drizzle.config.ts"]
}
```

## 2. Drizzle ORMのセットアップ

```bash
npm i drizzle-orm dotenv pg
npm i -D drizzle-kit @types/pg
```

.envファイルにDB接続情報を書く

```bash
DATABASE_URL=postgres://{username}:{password}@{address}:5432/{dbname}
```

drizzle.config.tsを作成する

```javascript
import 'dotenv/config';
import { defineConfig } from 'drizzle-kit';

export default defineConfig({
    dialect: 'postgresql',
    out: './src/drizzle',
    dbCredentials: {
        url: process.env.DATABASE_URL!,
    },
    schemaFilter: ['alpha'], // DBスキーマを指定する
    tablesFilter: ['tbl_log'],  // テーブル名を指定する
});
```

### 生成されるschema.tsの場所を指定したい

[Drizzle Kit configuration file > out](https://orm.drizzle.team/docs/drizzle-config-file#out)

SQLマイグレーションファイル、スキーマのJSONスナップショット、`drizzle-kit pull`コマンドが生成する`schema.ts`の出力先フォルダを指定する

デフォルト値: `drizzle`

## 3. スクリプト実行

```bash
npx drizzle-kit pull
# No config path provided, using default 'drizzle.config.ts'
# Reading config file 'C:\Users\isuzuki\workspaces\graphql-dojo\gohan\drizzle.config.ts'
#Pulling from ['alpha'] list of schemas
#
# Using 'pg' driver for database querying
# [✓] 1  tables fetched
# [✓] 5  columns fetched
# [✓] 0  enums fetched
# [✓] 0  indexes fetched
# [✓] 0  foreign keys fetched
# [✓] 0  policies fetched
# [✓] 0  check constraints fetched
# [✓] 0  views fetched
#
# [✓] Your SQL migration file ➜ src\drizzle\0000_jazzy_jane_foster.sql 🚀
# [✓] Your schema file is ready ➜ src\drizzle\schema.ts 🚀
# [✓] Your relations file is ready ➜ src\drizzle\relations.ts 🚀
```

src/drizzle/schema.ts

```javascript
import { pgTable, pgSchema, bigserial, timestamp, varchar, bigint } from "drizzle-orm/pg-core"
import { sql } from "drizzle-orm"

export const alpha = pgSchema("alpha");

export const tblLogInAlpha = alpha.table("tbl_log", {
 id: bigserial({ mode: "bigint" }).primaryKey().notNull(),
 datetime: timestamp({ mode: 'string' }),
 jobtype: varchar({ length: 30 }),
 username: varchar({ length: 30 }),
 bikou: varchar({ length: 255 }),
});
```

src/index.ts

```javascript
import 'dotenv/config';
import { desc } from 'drizzle-orm';
import { drizzle } from 'drizzle-orm/node-postgres';
import { Pool } from 'pg';
import { tblLogInAlpha } from './drizzle/schema.js';

const pool = new Pool({
    connectionString: process.env.DATABASE_URL,
});

async function fetchData() {
    try {
        const db = drizzle({ client: pool });
        const result = await db.select().from(tblLogInAlpha).orderBy(desc(tblLogInAlpha.id)).limit(10);
        if (result.length === 0) {
            console.log('No data found');
            return;
        }
        console.log(result);
    } catch (error) {
        console.error('Error fetching data:', error);
    }
}

fetchData();
```

package.jsonを編集

```json
{
    "scripts": {
        "compile": "npx tsc",
        "start": "npm run compile && node dist/index.js"
    },
}
```

スクリプト実行

```bash
npm start
# => データ10件取得成功
```
