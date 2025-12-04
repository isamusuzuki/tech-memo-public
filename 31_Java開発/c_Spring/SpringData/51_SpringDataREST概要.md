# Spring Data REST 概要

作成日 2025/02/13

## 公式サイト（英語）

[Spring Data REST](https://spring.io/projects/spring-data-rest)

## 参照サイト

[Spring Data REST の要点と利用方法](https://qiita.com/umiushi_1/items/b369f659bbd94576b8f4)

> Spring Data REST は、Spring Data で作成したリポジトリを RESTful なエンドポイントとして自動的に公開します。\
> Spring Data REST の機能を利用することによって、Controller や Service クラスの実装を省略する事ができるということです。

Entity クラス

```java
@Entity
public class User {
  @Id
  private Long id;
  private String name;
  private Integer age;
  // getter, setter
}
```

Repository クラス

```java
public interface UserRepository extends JpaRepository<User,Long> {}
```

> Spring Data REST はこのように定義された Repository 群を RESTful な API エンドポイントとして\
> 公開してくれるライブラリだということです(Controller や Service クラスの実装が必要なくなるということです)。

### Spring Data REST の機能紹介

1. CRUD 操作の自動生成
1. ページングとソート
1. クエリメソッドのサポート
