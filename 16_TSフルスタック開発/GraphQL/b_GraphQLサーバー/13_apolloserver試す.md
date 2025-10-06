# @apollo/serverを試す

作成日 2025/10/02

## 公式ドキュメントのGet Startedを写経する

[Get Started with Apollo Server](https://www.apollographql.com/docs/apollo-server/getting-started)

### セットアップ

```bash
mkdir bacon
cd bacon
npm init -y
npm i @apollo/server graphql
npm i -D typescript @types/node
npx tsc --init
```

package.jsonを変更 => `{"type": "module"}`

tsconfig.jsonを変更

```json
{
  "compilerOptions": {
      "rootDir": "./src",  // 追加
      "outDir": "./dist",  // 追加
      "lib": ["esnext"],   // 追加
      "types": ["node"],   // 変更
  }
}
```

### スキーマ・データ・Resolverの設定

src/index.ts

```javascript
// スキーマ
const typeDefs = `
  type Book {
    title: String
    author: String
  }
  type Query {
    books: [Book]
  }
`

// データ
const books = {
  // 略
}

// Resolver
const resolvers = {
  Query: {
    books: () => books,
  },
};
```

### サーバーの設定

src/index.ts

```javascript
import { ApolloServer } from '@apollo/server';
import { startStandaloneServer } from '@apollo/server/standalone';

const server = new ApolloServer({
  typeDefs,
  resolvers,
});

const { url } = await startStandaloneServer(server, {
  listen: { port: 4000 },
});

console.log(`🚀  Server ready at: ${url}`);
```

### デバッグする

package.jsonを変更

```json
{
  "scripts": {
    "compile": "tsc",  // 追加
    "start": "npm run compile && node ./dist/index.js"  // 追加
  }
}
```

スクリプト実行 => `npm start`

`http://localhost:4000/`を開いたら、Studio Sandboxが登場した

左枠（型定義が効いていた）

```javascript
query GetBooks {
  books {
    title
    author
  }
}
```

ボタンをクリック => 右枠

```json
{
  "data": {
    "books": [
      {
        "title": "The Awakening",
        "author": "Kate Chopin"
      },
      {
        "title": "City of Glass",
        "author": "Paul Auster"
      }
    ]
  }
}
```
