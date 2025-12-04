# Securing Microservices with Spring Security: Implementing JWT 写経

作成日 2025/02/04、更新日 2025/02/13

## 1. 参照サイト

[Securing Microservices with Spring Security: Implementing JWT](https://dev.to/ayshriv/securing-microservices-with-spring-security-implementing-jwt-38m6)

## 2. ソースコード

[User Auth Service](https://github.com/ayushstwt/user-auth-service)

Spring Boot のバージョンは 3.3.3

## 3. application.properties 編集

```bash
spring.datasource.driver-class-name=org.postgresql.Driver
spring.datasource.url=jdbc:postgresql://localhost:5432/testdb
spring.datasource.username=dbuser
spring.datasource.password=dbpasswd
```

あらかじめテーブルを生成しておく必要はない。Spring Data JPA が自動生成してくれる

## 4. entity/User.java 編集

```java
// @Entity(name = "User")
@Entity(name = "uasUser")
public class User {
}
```

PostgreSQL では、`user`が予約語で、テーブル名に使えないため

エビデンス => [Cannot create a database table named 'user' in PostgreSQL](https://stackoverflow.com/questions/22256124/cannot-create-a-database-table-named-user-in-postgresql)

## 6. launch.json 生成

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Launch Chrome",
      "request": "launch",
      "type": "chrome",
      "url": "http://localhost:8000/swagger-ui/index.html",
      "webRoot": "${workspaceFolder}"
    },
    {
      "type": "java",
      "name": "UserAuthServiceApplication",
      "request": "launch",
      "mainClass": "com.tier3Hub.user_auth_service.UserAuthServiceApplication",
      "projectName": "user-auth-service"
    }
  ],
  "compounds": [
    {
      "name": "Debug App",
      "configurations": ["UserAuthServiceApplication", "Launch Chrome"],
      "stopAll": true
    }
  ]
}
```

## 7. デバッグ開始

auth-controller > /api/auth/register > Try it out

```json
{
  "username": "taroyamada",
  "password": "password",
  "email": "taro@example.com"
}
```

auth-controller > /api/auth/login > Try it out

```json
{
  "data": {
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ0YXJveWFtYWRhIiwiaWF0IjoxNzM5NDEzNTk0LCJleHAiOjE3Mzk0MTY1OTR9.l-mudFojUIm_WHcca0DQg6YjJhVAYfIfhMm8T1Y5is0",
    "tokenType": "Bearer"
  },
  "message": "User logged in successfully",
  "status": 200
}
```

ヘッダー付きの GET 送信は、REST Client でないとできない

```bash
POST http://localhost:8000/api/auth/login HTTP/1.1
content-type: application/json

{
    "username": "taroyamada",
    "password": "password"
}

###

GET http://localhost:8000/api/test/public/hello
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ0YXJveWFtYWRhIiwiaWF0IjoxNzM5NDI4NzQ2LCJleHAiOjE3Mzk0MzE3NDZ9.6YB7jnk7miOtsPuuV6l1hBXs1KMUzc5dokBCqoaedlQ

###

GET http://localhost:8000/api/test/private/hello
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ0YXJveWFtYWRhIiwiaWF0IjoxNzM5NDI4NzQ2LCJleHAiOjE3Mzk0MzE3NDZ9.6YB7jnk7miOtsPuuV6l1hBXs1KMUzc5dokBCqoaedlQ
```
