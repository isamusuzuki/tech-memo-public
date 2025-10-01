# Zodを試す

作成日 2025/09/28、更新日 2025/09/29

## 1. 解説動画を写経する

[【Cloudflare Workers】誰でも簡単に始められる！Hono+Cloudflare D1+Drizzle ORMで株式投資取引APIを作る](https://www.youtube.com/watch?v=nVuZiBAXJo0)

## 2. 新規プロジェクトを作成する

【変更点】`ban create hono@latest`は未検証であるが、少なくとも`npm create hono@latest`はTypeScriptがインストールされず、Cloudflareサービス群の型定義ファイルも欠けていて、続行できない。代わりに`npm create cloudflare@latest`を使う

```bash
npm create cloudflare@latest stock-trade-api
# category Hello World example
# type Worker only
# lang TypeScript
# no git
# no deploy via 'npm run deploy'

# Honoを導入
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

drizzle.config.tsを作成する

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

# データベースにテーブルを追加する
npx wrangler d1 migrations apply stock-db --local
```

## 4. APIを作成する

src/index.ts

```javascript
import { Hono } from 'hono';
import stock from './stock';

const app = new Hono().basePath("/api");

app.route("stocks", stock);

export default app;
```

src/stock/index.ts

```javascript
import { eq } from 'drizzle-orm';
import { drizzle } from 'drizzle-orm/d1';
import { Hono } from 'hono';
import { stockTable } from "../../db/schema";

const stock = new Hono<{ Bindings: { stock_db: D1Database } }>();

stock
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

export default stock;
```

## 5. Zodを導入する

```bash
npm i zod
npm i @hono/zod-validator
```

db/validationSchema/stock-schema.ts

```javascript
import { z } from "zod";

export const codeRule = "([1][3-9ACDF-HJ-NPR-UW-Y][0-9][0-9ACDF-HJ-NPR-UW-Y]|[2-9][0-9ACDF-HJ-NPR-UW-Y][0-9][0-9ACDF-HJ-NPR-UW-Y])";

const codeRegex = new RegExp(codeRule);
 
export const stockSchema = z.object({
    code: z.string().regex(codeRegex),
    stockName: z.string().trim().min(2).max(128),
    market: z.enum(["プライム", "スタンダード", "グロース"]),
});
```

src/stock/index.ts

```javascript
import { eq } from 'drizzle-orm';
import { drizzle } from 'drizzle-orm/d1';
import { Hono } from 'hono';
import { stockTable } from "../../db/schema";
import { stockSchema, codeRule } from "../../db/validationSchema/stock-schema";
import { zValidator } from '@hono/zod-validator';

const stock = new Hono<{ Bindings: { stock_db: D1Database } }>();

stock
    .get("/", async (c) => {
        try {
            const db = drizzle(c.env.stock_db);
            const res = await db.select().from(stockTable);
            if (res.length === 0) {
                return c.json({ error: "登録データがありません" }, 404)
            }
            return c.json(res);
        } catch (e) {
            return c.json({ error: e }, 500);
        }
    }).post("/", zValidator("json", stockSchema), async (c) => {
        const stock = c.req.valid("json");
        try {
            const db = drizzle(c.env.stock_db);
            await db.insert(stockTable).values(stock);
            return c.json({ message: "登録しました" }, 201);
        } catch (e) {
            return c.json({ error: "すでに登録されています" }, 500);
        }
    }).get(`/:code{${codeRule}}`, async (c) => {
        const code = c.req.param("code");
        try {
            const db = drizzle(c.env.stock_db);
            const res = await db.select().from(stockTable).where(eq(stockTable.code, code));
            if (res.length === 0) {
                return c.json({ error: "登録データがありません" }, 404)
            }
            return c.json(res);
        } catch (e) {
            return c.json({ error: e }, 500);
        }
    }).put(`/:code{${codeRule}}`, zValidator("json", stockSchema), async (c) => {
        const code = c.req.param("code");
        const stock = c.req.valid("json");
        try {
            const db = drizzle(c.env.stock_db);
            await db.update(stockTable).set(stock).where(eq(stockTable.code, code));
            return c.json({ message: "更新しました" }, 200);
        } catch (e) {
            return c.json({ error: e }, 500);
        }
    }).delete(`/:code{${codeRule}}`, async (c) => {
        const code = c.req.param("code");
        try {
            const db = drizzle(c.env.stock_db);
            await db.delete(stockTable).where(eq(stockTable.code, code));
            return c.json({ message: "削除しました" }, 200);
        } catch (e) {
            return c.json({ error: e }, 500);
        }
    });

export default stock;
```

26:56まで
