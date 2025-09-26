# REST APIを作成する

作成日 2025/09/10、更新日 2025/09/26

解説動画を更新しつつ写経する => [【Cloudflare Workers】HonoとCloudflare D1を使って20分でREST APIを作成する](https://www.youtube.com/watch?v=XyjACmtXqj0)

## 1. プロジェクト開始

```bash
npm create cloudflare@latest
# dir ./simple-todo-api
# category Hello World example
# type Worker only
# lang TypeScript
# no git
# no deploy via 'npm run deploy'

cd simple-todo-api
npm run dev #=> Open browser to http://localhost:8787/
```

## 2. データベース作成

```bash
npx wrangler d1 create todoapi
```

- 開発コンテナを使っていると、ブラウザからのOAuthトークンの受け渡しがうまくいかない
- 打開策は、APIトークンを使用すること
- ダッシュボード ＞ アカウントの管理 ＞ アカウントAPIトークン ＞ トークンを作成する ＞ D1編集の権限を追加したトークンを作成する
- .envファイルに、`CLOUDFLARE_API_TOKEN=xxx`を書き込む
- wrangler.jsoncファイルに、`"account_id": "xxx"`を書き込む
- ダッシュボード ＞ アカウントホーム ＞ アカウント名の右横にある縦3個ドットをクリック ＞ アカウントIDをコピー

schema.sqlを作成する

```sql
DROP TABLE IF EXISTS todos;

CREATE TABLE todos (
    id INTEGER PRIMARY KEY,
    title TEXT NOT NULL,
    content TEXT NOT NULL
);

INSERT INTO todos (title, content) VALUES
("タスク1", "タスク1です"),
("タスク2", "タスク2です"),
("タスク3", "タスク3です");
```

ローカル環境にデータベースを作成し、テストする

```bash
npx wrangler d1 execute todoapi --local --file=./schema.sql
npx wrangler d1 execute todoapi --local --command='SELECT * FROM todos'
```

## 3. Honoをインストールする

```bash
npm install hono
```

src/index.tsを上書きする

```javascript
import { Hono } from "hono";

const app = new Hono();

app.get("/", (c) => c.json({ message: "Hello Cloudflare Workers!"}));

export default app;
```

thunder clientで、テストする

7:45まで
