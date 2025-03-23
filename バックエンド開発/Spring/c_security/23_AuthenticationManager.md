# AuthenticationManager

作成日 2025/02/13

`config/SecurityConfig.java`

```java
package com.tier3Hub.user_auth_service.config;

import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;

@Configuration
@EnableWebSecurity
public class SecurityConfig {
    @Autowired
    private UserInfoConfigManager userInfoConfigManager;

    @Autowired
    public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userInfoConfigManager).passwordEncoder(passwordEncoder());
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration auth) throws Exception {
        return auth.getAuthenticationManager();
    }
}
```

AuthenticationManager は必ず Bean 定義しておく必要があるらしい

## 公式ガイド（英語）を読む

ここに書いてあるコードが正解なんだと思う

[Username/Password Authentication](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/index.html)

【自分訳】`AuthenticationManager`ビーンを自分でパブリッシュして認証機能をカスタムしたいと思う場面は、ログインフォームの代わりに REST API 経由でユーザー認証をしたいというときである

```java
@Bean
public AuthenticationManager authenticationManager(
        UserDetailsService userDetailsService,
        PasswordEncoder passwordEncoder) {
    DaoAuthenticationProvider authenticationProvider = new DaoAuthenticationProvider();
    authenticationProvider.setUserDetailsService(userDetailsService);
    authenticationProvider.setPasswordEncoder(passwordEncoder);

    return new ProviderManager(authenticationProvider);
}
```

Controller で AuthenticationManager を利用する場合は、コンストラクターの引数に AuthenticationManager を入れて、プライベートの変数に当てはめる

## javadoc を読む

[AuthenticationManager (spring-security-docs 6.4.2 API)](https://docs.spring.io/spring-security/reference/api/java/org/springframework/security/authentication/AuthenticationManager.html)

メソッドは、authenticate しかない。引数は Authentication
