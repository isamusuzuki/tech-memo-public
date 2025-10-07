# codegen.tsを読み解く

作成日 2025/10/02

## 公式ドキュメント（英語）を読む

[Config Reference > codegen.ts file](https://the-guild.dev/graphql/codegen/docs/config-reference/codegen-config)

codegen.tsの例

```javascript
import { CodegenConfig } from '@graphql-codegen/cli'

const config: CodegenConfig = {
  schema: 'http://localhost:4000/graphql',
  documents: ['src/**/*.tsx'],
  generates: {
    './src/gql/': {
      preset: 'client'
    }
  }
}

export default config
```

- `schema` (required) ... GraphQLエンドポイントのURL、または`.graphql`ファイルのローカルパス
- `documents` ... `gql`タグ付きか、もしくはプレーンの文字列のGraphQLクエリードキュメント
- `generates` (required) ... 生成したコードを出力する先

### [schema field](https://the-guild.dev/graphql/codegen/docs/config-reference/schema-field)

`schema`フィールドは、`GraphQLSchema`を指し示す必要がある。`GraphQLSchema`を定義しロードさせる方法はいくつかある

### [documents field](https://the-guild.dev/graphql/codegen/docs/config-reference/documents-field)

`documents`フィールドは、`query`,`mutation`,`subscription`,`fragment`といった、GraphQLドキュメントを指し示す必要がある。`documents`フィールドは、クライアントサイド向けにコードを生成するプラグインを使う場合のみ、必要とされる
