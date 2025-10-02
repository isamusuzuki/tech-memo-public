# integration-cloudflare-workersを試す

作成日 2025/10/02

すでに[GraphQLサーバーとは](11_GraphQLサーバーとは.md)で理解した通り、GraphQLサーバーの仕事は、GraphQLクエリーというドキュメントを受け取って、処理して返すことで、HTTPサーバーとの統合は必須。HTTPサーバーのみならず、クラウドプラットフォームと連携するライブラリも含めて、サーバーを選定していく必要がある

## 1. @as-integrationsの正体

as-integrationsはApollo Server Integrationの略称だった

[apollo-server-integrations](https://github.com/apollo-server-integrations)

> Hello! Welcome to the Apollo Server integrations community GitHub organization. This is a shared space where community maintainers can choose to keep their integration repositories. We also have a shared NPM scope @as-integrations where we publish our packages.

Cloudflare Workers用のパッケージがあった

[apollo-server-integration-cloudflare-workers](https://github.com/apollo-server-integrations/apollo-server-integration-cloudflare-workers)

Cloudflare Workers用のパッケージを取り込んだテンプレートもあった

[kimyvgy/worker-apollo-server-template: Deploy Apollo Server v4 to Cloudflare Workers](https://github.com/kimyvgy/worker-apollo-server-template)

## 2. Cloudflareが提供しているテンプレート

[cloudflare/workers-graphql-server: 🔥Lightning-fast, globally distributed Apollo GraphQL server, deployed at the edge using Cloudflare Workers](https://github.com/cloudflare/workers-graphql-server)

テンプレートをそのまんま動かしてみる（パッケージをひとつづつインストールして、再現することは失敗した）

### セットアップ

GitHubリポジトリのページにある緑色のCodeボタンをクリック ＞ Download ZIPをクリック

ファイルを展開して、workers-graphql-server-templateフォルダをcacaoに名前変更して、フォルダを移動

```bash
cd cacao
npm ci
```

### GraphQLCodegenを走らせる

```bash
npm run codegen
# => src/generated/graphql.ts 8.28KB
# => graphql.schema.json 45KB

npm run dev #=> Open brwoser to http://127.0.0.1:8787
```

Apollo Sandboxが登場

Operation 枠の左にDocumentation枠があり、そこを使うとラクにクエリーを作成できる

### ちゃんと動いたGraphQLクエリーの例

```javascript
query GetMovie {
  movies {
    id
    rating
    releaseDate
    title
  }
}

mutation add {
  add (
    numberOne: 11,
    numberTwo: 23
    )
}

query GetPokemon {
  pokemon(id: 3) {
    id
    name
    weight
    height
    sprites {
      back_default
      front_default
    }
  }
}
```

### このテンプレートを再現しようとしてみてわかったこと

- `npm i @apollo/server`を実行すると、リリースされたばかりのv5がインストールされる。Cloudflare Workers統合のパッケージは、まだこのバージョンに対応していない
- Apollo Server v4の最終バージョンは`4.12.2`である。v4系は2026年1月に引退する
