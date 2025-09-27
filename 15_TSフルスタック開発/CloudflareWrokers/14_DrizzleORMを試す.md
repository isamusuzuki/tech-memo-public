# Drizzle ORMを試す

作成日 2025/09/27

## 1. 解説動画を写経する

[【Cloudflare Workers】Hono＋Cloudflare D1＋Drizzle ORM 30分でREST APIを作成](https://www.youtube.com/watch?v=kCwHEXqoPsE)

## 2. 新規プロジェクトとデータベースを作成する

```bash
mkdir todo-api-drizzle
cd todo-api-drizzle
npm create cloudflare@latest .
# dir ./.
# category Hello World example
# type Worker only
# lang TypeScript
# no git
# no deploy via 'npm run deploy'

npx wrangler d1 create todo-api-drizzle
# wrangler.jsoncファイルにD1データベースの識別子が書き込まれる
```

## 3. Drizzleを導入する

```bash
npm install drizzle-orm
npm install -D drizzle-kit
```

./drizzle.config.ts

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

pacakge.jsonファイルのscript項目に追加する

```json
{
    "scripts": {
        "generate": "drizzle-kit generate:sqlite",
        "migrate": "wrangler d1 migrations apply todo-api-drizzle --local"
    }
}
```

./db/schema.ts

```javascript
import { sql } from "drizzle-orm";
import { integer, sqliteTable, text } from "drizzle-orm/sqlite-core";

export const todos = sqliteTable("todos", {
    id: integer("id", { mode: "number" }).primaryKey({ autoIncrement: true }),
    todo: text("todo").notNull(),
    score: integer("score", { mode: "number" }).default(0),
    isDone: integer("is_done", { mode: "boolean" }).default(false),
    createdAt: text("created_at").default(sql"CURRENT_TIMESTAMP"),
});
```

コマンド実行

```bash
npm run generate
# => drizzle/migrationsフォルダが作成され、その中にSQLファイルが生成される

npm run migrate
# => ローカル環境のデータベースにテーブルが作成される
```

./db/dummy-data.sql

```sql
INSERT INTO todos (todo) VALUES ("タスク1"), ("タスク2"), ("タスク3");
```

```bash
npx wrangler d1 execute todo-api-drizzle --local --file=./db/dummy-data.sql
```

## 4. Honoを導入して、APIを作成する

```bash
npm install hono
```

./src/index.ts

```javascript
import { eq } from "drizzle-orm";
import { drizzle } from "drizzle-orm/d1";
import { Hono } from "hono";
import { todos } from "../db/schema";

export interface Env {
    todo-api-drizzle: D1Database;
}

const app = new Hono<{ Bindings: Env }>().basePath("/api");

// 全件取得
app.get("/todos", async (c) => {
    try {
        const db = drizzle(c.env.todo-api-drizzle);
        // 全てのカラムを表示する場合
        // const results = await db.select().from(todos);
        // 表示するカラムを指定する場合
        const results = await db.select({ todo: todos.todo }).from(todos);
        return c.json(results);
    } catch (e) {
        return c.json({ err: e }, 500);
    }
});

// 1件取得
app.get("/todos/:id", async (c) => {
    const id = parseInt(c.req.param("id"));
    try {
        const db = drizzle(c.env.todo-api-drizzle);
        const results = await db.select().from(todos).where(eq(todos.id, id));
        return c.json(results);
    } catch (e) {
        return c.json({ err: e }, 500);
    }
})

// 1件登録
app.post("/todos", async (c) => {
    const todo = await c.req.json<typeof todos.$inferInsert>();
    try {
        const db = drizzle(c.env.todo-api-drizzle);
        await db.insert(todos).values(todo);
        return c.json({ message: "success" }, 201);
    } catch (e) {
        return c.json({ err: e }, 500);
    }
});

// 1件削除
app.delete("/todos/:id", async (c) => {
    const id = parseInt(c.req.param("id"));
    try {
        const db = drizzle(c.env.todo-api-drizzle);
        await db.delete(todos).where(eq(todos.id, id));
        return c.json({ message: "success" }, 200);
    } catch (e) {
        return c.json({ err: e }, 500);
    }
});

// 1件更新
app.put("/todos/:id", async (c) => {
    const id = parseInt(c.req.param("id"));
    const { todo, score, isDone } = await c.req.json<typeof todos.$inferInsert>();
    try {
        const db = drizzle(c.env.todo-api-drizzle);
        await db.update(todos).set({ todo, score, isDone }).where(eq(todos.id, id));
        return c.json({ message: "success" }, 200);
    } catch (e) {
        return c.json({ err: e }, 500);
    }
});

export default app;
```

## 5. Cloudflareにデプロイする

```bash
npx wrangler d1 migrations apply todo-api-drizzle --remote
npx wrangler d1 execute todo-api-drizzle --remote --file=./db/dummy-data.sql
npm run deploy
# 成功するとURLが表示される
```
