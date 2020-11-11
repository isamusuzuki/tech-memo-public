# Apollo を学ぶ

作成日 2019/11/28

## 01. Apollo とは

公式トップ => [https://www.apollographql.com/](https://www.apollographql.com/)

イントロダクション => [0\. Introduction \- Apollo Basics \- Apollo GraphQL Docs](https://www.apollographql.com/docs/tutorial/introduction/)

## 02. Apollo の紹介記事を読む

[\[GraphQL\] TypeScript\+VSCode\+Apollo で最高の DX を手に入れよう ｜ Developers\.IO](https://dev.classmethod.jp/client-side/apollo-good-dx/)

[bisque33/graphql\-client\-dx](https://github.com/bisque33/graphql-client-dx)

Apollo GraphQL という vscode 拡張を使う

- VSCode に拡張機能の Apollo GraphQL を追加します。
- apollo.config.js に GraphQL サーバの接続情報を設定します。
- VSCode のコマンドパレットで Apollo: Reload schema を実行します。
- 以上の設定を終えると、 gql タグで囲った内部で、入力補完が働くようになります。

apollo-tooling という CLI を使う

- apollo-tooling をインストールします。インストールせずに npx を使っても良いです。
- client:codegen コマンドを実行します。 apollo client:codegen --target typescript types
- 以上の手順を終えると、 gql タグで囲った内部に定義した Query や Mutation のレスポンスの型が自動生成されます。
- また Enum や Input Object の型定義も自動生成されます。

Apollo のエコシステムを利用することで、
GraphQL を利用するクライアントアプリケーションの実装が
省力化できることがおわかりいただけたかと思います。

今回紹介したのと同じような内容が GraphQL Summit 2019 の
セッションにもありますので、ぜひこちらも御覧になってください。

[The GraphQL developer experience \- YouTube](https://www.youtube.com/watch?v=q-eJvp9CMY8)
