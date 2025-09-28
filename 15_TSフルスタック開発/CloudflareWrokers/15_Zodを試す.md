# Zodを試す

作成日 2025/09/28

## 1. 解説動画を写経する

[【Cloudflare Workers】誰でも簡単に始められる！Hono+Cloudflare D1+Drizzle ORMで株式投資取引APIを作る](https://www.youtube.com/watch?v=nVuZiBAXJo0)

## 2. 新規プロジェクトを作成する

```bash
npm create hono@latest stock-trade-api

cd stock-trade-api
npm run dev #=> Open browser to http://localhost:8787/
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

## 3.  D1データベースを用意する

```bash
npx wrangler d1 create stock-trade-db
# 成功するとwrangler.jsoncにD1データベースの識別子が追記される
```
