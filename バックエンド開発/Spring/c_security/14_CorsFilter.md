# CorsCustomizer

作成日 2025/02/07、更新日 2025/03/04

## 1. CORS とは

Cross-Origin Resource Sharing

HTTP ヘッダーの転送で構成されるシステム

ブラウザがオリジンをまたいだリクエストのレスポンスに、フロントエンドの JavaScript コードがアクセスすることをブロックするかどうかを決める

## 2. リファレンスドキュメントを読む

[Spring の CORS サポート](https://spring.pleiades.io/spring-security/reference/servlet/integrations/cors.html)

ユーザーは、CorsConfigurationSource を提供することで、CorsFilter を Spring Security と統合できる

Spring Security は、UrlBasedCorsConfigurationSource インスタンスが存在する場合にのみ CORS を自動的に構成する

```java
@Bean
UrlBasedCorsConfigurationSource corsConfigurationSource() {
    CorsConfiguration configuration = new CorsConfiguration();
    configuration.setAllowedOrigins(Arrays.asList("https://example.com"));
    configuration.setAllowedMethods(Arrays.asList("GET","POST"));
    UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
    source.registerCorsConfiguration("/**", configuration);
    return source;
}
```

Spring MVC の CORS サポートを使用する場合は、CorsConfigurationSource の指定を省略でき、Spring Security は Spring MVC に提供されている CORS 構成を使用する

```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            // if Spring MVC is on classpath and no CorsConfigurationSource is provided,
            // Spring Security will use CORS configuration provided to Spring MVC
            .cors(withDefaults())
            ...
        return http.build();
    }
}
```

SecurityFilterChain ごとに異なる CorsConfigurationSource を指定する場合は、それを .cors() DSL に直接渡すことができる

```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig {

    @Bean
    public SecurityFilterChain apiFilterChain(HttpSecurity http) throws Exception {
        http
            .securityMatcher("/api/**")
            .cors((cors) -> cors
                .configurationSource(apiConfigurationSource())
            )
            ...
        return http.build();
    }

    UrlBasedCorsConfigurationSource apiConfigurationSource() {
        CorsConfiguration configuration = new CorsConfiguration();
        configuration.setAllowedOrigins(Arrays.asList("https://api.example.com"));
        configuration.setAllowedMethods(Arrays.asList("GET","POST"));
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", configuration);
        return source;
    }
}
```

## 3. javadoc を読む

[クラス CorsConfiguration](https://spring.pleiades.io/spring-framework/docs/current/javadoc-api/org/springframework/web/cors/CorsConfiguration.html)

- setAllowedOriginPatterns ... ホスト名の任意の場所に"\*"がついた、より柔軟なオリジンパターンをサポートする
- setAllowedOrigins ... 許可されるオリジンのリスト
- setAllowedMethods ... 許可する HTTP メソッド (例: "GET"、"POST"、"PUT" など) を設定
- setAllowCredentials ... ユーザー資格情報がサポートされているかどうか
- setAllowedHeaders ... プリフライトリクエストが実際のリクエスト中に使用できるようにリストできるヘッダーのリストを設定
