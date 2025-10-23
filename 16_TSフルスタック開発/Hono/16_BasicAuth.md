# Basic Auth Middleware

作成日 2025/10/16

## 1. 公式サイト（英語）を読む

[Basic Auth Middleware - Hono](https://hono.dev/docs/middleware/builtin/basic-auth)

## 2. [vite-react-template](../CloudflareWrokers/12_vite-react-template.md)に組み込んでみる

新しいプロジェクトを作成する

```bash
node -v
# v22.14.0
npm -v
# 11.4.2

npm create cloudflare@latest -- --template=cloudflare/templates/vite-react-template
# dir ./anpan
# no git
# no deploy

cd anpan
npm run dev # open browser to http://localhost:5173/
```

src/worker/index.ts

```javascript
import { Hono } from "hono";
import { basicAuth } from "hono/basic-auth";

const app = new Hono<{ Bindings: Env }>();

app.use(
    "/api/*",
    basicAuth({
        username: "anpan",
        password: "anpan"
    })
);

app.get("/api/", (c) => c.json({ name: "Cloudflare" }));

export default app;
```

想像していた通り、静的ファイルであるindex.htmlと（コンパイル後の）index.jsへのアクセスは制限しない。`/api/`に対してfetchしたときに、ブラウザがログインダイアログを表示する。正しいユーザー名とパスワードを入力できれば、いつも通りの結果になるし、正しいユーザー名とパスワードが入力できなければ、いつまでもログインダイアログが表示され続ける。キャンセルボタンを押すとページには何も変化がない
