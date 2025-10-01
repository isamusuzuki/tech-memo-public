# Zod を試す

作成日 2025/09/30

## 1. 解説動画を写経する（続き）

[【Cloudflare Workers】誰でも簡単に始められる！Hono+Cloudflare D1+Drizzle ORM で株式投資取引 API を作る](https://www.youtube.com/watch?v=nVuZiBAXJo0)

26:57 から

## 2. テーブルを追加する

config/regex.ts

```javascript
export const codeRule =
  "([1][3-9ACDF-HJ-NPR-UW-Y][0-9][0-9ACDF-HJ-NPR-UW-Y]|[2-9][0-9ACDF-HJ-NPR-UW-Y][0-9][0-9ACDF-HJ-NPR-UW-Y])";

export const codeRegex = new RegExp(codeRule);
```

db/validationSchema/stock-schema.ts -> lib/validations/stock.ts

```javascript
import { z } from "zod";
import { codeRegex } from "../../config/regex";

export const stockSchema = z.object({
  code: z.string().regex(codeRegex),
  stockName: z.string().trim().min(2).max(128),
  market: z.enum(["プライム", "スタンダード", "グロース"]),
});
```

db/schema.ts -> drizzle/schema.ts

```javascript
import { relations } from "drizzle-orm";
import { integer, real, sqliteTable, text } from "drizzle-orm/sqlite-core";

export const stockTable = sqliteTable("stock_table", {
  code: text("code", { length: 4 }).primaryKey(),
  stockName: text("stock_name").notNull(),
  market: text("market", {
    enum: ["プライム", "スタンダード", "グロース"],
  }).notNull(),
});

export const tradeTable = sqliteTable("trade_table", {
  id: integer("id").primaryKey({ autoIncrement: true }),
  code: text("code", { length: 4 })
    .references(() => stockTable.code)
    .notNull(),
  shares: integer("shares").notNull(),
  price: real("price").notNull(),
  buySell: text("buy_sell", { enum: ["買", "売"] }).notNull(),
  tradingDate: text("trading_date").notNull(),
});

export const stockRelations = relations(stockTable, ({ many }) => ({
  trades: many(tradeTable),
}));

export const tradeRelations = relations(tradeTable, ({ one }) => ({
  stock: one(stockTable, {
    fields: [tradeTable.code],
    references: [stockTable.code],
  }),
}));

export const schema = {
  stockTable,
  tradeTable,
  stockRelations,
  tradeRelations,
};
```

drizzle.config.ts

```javascript
import { defineConfig } from "drizzle-kit";

export default defineConfig({
  dialect: "sqlite",
  schema: "./drizzle/schema.ts",
  out: "./drizzle/migrations",
});
```

開発用データベースにテーブルを作成する

```bash
# マイグレーションファイルを生成する
npx drizzle-kit generate

# 開発用データベースにテーブルを追加する
npx wrangler d1 migrations apply stock-db --local
```

lib/validations/trade.ts

```javascript
import { z } from "zod";
import { codeRegex } from "../../config/regex";

export const tradeSchema = z.object({
  id: z.number().int().nonnegative(),
  code: z.string().regex(codeRegex),
  shares: z.number().nonnegative(),
  price: z.number().nonnegative(),
  buySell: z.enum(["買", "売"]),
  tradingDate: z.string().date(),
});

export const createTradeSchema = tradeSchema.omit({ id: true });
```

src/trade/index.ts

```javascript
import { eq } from 'drizzle-orm';
import { drizzle } from 'drizzle-orm/d1';
import { Hono } from 'hono';
import { tradeTable, schema } from "../../drizzle/schema";
import { createTradeSchema } from "../../lib/validations/trade";
import { zValidator } from '@hono/zod-validator';

const trade = new Hono<{ Bindings: { stock_db: D1Database } }>();

trade
    .get("/", async (c) => {
        try {
            const db = drizzle(c.env.stock_db, { schema });
            const res = await db.query.tradeTable.findMany({
                with: { stock: true },
            });
            if (res.length === 0) {
                return c.json({ error: "登録データがありません" }, 404)
            }
            return c.json(res);
        } catch (e) {
            return c.json({ error: e }, 500);
        }
    }).post("/", zValidator("json", createTradeSchema), async (c) => {
        const trade = c.req.valid("json");
        try {
            const db = drizzle(c.env.stock_db);
            await db.insert(tradeTable).values(trade);
            return c.json({ message: "登録しました" }, 201);
        } catch (e: unknown) {
            if (e instanceof Error) {
                return c.json({ error: e.message }, 500);
            } else {
                return c.json({ error: e }, 500);
            }
        }
    }).get(`/:id`, async (c) => {
        const id = parseInt(c.req.param("id"));
        try {
            const db = drizzle(c.env.stock_db, { schema });
            const res = await db.query.tradeTable.findFirst({
                columns: { code: true, shares: true, price: true },
                with: { stock: { columns: { stockName: true, market: true } } },
                where: eq(tradeTable.id, id),
            });
            if (!res) {
                return c.json({ error: "登録データがありません" }, 404)
            }
            return c.json(res);
        } catch (e) {
            return c.json({ error: e }, 500);
        }
    }).put(`/:id`, zValidator("json", createTradeSchema), async (c) => {
        const id = parseInt(c.req.param("id"));
        const trade = c.req.valid("json");
        try {
            const db = drizzle(c.env.stock_db);
            await db.update(tradeTable).set(trade).where(eq(tradeTable.id, id));
            return c.json({ message: "更新しました" }, 200);
        } catch (e) {
            return c.json({ error: e }, 500);
        }
    }).delete(`/:id`, async (c) => {
        const id = parseInt(c.req.param("id"));
        try {
            const db = drizzle(c.env.stock_db);
            await db.delete(tradeTable).where(eq(tradeTable.id, id));
            return c.json({ message: "削除しました" }, 200);
        } catch (e) {
            return c.json({ error: e }, 500);
        }
    });

export default trade;
```

src/index.ts

```javascript
import { Hono } from "hono";
import stock from "./stock";
import trade from "./trade";

const app = new Hono().basePath("/api");

app.route("stocks", stock);
app.route("trades", trade);

export default app;
```

drizzle/dummy-data.sql

```sql
INSERT INTO
    stock_table (code, stock_name, market)
VALUES
    ("1P01", "P株式会社", "プライム"),
    ("1S01", "S株式会社", "スタンダード"),
    ("1G01", "G株式会社", "グロース");

INSERT INTO
    trade_table (code, shares, price, buy_sell, trading_date)
VALUES
    ("1P01", 200, 2000, "買", "2025-09-10"),
    ("1S01", 300, 1000, "買", "2025-09-20"),
    ("1G01", 400, 500, "買", "2025-09-30");
```

ダミーデータを登録する

```bash
npx wrangler d1 execute stock-db --local --file=./drizzle/dummy-data.sql

# 検証する
npm run dev
```

本番環境にデプロイする

```bash
npm run deploy

npx wrangler d1 migrations apply stock-db --remote
```
