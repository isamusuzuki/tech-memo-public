# codegen.tsを読み解く

作成日 2025/10/02

## 1. 公式ドキュメント（英語）を読む

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
