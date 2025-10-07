# typescript-operations プラグイン

作成日 2025/10/07

## 公式サイト（英語）を読む

[Plugins > TypeScript > operations](https://the-guild.dev/graphql/codegen/plugins/typescript/typescript-operations)

### インストール

```bash
npm i -D @graphql-codegen/typescript-operations
```

### 使用上の注意

このプラグインを使うためには、GraphQLオペレーション (query, mutation, subscription, fragment) が、codegen.ymlファイルのdocuments項目に用意されている必要がある

GraphQLオペレーション (query, mutation, subscription, fragment) をロードしていない場合は、生成される結果になにも変化がないことになる
