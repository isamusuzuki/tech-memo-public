# EnableMethodSecurity, EnableGlobalMethodSecurity アノテーション

作成日 2025/02/14

```java
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;

@SpringBootApplication
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(SecurityServiceApplication.class, args);
    }
}
```

## 参考記事を読む

[Spring の@EnableGlobalMethodSecurity を完全ガイド！初心者でもわかるメソッドセキュリティ設定](https://star-school.jp/spring/180)

> @EnableGlobalMethodSecurity アノテーションは、Spring Security のメソッドレベルのセキュリティを有効にするためのアノテーションです。
> これを利用することで、@Secured や@PreAuthorize といったアノテーションを使い、特定のメソッドに対するアクセス制御を設定できます。
> 例えば、管理者ユーザーのみが実行できるメソッドや、特定のロールを持つユーザーのみがアクセスできる機能を実装することが可能です。

## EnableGlobalMethodSecurity のオプション

```java
@EnableGlobalMethodSecurity(securedEnabled = true)

@Secured("ROLE_ADMIN")
public void performAdminTask() {
    System.out.println("管理者専用のタスクを実行中...");
}
```

```java
@EnableGlobalMethodSecurity(prePostEnabled = true)

@PreAuthorize("hasRole('ADMIN') or #username == authentication.name")
public void viewProfile(String username) {
    System.out.println(username + "のプロフィールを表示中...");
}
```

## javadoc を読む

[アノテーションインターフェース EnableGlobalMethodSecurity](https://spring.pleiades.io/spring-security/site/docs/current/api/org/springframework/security/config/annotation/method/configuration/EnableGlobalMethodSecurity.html)

> 使用すべきではありません。代わりに EnableMethodSecurity を使用してください

[アノテーションインターフェース EnableMethodSecurity](https://spring.pleiades.io/spring-security/site/docs/current/api/org/springframework/security/config/annotation/method/configuration/EnableMethodSecurity.html)

secureEnabled, prePostEnabld は健在
