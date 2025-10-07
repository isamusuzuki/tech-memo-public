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
