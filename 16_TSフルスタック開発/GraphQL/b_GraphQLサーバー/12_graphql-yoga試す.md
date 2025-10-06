# graphql-yogaを試す

作成日 2025/10/02

## 公式ドキュメントのQuick startを写経する

[Quick start | Yoga](https://the-guild.dev/graphql/yoga-server/docs)

### セットアップ

```bash
mkdir anpan
cd anpan
npm init -y
npm i graphql-yoga graphql
```

package.jsonを変更 => `{"type": "module"}`

### スキーマの定義

schema.js

```javascript
import { createSchema } from 'graphql-yoga'

export const schema = createSchema({
  typeDefs: /* GraphQL */ `
    type Query {
      hello: String
    }
  `,
  resolvers: {
    Query: {
      hello: () => 'Hello, world'
    }
  }
})
```

### サーバーの設定

```javascript
import { createServer } from 'node:http'
import { createYoga } from 'graphql-yoga'
import { schema } from './schema'

// Create a Yoga instance with a GraphQL schema.
const yoga = createYoga({ schema })

// Pass it into a server to hook into request handlers.
const server = createServer(yoga)

// Start the server and you're done!
server.listen(4000, () => {
  console.info('Server is running on http://localhost:4000/graphql')
})
```

### デバッグする

package.jsonを変更 => `{"scripts": {"dev": "node index.js"}}`

スクリプト実行 => `npm run dev`

`http://localhost:4000/graphql`を開いたら、Yoga GraphiQLが登場した

左枠

```javascript
query HelloQuery {
  hello
}
```

ボタンをクリック => 右枠

```json
{
  "data": {
    "hello": "Hello, world!"
  }
}
```
