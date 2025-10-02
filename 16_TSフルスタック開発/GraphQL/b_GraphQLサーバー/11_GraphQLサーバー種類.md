# GraphQLサーバーの種類

作成日 2025/10/02

## 1. 解説記事を読む

[JavaScript で GraphQL サーバーの技術選定をする際の登場人物](https://www.mizdra.net/entry/2024/12/15/000000)

- GraphQLサーバーは、GraphQLクエリを受け取って、GraphQLレスポンスを返すことが仕事
- GraphQLサーバーは、外で定義されたSchema, Resolverを受け取り、それを使ってGraphQLクエリを処理する
- GraphQLサーバーは、直接HTTPリクエストを受け取って、HTTPレスポンスを返す機能がない

GraphQLサーバー

- [@apollo/server](https://www.npmjs.com/package/@apollo/server)
  - 開発者 [Apollo Graph Inc.](https://www.apollographql.com/)
- [graphql-yoga](https://www.npmjs.com/package/graphql-yoga)
  - 開発者 [The Guild](https://the-guild.dev/)
  - GraphQL Code Generatorも作っている
- [@hono/graphql-server](https://www.npmjs.com/package/@hono/graphql-server)
  - Honoのミドルウェア

HTTPサーバー for GraphQLサーバー

- @apollo/serverについているstandaloneserver
- Hono

スキーマ定義

- [pothos](https://pothos-graphql.dev/)
