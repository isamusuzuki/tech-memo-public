# UserDetailsService インターフェイス

作成日 2025/02/13

参照サイト => [UserDetails と UserDetailsService：ユーザー情報を扱うコンポーネントを理解しよう](https://poco-tech.com/posts/spring-security-introduction/userdetails-and-userdetailsservice/)

UserDetails は、ユーザー情報を保持するインターフェース

```java
package org.springframework.security.core.userdetails;

public interface UserDetails extends Serializable {
}
```

User は、UserDertails の実装クラス

```java
package org.springframework.security.core.userdetails;

public class User implements UserDetails, CredentialsContainer {
}
```

UserDetailsService は、UserDetails オブジェクトを取得するためのインターフェース

```java
package org.springframework.security.core.userdetails;

public interface UserDetailsService {
    UserDetails loadUserByUsername(String username) throws UsernameNotFoundException;
}
```

アプリケーションで実際の処理を実現するためには、このインターフェースの実装クラスを作成する必要がある

`service/UserInfoConfigManager.java`

```java
package com.tier3Hub.user_auth_service.service;

import com.tier3Hub.user_auth_service.Repository.AuthRepository;
import com.tier3Hub.user_auth_service.entity.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

@Service
public class UserInfoConfigManager implements UserDetailsService {
    @Autowired
    private AuthRepository authRepository;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        User user=authRepository.findByUsername(username);
        if (user != null) {
            return org.springframework.security.core.userdetails.User.builder()
                    .username(user.getUsername())
                    .password(user.getPassword())
                    .roles(user.getRoles().toArray(new String[0]))
                    .build();
        }
        throw new UsernameNotFoundException("User not found with username: " + username);
    }
}
```

UserDetailsService クラスの実装クラスを作成したら、Spring Security に統合することで、アプリケーション固有のユーザー認証ロジックを実現できる

`config/SecurityConfig.java`

```java
package com.tier3Hub.user_auth_service.config;

import com.tier3Hub.user_auth_service.security.JWTFilter;
import com.tier3Hub.user_auth_service.service.UserInfoConfigManager;
import com.tier3Hub.user_auth_service.utils.AppConstants;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configurers.AbstractHttpConfigurer;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

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
}
```
