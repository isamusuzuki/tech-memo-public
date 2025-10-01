# Spring Boot Actuator

作成日 2025/02/13、更新日 2025/08/26

## 1. 参照サイトを読む

[Spring Boot Actuator とは？](https://zenn.dev/choimake/articles/4ea99fd55a3b35)

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

```text
http://localhost:8080/actuator/health
```

## 2. 公式リファレンスを読む

[本番対応機能を有効にする](https://spring.pleiades.io/spring-boot/reference/actuator/enabling.html)

[エンドポイント](https://spring.pleiades.io/spring-boot/reference/actuator/endpoints.html)

- bean ... アプリケーション内のすべての`Spring Bean`の完全なリストを表示します。
- env ... `ConfigurableEnvironment`サブジェクトのプロパティをサニタイズに公開します。
- health ... アプリケーションの正常性情報を表示します。
- mappings ... すべての`@RequestMapping`パスの照合リストを表示します。

※ GraphQLの`@QueryMapping`パスの照合リストを表示するエンドポイントはない

## 3. mappintsを試す

application.properties

```text
management.endpoints.web.exposure.include=*
```

```text
http://localhost:8080/actuator/mappings
```
