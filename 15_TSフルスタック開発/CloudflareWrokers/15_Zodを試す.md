# Zodを試す

作成日 2025/09/28

## 1. 解説動画を写経する

[【Cloudflare Workers】誰でも簡単に始められる！Hono+Cloudflare D1+Drizzle ORMで株式投資取引APIを作る](https://www.youtube.com/watch?v=nVuZiBAXJo0)

## 2. 新規プロジェクトを作成する

`ban create hono@latest`は未検証であるが、少なくとも`npm create hono@latest`はTypeScriptがインストールされず、Cloudflareサービス群の型定義ファイルも欠けていて、問題がある。代わりに`npm create cloudflare@latest`を使う

```bash
npm create cloudflare@latest stock-trade-api
# category Hello World example
# type Worker only
# lang TypeScript
# no git
# no deploy via 'npm run deploy'

npm install hono
```

src/index.ts

```javascript
import { Hono } from 'hono';

const app = new Hono();

app.get('/', (c) => {
  return c.json({ ok: true, message: 'Hello Hono!'});
});

export default app;
```

## 3.  D1データベースを用意して、drizzleを導入する

```bash
npx wrangler d1 create stock-db
# 成功するとwrangler.jsoncにD1データベースの識別子が追記される

npm install drizzle-orm
npm install -D drizzle-kit
```

drizzle.config.tsを新規作成する

```javascript
import { defineConfig } from "drizzle-kit";

export default defineConfig({
    dialect: "sqlite",
    schema: "./db/schema.ts",
    out: "./drizzle/migrations",
});
```

wrangler.jsoncファイルのd1_databases項目に追加する

```json
{
    "d1_databases": {
        "migrations_dir" : "drizzle/migrations"
    }
}
```

db/schema.ts

```javascript
import { sqliteTable, text } from "drizzle-orm/sqlite-core";

export const stockTable = sqliteTable("stock_table", {
    code: text("code", { length: 4 }).primaryKey(),
    stockName: text("stock_name").notNull(),
    market: text("market").notNull(),
});
```

デバッグ用のデータベースを作成する

```bash
# マイグレーションファイルを生成する
npx drizzle-kit generate

npx wrangler d1 migrations apply stock-db --local
```

## 4. APIを作成する

src/index.ts

```javascript
import { eq } from 'drizzle-orm';
import { drizzle } from 'drizzle-orm/d1';
import { Hono } from 'hono';
import { stockTable } from "../db/schema";

export interface Env {
    stock_db: D1Database;
}

const app = new Hono<{ Bindings: Env }>();

app
    .get("/", async (c) => {
        const db = drizzle(c.env.stock_db);
        const res = await db.select().from(stockTable);
        return c.json(res);
    }).post("/", async (c) => {
        const db = drizzle(c.env.stock_db);
        const stock = await c.req.json<typeof stockTable.$inferInsert>();
        const res = await db.insert(stockTable).values(stock);
        return c.json(res);
    }).delete("/:code", async (c) => {
        const db = drizzle(c.env.stock_db);
        const code = await c.req.param("code");
        const res = await db.delete(stockTable).where(eq(stockTable.code, code));
        return c.json(res);
    });

export default app;
```
