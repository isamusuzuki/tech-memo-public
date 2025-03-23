# graphql-codegen

作成日 2025/01/07

参照サイト => [graphql-codegen を使って GraphQL スキーマからフロントエンドのコードを自動生成してみた](https://zenn.dev/sky/articles/47b86d3387389d)

> フロントエンド(NuxtJS)と BFF(GraphQL サーバ)の間に差が出ないように、スキーマファイルからコードを自動生成し開発しています。
>
> ソースコードを自動生成しているライブラリが便利だったので、使い方を紹介していきます。使用しているライブラリは graphql-code-generator です。

## 必要なライブラリとインストール

```bash
# @graphql-codegen/cli
# @graphql-codegen/import-types-preset
# @graphql-codegen/typescript-graphql-request
# @graphql-codegen/typescript-operations
# @graphql-codegen/near-operation-file-preset
# @graphql-codegen/add
# graphql-request

$npm install -D @graphql-codegen/typescript @graphql-codegen/typescript-graphql-request @graphql-codegen/typescript-operations

$npm install graphql-request
```

必要なライブラリを用意した後は設定ファイルとスキーマファイルが必要になります

## 設定ファイル

`npx graphql-codegen`を実行すると TypeScript ファイルが自動で生成されます。

フィールド説明

- `schema` ... 参照するスキーマファイルのパス
- `documents` ... クライアントから発行するクエリファイル
- `generates` ... 生成に関するオプション。キーが生成ファイル名。バリューがそのファイルに対するオプション

## スキーマファイル

サンプルコード => [https://raw.githubusercontent.com/marmelab/GraphQL-example/master/schema.graphql](https://raw.githubusercontent.com/marmelab/GraphQL-example/master/schema.graphql)

## 生成ファイル

2 つのスキーマファイルを用意した上で`npx graphql-codegen`を実行すると`client/types.ts`が生成されます。

自動生成されたファイルから`getSdk`が提供されるので、`graphql-request`のクライアントクラスを引数に渡すと戻り値で TweetByIdDocument が実行できるようになります。

```javascript
import { GraphQLClient } from 'graphql-request';
import { getSdk } from './types.ts';

const BASE_GRAPHQL_ENDPOINT = 'http://127.0.0.1:xxxx/graphql';

const graphQLClient = new GraphQLClient(BASE_GRAPHQL_ENDPOINT);
const sampleClient = getSdk(graphQLClient);
```
