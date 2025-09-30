# Zod OpenAPI Hono

作成日 2025/09/02

## 1. 解説記事を読む

[少人数で3つのWebアプリを支える技術 - Hono × Cloudflare で実現する最高のDeveloper Experience](https://zenn.dev/miravy/articles/d5c59c27f01b4e)

> バックエンドもフロントエンドもTypeScriptで統一することで、全体の開発効率を高めたいと考えていました。
>
> 様々なサービスを検討した結果、Cloudflare Workers + Hono + TypeScriptモノレポという組み合わせにたどり着きました。
>
> HonoとTypeScriptで統一した理由: HonoはTypeScriptの型定義が充実しており、開発体験が素晴らしかったこと。そして、zod-openapiとの統合により、OpenAPIスキーマから型を自動生成できることが魅力的でした。

## 2. Zodとは？

公式サイト（英語） => [Intro | Zod](https://zod.dev/)

> TypeScript-first schema validation with static type inference

```javascript
import * as z from "zod";

const User = z.object({
    name: z.string(),
});

const input = { /* 中身不明なデータ */};

// バリデーション
const data = User.parse(input);

// 型安全を達成
console.log(data.name);
```

## 3. Zod to OpenAPIとは？

[zod-openapi - npm](https://www.npmjs.com/package/zod-openapi)

> A TypeScript library which uses Zod schemas to generate OpenAPI v3.x documentation.

ZodスキーマからダイレクトにOpenAPI仕様書を生成することで、SSoT(Single Source of Truth)を実現する

```javascript
const UserSchema = z
  .object({
    id: z.string().openapi({ example: '1212121' }),
    name: z.string().openapi({ example: 'John Doe' }),
    age: z.number().openapi({ example: 42 }),
  })
  .openapi('User');

registry.registerPath({
  method: 'get',
  path: '/users/{id}',
  summary: 'Get a single user',
  request: {
    params: z.object({ id: z.string() }),
  },

  responses: {
    200: {
      description: 'Object with user data.',
      content: {
        'application/json': {
          schema: UserSchema,
        },
      },
    },
  },
});
```

## 4. Zod OpenAPI Honoとは？

GitHubリポジトリ => [honojs/middleware/zod-openapi](https://github.com/honojs/middleware/tree/main/packages/zod-openapi)

Zodを使って値と型をバリデーションしつつ、OpenAPIのドキュメントを生成できる

## 5. Drizzleとは？

公式サイト（英語） => [Drizzle ORM - next gen TypeScript ORM.](https://orm.drizzle.team/)

[drizzle-zod](https://orm.drizzle.team/docs/zod)というプラグインを使うと、DrizzleスキーマからZodスキーマを生成できる

## 6. Orvalとは？

公式サイト（英語） => [orval - Restful client generator](https://orval.dev/)

OpenAPIスキーマから、TypeScriptのAPIクライアントを自動生成するツール
