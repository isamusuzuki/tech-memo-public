# Honoとは

作成日 2025/09/08

## 1. 公式サイト（英語）を読む

[Hono - Web framework built on Web Standards](https://hono.dev/)

> Fast, lightweight, built on Web Standards. Support for any JavaScript runtime.

Node.jsに縛られないWebサーバー

## 2. 参照サイトを読む

[Honoで始める高速Webアプリ開発入門！TypeScriptフレームワークの特徴と実装例](https://tasukehub.com/articles/hono-typescript-web-framework-guide/)

> Honoの最大の特徴の一つが、様々なJavaScriptランタイム環境で同じコードを実行できる「マルチランタイム対応」です。
>
> Honoは以下のような多様なランタイム環境で動作します：
>
>- Cloudflare Workers
>- Deno
>- Bun
>- Vercel
>- AWS Lambda
>- Node.js

Node.js

```javascript
import { Hono } from 'hono'
import { serve } from '@hono/node-server'

const app = new Hono()

app.get('/', (c) => c.text('Hello Node.js!'))

// Node.jsでのサーバー起動
serve({
  fetch: app.fetch,
  port: 3000
})
```

Bun

```javascript
import { Hono } from 'hono'

const app = new Hono()

app.get('/', (c) => c.text('Hello Bun!'))

// Bunでのサーバー起動
export default {
  port: 3000,
  fetch: app.fetch
}
```
