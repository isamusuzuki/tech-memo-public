# Spring for GraphQL

作成日 2025/01/09、更新日 2025/06/12

## 1. [アノテーション付きコントローラー](https://spring.pleiades.io/spring-graphql/reference/controllers.html)

参照サイト => [SpingBoot で作る GraphQL アプリケーション](https://qiita.com/rhirabay/items/6c7a6308793ed97b6045)

- RestAPIとの違いは、`@GetMapping`等の代わりに`@QueryMapping`を使用するところ
- メソッド名は（スキーマ定義の）`type Query`で定義した名前と対応させる
- 更新系は、`@MutationMapping`を使用する
- パラメータは`@Argument`を付与した引数で受け取る

## 2. Spring Boot内のマニュアル（英語）を読む

[Spring for GraphQL](https://docs.spring.io/spring-boot/reference/web/spring-graphql.html)

### 2a. GraphQL Schema

Spring GraphQLアプリケーションは、起動時に定義されたスキーマを必要とする。デフォルトでは、`src/main/resources/graphql/**` フォルダ下に `.graphqls`拡張子のスキーマファイルを書くと、Spring Bootが自動的に、これらをピックアップする

### 2b. HTTP and WebSocket

デフォルトでは、GraphQL HTTPのエンドポイントは`/grapql`である（HTTPメソッドはPOST）。このパスは、`spring.graphql.http.path`でカスタマイズ可能である
