# graphql-codegenとは

作成日 2025/09/30

## 1. 公式サイト（英語）を読む

[GraphQL Codegen](https://the-guild.dev/graphql/codegen)

> Effortlessly generate comprehensive code from GraphQL schemas and operations, streamlining development across your tech stack.]

[Introduction to GraphQL Code Generator](https://the-guild.dev/graphql/codegen/docs/getting-started)

> GraphQL Code Generator is a plugin-based tool that helps you get the best out of your GraphQL stack.

## 2. 解説記事aを読む

[graphql-codegen を使って GraphQL スキーマからフロントエンドのコードを自動生成してみた](https://zenn.dev/sky/articles/47b86d3387389d)

> フロントエンド(NuxtJS)とBFF(GraphQLサーバ)の間に差が出ないように、スキーマファイルからコードを自動生成し開発しています。
>
> ソースコードを自動生成しているライブラリが便利だったので、使い方を紹介していきます。使用しているライブラリはgraphql-code-generatorです。

### 必要なライブラリとインストール

```bash
npm install -D @graphql-codegen/typescript @graphql-codegen/typescript-graphql-request @graphql-codegen/typescript-operations

npm install graphql-request
```

必要なライブラリを用意した後は設定ファイルとスキーマファイルが必要になります

### 設定ファイル

`npx graphql-codegen`を実行するとTypeScriptファイルが自動で生成されます。

フィールド説明

- `schema` ... 参照するスキーマファイルのパス
- `documents` ... クライアントから発行するクエリファイル
- `generates` ... 生成に関するオプション。キーが生成ファイル名。バリューがそのファイルに対するオプション

### スキーマファイル

サンプルコード => [https://raw.githubusercontent.com/marmelab/GraphQL-example/master/schema.graphql](https://raw.githubusercontent.com/marmelab/GraphQL-example/master/schema.graphql)

### 生成ファイル

2 つのスキーマファイルを用意した上で`npx graphql-codegen`を実行すると`client/types.ts`が生成されます。

自動生成されたファイルから`getSdk`が提供されるので、`graphql-request`のクライアントクラスを引数に渡すと戻り値で TweetByIdDocument が実行できるようになります。

```javascript
import { GraphQLClient } from 'graphql-request';
import { getSdk } from './types.ts';

const BASE_GRAPHQL_ENDPOINT = 'http://127.0.0.1:xxxx/graphql';

const graphQLClient = new GraphQLClient(BASE_GRAPHQL_ENDPOINT);
const sampleClient = getSdk(graphQLClient);
```

## 3. 解説記事bを読む

[GraphQL のスキーマ定義やクエリから型定義、自動生成できまっせ](https://qiita.com/yoshii0110/items/b461e608dc0cff78982e)

> GraphQL を使用する際に GraphQL のスキーマやクエリから TypeScript の型定義を自動生成する方法について記事にしていきます。
>
> - GraphQL Code Generator というのは、GraphQL のスキーマ定義を使用して型定義を自動生成するためのツールです。
> - このスキーマ定義の型定義を作成することで、クライアントサイドからの GraphQL のリクエストとレスポンスに型をつけることができます。
> - さまざまなフレームワーク、ライブラリに対応しているので、用途にあったプラグインを組み合わせて使用します。
> - また、フロントエンドで GraphQL を使用する場合とバックエンドで GraphQL を使用する場合、それぞれプラグインや実装方法が異なるので、注意が必要です。
