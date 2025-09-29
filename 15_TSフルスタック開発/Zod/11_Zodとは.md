# Zodとは

作成日 2025/09/30

## 1. 公式サイト（英語）を読む

[Intro | Zod](https://zod.dev/)

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

## 2. さまざまな変換ツールが用意されている

[zod-openapi](https://www.npmjs.com/package/zod-openapi)

> uses Zod schemas to generate OpenAPI v3.x documentation.

[drizzle-zod](https://orm.drizzle.team/docs/zod)

> generate Zod schemas from Drizzle ORM schemas.
