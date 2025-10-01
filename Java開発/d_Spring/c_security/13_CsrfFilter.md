# CsrfFilter

作成日 2025/02/07、更新日 2025/03/04

## 1. CSRF とは

参照書籍 => P.185 「14.13 CSRF の対応」 from 『プロになるための Spring 入門』

CSRF = Cross Site Request Forgery

一般的な対処方法 => CSRF トークン（Web 側で生成するランダムな値）を使う

リクエストに含められた CSRF トークンとセッションスコープの中の CSRF トークンを比較し、合致していなければエラーにする

## 2. ソースコードを読む

パッケージ => `org.springframework.security.config.annotation.web.builders`

ファイル => `HttpSecurity.class` 1740 行目

```java
/**
 * Enables CSRF protection. This is activated by default when using
 * {@link EnableWebSecurity}. You can disable it using:
 */
@Configuration
@EnableWebSecurity
public class CsrfSecurityConfig {
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf((csrf) -> csrf.disable());
        return http.build();
    }
}
```
