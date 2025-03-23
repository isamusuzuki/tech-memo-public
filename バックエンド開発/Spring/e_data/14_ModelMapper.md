# ModelMapper

作成日 2025/02/13

Maven Repository => [ModelMapper](https://mvnrepository.com/artifact/org.modelmapper/modelmapper)

公式サイト（英語） => [ModelMapper - Simple, Intelligent, Object Mapping.](https://modelmapper.org/)

## [Gettig Started](https://modelmapper.org/getting-started/)を読む

セットアップ

```xml
<dependency>
  <groupId>org.modelmapper</groupId>
  <artifactId>modelmapper</artifactId>
  <version>3.2.2</version>
</dependency>
```

## 使用例

config/ApplicationConfigs.java

```java
package com.tier3Hub.user_auth_service.config;

import org.modelmapper.ModelMapper;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class ApplicationConfigs {
    @Bean
    public ModelMapper modelMapper()
    {
        return new ModelMapper();
    }
}
```

service/AuthServiceImpl.java

```java
package com.tier3Hub.user_auth_service.service;

import com.tier3Hub.user_auth_service.Repository.AuthRepository;
import com.tier3Hub.user_auth_service.dto.RegisterDTO;
import com.tier3Hub.user_auth_service.dto.RegisterResponse;
import com.tier3Hub.user_auth_service.entity.User;
import org.modelmapper.ModelMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.stereotype.Service;

import java.time.LocalDateTime;
import java.util.Arrays;
// import java.util.List;

@Service
public class AuthServiceImpl implements AuthService {
    @Autowired
    private AuthRepository authRepository;

    @Autowired
    private ModelMapper modelMapper;

    private static final PasswordEncoder passwordEncoder = new BCryptPasswordEncoder();

    @Override
    public RegisterResponse register(RegisterDTO registerDTO) {
        User user = modelMapper.map(registerDTO, User.class);
        user.setPassword(passwordEncoder.encode(registerDTO.getPassword()));
        user.setCreatedAt(LocalDateTime.now());
        user.setRoles(Arrays.asList("USER"));
        User save = authRepository.save(user);
        return modelMapper.map(save, RegisterResponse.class);
    }
}
```
