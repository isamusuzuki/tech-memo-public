# JDBC 接続サンプルコード

作成日 2025/02/12、更新日 2025/02/20

JDBC = RDB にアクセスするための標準 JAVA API

## 1. PostgreSQL データベースを用意する

`PostgreSQL/12_テストDB構築.md`を参照

## 2. サンプルアプリを立ち上げる

[spring initializr](https://start.spring.io/)で追加する Dependencies

- Lombok
- Spring Web
- JDBC API
- PostgreSQL Driver

```text
--avocado/
    |--src/main/java/com/example/avocado/
    |   |--MyFriendsDto.java
    |   |--MyFriendsRepository.java
    |   `--MyFriendsController.java
    `--src/main/resources/
        `--application.properties
```

application.properties

```bash
spring.datasource.driverClassName=org.postgresql.Driver
spring.datasource.url=jdbc:postgresql://localhost:5432/testdb
spring.datasource.schema=public
spring.datasource.username=dbuser
spring.datasource.password=dbpasswd
```

- Spring Boot がデータソースを用意し、JdbcTemplate をインスタンス化する
- リポジトリのコンストラクターに引数として JdbcTemplate を与えるところから書く
- `spring.datasource.driver-class-name`と`spring.datasource.driverClassName`は同じ模様

## 3. Java コードを書く

MyFriendsDTO.java

```java
package com.example.avocado;

import java.util.Date;
import lombok.AllArgsConstructor;
import lombok.Data;

@Data
@AllArgsConstructor
public class MyFriendsDto {
    private int id;
    private String name;
    private String address;
    private Date created_on;
}
```

MyFriendsRepository.java

```java
package com.example.avocado;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;
import java.util.Map;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

@Repository
public class MyFriendsRepository {
    private final JdbcTemplate jdbcTemplate;

    public MyFriendsRepository(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    public List<MyFriendsDto> getAll() {
        String sql = "select id, name, address, created_on from myfriends";
        List<Map<String, Object>> myfriendsList = jdbcTemplate.queryForList(sql);
        List<MyFriendsDto> dtoList = new ArrayList<>();
        for (Map<String, Object> myfriend: myfriendsList) {
            dtoList.add(new MyFriendsDto(
                (int) myfriend.get("id"),
                (String) myfriend.get("name"),
                (String) myfriend.get("address"),
                (Date) myfriend.get("created_on")));
        }
        return dtoList;
    }
}
```

MyFriendsController.java

```java
package com.example.avocado;

import java.util.List;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.GetMapping;

@RestController
public class MyFriendsController {
    @Autowired
    private MyFriendsRepository myFriendsRepository;

    @GetMapping("/myfriends")
    public ResponseEntity<List<MyFriendsDTO>> getAll() {
        List<MyFriendsDto> list = myFriendsRepository.getAll();
        return ResponseEntity.ok(list);
    }
}
```

## queryForObject のサンプルコード

```java
@Repository
public class SessionRepository {
    private final JdbcTemplate jdbcTemplate;

    public SessionRepository(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    public Long getId() {
        String sql = "select user_id from d_session where session_uuid = '2xHGWkMlTXexgZ3uqZAzxA'";
        Long user_id = jdbcTemplate.queryForObject(sql, java.lang.Long.class);
        return user_id;
    }
}
```
