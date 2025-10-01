# Zodを試す

作成日 2025/09/30

## 1. 解説動画を写経する（続き）

[【Cloudflare Workers】誰でも簡単に始められる！Hono+Cloudflare D1+Drizzle ORMで株式投資取引APIを作る](https://www.youtube.com/watch?v=nVuZiBAXJo0)

26:57から

## 2. テーブルを追加する

config/regex.ts

```javascript
export const codeRule = "([1][3-9ACDF-HJ-NPR-UW-Y][0-9][0-9ACDF-HJ-NPR-UW-Y]|[2-9][0-9ACDF-HJ-NPR-UW-Y][0-9][0-9ACDF-HJ-NPR-UW-Y])";

export const codeRegex = new RegExp(codeRule);
```

db/validationSchema/stock-schema.ts -> lib/validations/stock.ts

```javascript
import { z } from "zod";
import { coreRegex } from '../../config/regex';
 
export const stockSchema = z.object({
    code: z.string().regex(codeRegex),
    stockName: z.string().trim().min(2).max(128),
    market: z.enum(["プライム", "スタンダード", "グロース"]),
});
```

db/schema.ts -> drizzle/schema.ts

```javascript
import { integer, real, sqliteTable, text } from "drizzle-orm/sqlite-core";

export const stockTable = sqliteTable("stock_table", {
    code: text("code", { length: 4 }).primaryKey(),
    stockName: text("stock_name").notNull(),
    market: text("market", {
        enum: ["プライム", "スタンダード", "グロース"]
    }).notNull(),
});

export const tradeTable = sqliteTable("trade_table", {
    id: integer("id").primaryKey({ autoIncrement: true }),
    code: text("code", { length: 4 }).references(() => stockTable.code).notNull(),
    shares: integer("shares").notNull(),
    price: real("price").notNull(),
    buySell: text("buy_sell", { enum: ["買", "売"] }).notNull(),
    tradingDate: text("trading_date").notNull(),
})
```

```bash
# マイグレーションファイルを生成する
npx drizzle-kit generate

# 開発用データベースにテーブルを追加する
npx wrangler d1 migrations apply stock-db --local
```

lib/validations/trade.ts

```javascript
import { z } from "zod";
import { coreRegex } from '../../config/regex';
 
export const tradeSchema = z.object({
    id: z.number().int().nonnegative(),
    code: z.string().regex(codeRegex),
    shares: z.number().nonnegative(),
    price: z.number().nonnegative(),
    buySell: z.enum(["買", "売"]),
    tradingDate: z.string().date(),
});

export const createTradeSchema=tradeSchema.omit({ id: true });
```

src/trade/index.ts

```javascript

```

src/index.ts

```javascript
import { Hono } from 'hono';
import stock from './stock';
import trade from './trade';

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
```

36:25まで
