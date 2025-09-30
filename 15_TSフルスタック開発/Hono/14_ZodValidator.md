# Zod Validator

作成日 2025/09/30

## バリデータがエラーだったときの対処

[12_Zodを試すa](../Zod/12_Zodを試すa.md) 160行目のsrc/stock/index.tsのコードで、Zod Validatorが登場する

```javascript
import { drizzle } from 'drizzle-orm/d1';
import { Hono } from 'hono';
import { stockTable } from "../../db/schema";
import { stockSchema } from "../../db/validationSchema/stock-schema";
import { zValidator } from '@hono/zod-validator';

const stock = new Hono<{ Bindings: { stock_db: D1Database } }>();

stock.post("/", zValidator("json", stockSchema), async (c) => {
    const stock = c.req.valid("json");
    try {
        const db = drizzle(c.env.stock_db);
        await db.insert(stockTable).values(stock);
        return c.json({ message: "登録しました" }, 201);
    } catch (e) {
        return c.json({ error: "すでに登録されています" }, 500);
    }
});

export default stock;
```

Zod Validatorが失敗したときの対処がなかったので調べてみた

[Error handling in Validator](https://hono.pages.dev/examples/validator-error-handling)

```javascript
app.post(
  '/users/new',
  zValidator('json', userSchema, (result, c) => {
    if (!result.success) {
      return c.text('Invalid!', 400)
    }
  }),
  async (c) => {
    const user = c.req.valid('json')
    console.log(user.name) // string
    console.log(user.age) // number
  }
)
```

zValidatorの第3引数に関数が入ることを知った
