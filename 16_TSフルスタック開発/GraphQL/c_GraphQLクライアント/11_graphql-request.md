# graphql-request

作成日 2025/10/02

## 1. 公式サイトを読む

[graphql-request - npm](https://www.npmjs.com/package/graphql-request)

> Minimal GraphQL client supporting Node and browsers for scripts or simple apps.

### インストール

```bash
npm install graphql-request graphql
```

このパッケージは`package.exports`を使う。それゆえ、TypeScriptを使用している場合は、以下のように設定する

- tsconfig.json > `{"moduleResolution": "bundler"}`
- package.json > `{"type": "module"}`

### クイックスタート

```javascript
import { gql, GraphQLClient } from 'graphql-request'

const document = gql`
  {
    company {
      ceo
    }
  }
`
const endpoint = 'https://api.spacex.land/graphql'
const client = new GraphQLClient(endpoint)
await client.request(document)
```
