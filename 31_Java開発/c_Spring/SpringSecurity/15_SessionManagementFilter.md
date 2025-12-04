# SessionManagementFilter

作成日 2025/03/04

## リファレンスを読む

[認証の永続性とセッション管理](https://spring.pleiades.io/spring-security/reference/servlet/authentication/session-management.html)

リクエスト間で認証を永続化するために、HttpSession を作成して維持する必要がない場合があります。HTTP ベーシックなどの一部の認証メカニズムはステートレスであるため、リクエストごとにユーザーを再認証します。

セッションを作成したくない場合は、次のように SessionCreationPolicy.STATELESS を使用できます。

```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) {
    http
        // ...
        .sessionManagement((session) -> session
            .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
        );
    return http.build();
}
```
