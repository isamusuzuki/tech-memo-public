# Zod OpenAPI Hono

作成日 2025/10/07

## 1. 公式ガイド（英語）を読む

[Zod OpenAPI - Hono](https://hono.dev/examples/zod-openapi)

Zod OpenAPI Honoは、Honoの拡張クラスで、OpenAPIをサポートする。Zodを使って値と型をバリデートしつつ、OpenAPIのSwaggerドキュメントを生成できる

ステップ1: Zodでスキーマを定義する

```javascript
import { z } from '@hono/zod-openapi'

const ParamsSchema = z.object({
    id: z.string().min.(3).openapi(/*略*/)
})

const UserSchema = z.object({
    id: z.string().openapi(/*略*/),
    name: z.string().openapi(/*略*/),
    age: z.number().openapi(/*略*/),
}).openapi('user')
```

## 2. リポジトリのREADMEを読む

[middleware/packages/zod-openapi at main · honojs/middleware](https://github.com/honojs/middleware/tree/main/packages/zod-openapi)

## 3. 解説記事を写経する

[Hono × Zod-OpenAPIで快適API開発](https://zenn.dev/slowhand/articles/b7872e09b84e15)

### プロジェクトの準備

```bash
# ルートプロジェクトのワークスペースとしてプロジェクトを開始する
npm init -w bakabon -y
cd bakabon

# Honoテンプレートを利用する
npm create hono@latest .
# ✔ Using target directory … .
# ✔ Which template do you want to use? cloudflare-workers
# ✔ Directory not empty. Continue? Yes
# ✔ Do you want to install project dependencies? Yes
# ✔ Which package manager do you want to use? npm

npm i -D typescript
npm i -D @types/node
npm i -D @hono/zod-openapi
npm i -D @hono/swagger-ui
```

### 作成するAPIの概要

- `GET /api/tasks` ... タスクのリストを取得
- `POST /api/tasks` ... 新しいタスクを作成

途中
