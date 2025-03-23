# Spring Security

作成日 2025/01/20

参照サイト => [【Spring Security】ユーザー認証のイメージを掴む編](https://zenn.dev/peishim/articles/6946f72e15affa)

```java
// SecurityConfig.java

@Configuration
@EnableWebSecurity
public class SecurityConfig{
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public InMemoryUserDetailsManager userDetailsService() {
        UserDetails user = User
                .withUsername("user")
                .password(passwordEncoder().encode("password"))
                .roles("USER")
                .build();
        return new InMemoryUserDetailsManager(user);
    }

}
```

DB から取得したユーザーの情報で認証を行うには、JdbcUserDetailsManager を使う
