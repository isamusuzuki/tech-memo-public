# ラムダ DSL

作成日 2025/02/07

## 公式サイト（英語）を読む

[Configuration Migrations](https://docs.spring.io/spring-security/reference/migration-7/configuration.html)

### Use the Lambda DSL

The Lambda DSL is present in Spring Security since version 5.2, and it allows HTTP security to be configured using lambdas.

The Lambda DSL is the preferred way to configure Spring Security, the prior configuration style will not be valid in Spring Security 7 where the usage of the Lambda DSL will be required.

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authorize -> authorize
                .requestMatchers("/blog/**").permitAll()
                .anyRequest().authenticated()
            )
            .formLogin(formLogin -> formLogin
                .loginPage("/login")
                .permitAll()
            )
            .rememberMe(Customizer.withDefaults());

        return http.build();
    }
}
```

### Lambda DSL Configuration Tips

- In the Lambda DSL there is no need to chain configuration options using the `.and()` method. The HttpSecurity instance is automatically returned for further configuration after the call to the lambda method.
- `Customizer.withDefaults()` enables a security feature using the defaults provided by Spring Security. This is a shortcut for the lambda expression `it → {}`.

### Goals of the Lambda DSL

- Automatic indentation makes the configuration more readable.
- There is no need to chain configuration options using `.and()`
- The Spring Security DSL has a similar configuration style to other Spring DSLs such as Spring Integration and Spring Cloud Gateway.

## Spring Security 7.0 はいつリリースされる

[Preparing for 7.0](https://docs.spring.io/spring-security/reference/migration-7/index.html)

> While Spring Security 7.0 does not have a release date yet, it is important to start preparing for it now.
> This preparation guide is designed to summarize the biggest changes in Spring Security 7.0 and provide steps to prepare for them.
> It is important to keep your application up to date with the latest Spring Security 6 and Spring Boot 3 releases.
