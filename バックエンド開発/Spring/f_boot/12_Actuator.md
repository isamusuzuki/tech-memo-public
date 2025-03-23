# Spring Boot Actuator

作成日 2025/02/13

## 参照サイト

[Spring Boot Actuator とは？](https://zenn.dev/choimake/articles/4ea99fd55a3b35)

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

```bash
curl localhost:8080/actuator/health
{"status":"UP"}%
```

## 公式リファレンス（英語）

[Enabling Production-ready Features](https://docs.spring.io/spring-boot/reference/actuator/enabling.html)

[Endpoints](https://docs.spring.io/spring-boot/reference/actuator/endpoints.html)

- bean
- env
- flyway
- health
- metrics
- mappings
