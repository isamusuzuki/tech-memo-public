# HTTPサーバー統合

作成日 2025/10/02

すでに[GraphQLサーバーとは](11_GraphQLサーバーとは.md)で理解した通り、GraphQLサーバーの仕事は、GraphQLクエリーというドキュメントを受け取って、処理して返すことで、HTTPサーバーとの統合は必須。HTTPサーバーのみならず、クラウドプラットフォームと連携するライブラリも含めて、サーバー選定をしていく必要がある

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
