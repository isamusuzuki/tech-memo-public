# client-presetプラグイン

作成日 2025/10/08

## 1. 公式サイト（英語）を読む

[Plugins > Presets > client-preset](https://the-guild.dev/graphql/codegen/plugins/presets/preset-client)

`client-preset`は、お好みのGraphQLクライアントに対して、型定義がついたGraphQLオペレーションを提供する

プリセットとは、プラグインと設定の集合体であると理解した。なにか新しい機能を追加するものではない

## 2. codegen.tsファイル生成時にエラーを出す

```text
✖ [client-preset] target output should be a directory, ex: "src/gql/". Make sure you add "/" at the end of the directory path
```

バージョンによって動きが違うのではなく、このプリセットは、出力先はディレクトリであるべきだという信念に基づき、この設定をイキにしているのだと理解した

## 3. どんなプラグインが含まれているのか？

node_modules/@graphql-codegen/client-preset/package.json

```json
{
    "dependencies":{
        "@graphql-codegen/add": "^6.0.0",
        "@graphql-codegen/typescript": "^5.0.2",
        "@graphql-codegen/typescript-operations": "^5.0.2",
        "@graphql-typed-document-node/core": "3.2.0",
    }
}
```

`@graphql-codegen/typescript-graphql-request`以外は含まれていることがわかった。おそらく含まれていないものだけを追加でインストールすればいいはず
