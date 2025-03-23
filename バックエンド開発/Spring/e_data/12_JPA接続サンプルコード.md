# JPA 接続サンプルコード

作成日 2025/02/12

JPA = Jakarta Persistence API

- RDB で管理されているレコードを Java オブジェクトにマッピングする
- マッピングされた Java オブジェクトに対して行われた操作を、RDB のレコードに反映する

## 1. PostgreSQL データベースを用意する

`PostgreSQL/12_作業環境構築.md`を参照

## 2. サンプルアプリを立ち上げる

[spring initializr](https://start.spring.io/)で追加する Dependencies

- Lombok
- Spring Web
- Spring Data JPA
- PostgreSQL Driver

```text
--banana/
    |--src/main/java/com/example/banana/
    |   |--entity/
    |   |   `--MyFriendsEntity.java
    |   |--repository/
    |   |   `--MyFriendsRepository.java
    |   |--service/
    |   |   |--impl/
    |   |   |   `--MyFriendsServiceImpl.java
    |   |   `--MyFriendsService.java
    |   `--MyFriendsController.java
    `--src/main/resources/
        `--application.properties
```

- どこにも SQL クエリーを書く箇所がない

application.properties

```bash
## connect to database
spring.datasource.driver-class-name=org.postgresql.Driver
spring.datasource.url=jdbc:postgresql://localhost:5432/testdb
spring.datasource.username=dbuser
spring.datasource.password=dbpasswd

## JPA Config
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```

## 3. Java コードを書く

MyFriendsEntity.java

```java
package com.example.banana.entity;

import java.util.Date;
import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.Table;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Entity
@Table(name = "myfriends")
@Data
@AllArgsConstructor
@NoArgsConstructor
public class MyFriendsEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private int id;

    @Column(name = "name")
    private String name;

    @Column(name = "address")
    private String address;

    @Column(name = "created_on")
    private Date created_on;
}
```

MyFriendsRepository.java

```java
package com.example.banana.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import com.example.banana.entity.MyFriendsEntity;

@Repository
public interface MyFriendsRepository extends JpaRepository<MyFriendsEntity, Integer> {
}
```

MyFriendsService.java

```java
package com.example.banana.service;

import java.util.List;
import com.example.banana.entity.MyFriendsEntity;

public interface MyFriendsService {
    List<MyFriendsEntity> findAllMyFriends();
}
```

MyFriendsServiceImpl.java

```java
package com.example.banana.service.impl;

import java.util.List;
import org.springframework.stereotype.Service;
import com.example.banana.entity.MyFriendsEntity;
import com.example.banana.repository.MyFriendsRepository;
import com.example.banana.service.MyFriendsService;

@Service
public class MyFriendsServiceImpl implements MyFriendsService {
    private final MyFriendsRepository myFriendsRepository;

    public MyFriendsServiceImpl(MyFriendsRepository myFriendsRepository) {
        this.myFriendsRepository = myFriendsRepository;
    }

    @Override
    public List<MyFriendsEntity> findAllMyFriends() {
        return myFriendsRepository.findAll();
    }
}
```

MyFriendsController.java

```java
package com.example.banana;

import com.example.banana.entity.MyFriendsEntity;
import com.example.banana.service.MyFriendsService;
import java.util.List;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyFrendsController {
    private final MyFriendsService myFriendsService;

    public MyFrendsController(MyFriendsService myFriendsService) {
        this.myFriendsService = myFriendsService;
    }

    @GetMapping("/myfriends")
    public ResponseEntity<List<MyFriendsEntity>> getAll() {
        List<MyFriendsEntity> list = myFriendsService.findAllMyFriends();
        return ResponseEntity
            .status(HttpStatus.OK)
            .header("Custom-Header", "HiHo")
            .body(list);
    }
}
```
