# `@Value`アノテーションのデフォルト値

作成日 2025/02/12

## 参照サイトを読む

[Spring の@Value で String 型のデフォルト値を null にする方法](https://qiita.com/kazuki43zoo/items/e60eda11980e04ac61b3)

Spring Framework では、プロパティ値(プロパティファイルやシステムプロパティの値)を取得する場合に\
`@org.springframework.beans.factory.annotation.Value`アノテーションを使うことができます。

`@Value`はキーがない時のデフォルト値を指定することができます。\
デフォルト値を指定する場合は、`@Value("${キー名:デフォルト値}")`という形式で指定します。

## ユーザーガイドを読む

[@Value を使用する](https://spring.pleiades.io/spring-framework/reference/core/beans/annotation-config/value-annotations.html)

```java
// @Value は通常、外部化されたプロパティを注入するために使用されます。
public MovieRecommender(@Value("${catalog.name}") String catalog) {
    this.catalog = catalog;
}

// デフォルト値を提供することが可能です。
public MovieRecommender(@Value("${catalog.name:defaultCatalog}") String catalog) {
    this.catalog = catalog;
}

// @Value に SpEL 式が含まれる場合、値は実行時に動的に計算されます。
public MovieRecommender(@Value("#{systemProperties['user.catalog'] + 'Catalog' }") String catalog) {
    this.catalog = catalog;
}

// SpEL は、より複雑なデータ構造の使用も可能にします。
public MovieRecommender(
    @Value("#{{'Thriller': 100, 'Comedy': 300}}") Map<String, Integer> countOfMoviesPerCatalog) {
this.countOfMoviesPerCatalog = countOfMoviesPerCatalog;
}
```
