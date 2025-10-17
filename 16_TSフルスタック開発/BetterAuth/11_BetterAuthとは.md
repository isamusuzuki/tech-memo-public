# BetterAuthとは

作成日 2025/10/16

## 1. 公式サイト（英語）を読む

[Better Auth](https://www.better-auth.com/)

> The most comprehensive authentication framework for TypeScript.

## 2. Cloudflare Workers向けのセットアップガイドを発見

[Better Auth on Cloudflare - Hono](https://hono.dev/examples/better-auth-on-cloudflare)

> A TypeScript-based lightweight authentication service optimized for Cloudflare Workers
>
> Stack Summary
>
>- Hono
>- Better Auth
>- Drizzle ORM
>- Posetgres with Neon

ソースコード => [bytaesu/cloudflare-auth-worker: A TypeScript-based lightweight authentication service optimized for Cloudflare Workers](https://github.com/bytaesu/cloudflare-auth-worker)

src/index.ts

```typescript
import { Hono } from 'hono';
import { auth } from './lib/better-auth';

const app = new Hono<{ Bindings: CloudflareBindings }>();

app.on(['GET', 'POST'], '/api/*', (c) => {
  return auth(c.env).handler(c.req.raw);
});

export default app;
```

src/lib/better-auth/index.ts

```typescript
import { neon } from '@neondatabase/serverless';
import { drizzle } from 'drizzle-orm/neon-http';
import { drizzleAdapter } from 'better-auth/adapters/drizzle';
import { betterAuth } from 'better-auth';
import { betterAuthOptions } from './options';

export const auth = (env: CloudflareBindings) => {
  const sql = neon(env.DATABASE_URL);
  const db = drizzle(sql);

  return betterAuth({
    ...betterAuthOptions,
    database: drizzleAdapter(db, { provider: 'pg' }),
    baseURL: env.BETTER_AUTH_URL,
    secret: env.BETTER_AUTH_SECRET,
  });
};
```

コードはシンプルでサーバーサイドしかない。クライアントサイドのコードも見たい
