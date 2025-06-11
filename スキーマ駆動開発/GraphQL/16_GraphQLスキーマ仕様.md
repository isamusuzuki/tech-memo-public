# GraphQLスキーマ仕様

作成日 2025/06/11

- `type`キーワード ... オブジェクト型 GraphQLクエリによって取得可能なデータの構造
- `Query`型 ... クエリ要求のエントリーポイント
- `Mutation`型 ... 変更要求のエントリーポイント

公式ドキュメント => [Overview](https://www.graphql-js.org/docs/)

[Basic Types](https://www.graphql-js.org/docs/basic-types/)

このページのサンプルコードがしていること

- `buildSchema()`メソッドを使ってスキーマを構築する
- [String, Int, Float, Boolean, ID] といったスカラーはすぐに使える
