# Hono RPCとは

作成日 2025/09/10

## 1. 紹介記事を読む

[見よ、これがHonoのRPCだ](https://zenn.dev/yusukebe/articles/a00721f8b3b92e)

サーバーサイドでAPIを書く

server/index.ts

```javascript
import { Hono } from 'hono'
import { z } from 'zod'
import { zValidate } from '@hono/zod-validator'

const app = new Hono()

const schema = z.object({
    name: z.string(),
    age: z.number()
})

const routes = app.post('/api/users', zValidator('json', schema), (c) => {
    const data = c.req.valid('json')

    return c.json({
        message: `${data.name} is ${data.age.toString()} years old.`
    })
})

export type AppType = typeof routes

export default app
```

クライアントはサーバーから型をインポートする

client/index.ts

```javascript
import { hc } from 'hono/client'
import { AppType } from './server'

const client = hc<AppType>('/')

const res = await client.api.users.$post({
    'json': {
        'name': 'young man',
        'age': 20
    }
})

const data = await res.json()

console.log(data.message)
```

## 2. 公式サイト（英語）を読む

[RPC - Hono](https://hono.dev/docs/guides/rpc)
