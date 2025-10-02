# @apollo/serverを試す

作成日 2025/10/02

## 公式ドキュメントのGet Startedを写経する

[Get Started with Apollo Server](https://www.apollographql.com/docs/apollo-server/getting-started)

### セットアップ

```bash
mkdir apple
cd apple
npm init -y
npm i @apollo/server graphql
npm i -D typescript @types/node
npx tsc --init
```

package.jsonを変更 => `{"type": "module"}`

tsconfig.jsonを変更 => 3行追加、types変更

```json
{
  "compilerOptions": {
      "rootDir": "./src",
      "outDir": "./dist",
      "lib": ["esnext"],
      "types": ["node"],
  }
}
```

### スキーマの設定

src/index.ts

```javascript
const typeDefs = `
  type Book {
    title: String
    author: String
  }
  type Query {
    books: [Book]
  }
`
```

### データの設定

```javascript
const books = {
  // 略
}
```

### Resolverの設定

```javascript
const resolvers = {
  Query: {
    books: () => books,
  },
};
```

### サーバーの設定

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

package.jsonを変更 => scripts項目に2行追加

```json
{
  "scripts": {
    "compile": "tsc",
    "start": "npm run compile && node ./dist/index.js"
  }
}
```

スクリプト実行 => `npm start`

`http://localhost:4000/`を開いたら、Studio Sandboxが登場した

左枠

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
