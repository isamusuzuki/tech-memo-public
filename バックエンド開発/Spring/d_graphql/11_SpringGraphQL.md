# Spring for GraphQL

作成日 2025/01/09、更新日 2025/01/16

## 公式マニュアル（日本語）

[概要](https://spring.pleiades.io/spring-graphql/reference/)

## [アノテーション付きコントローラー](https://spring.pleiades.io/spring-graphql/reference/controllers.html)

参照サイト => [SpingBoot で作る GraphQL アプリケーション](https://qiita.com/rhirabay/items/6c7a6308793ed97b6045)

- RestAPI との違いは、`@GetMapping`等の代わりに`@QueryMapping`を使用するところ
- メソッド名は（スキーマ定義の）`type Query`で定義した名前と対応させる
- 更新系は、`@QueryMapping`の代わりに`@MutationMapping`を使用する
- パラメータは`@Argument`を付与した引数で受け取る
