# 開発コンテナ例 Java&Postgres

作成日 2025/02/11、更新日 2025/12/10

## 1. 概要

- PostgreSQLデータベースをコンテナで用意する
- もうひとつのコンテナ（Appコンテナ）からデータベースを操作する
- AppコンテナにはJava, Mavenがインストールされている
- `perl: warning: Setting locale failed.`警告を出さないように設定する
- Java開発に必要な機能拡張をインストールする

## 2. ファイル＆ディレクトリ構造

```text
--coconut/
    |--.devcontainer/
    |   |--devcontainer.json
    |   |--docker-compose.yml
    |   `--Dockerfile
    `--demo
        |--src/main/java/com/example/
        |   `--Main.java
        `--pom.xml
```

devcontainer.json

```json
{
    "name": "Java with Database",
    "dockerComposeFile": "docker-compose.yml",
    "service": "app",
    "workspaceFolder": "/workspaces/${localWorkspaceFolderBasename}",
    "customizations": {
        "vscode": {
            "extensions": [
                "vscjava.vscode-java-pack"
            ],
            "settings": {}
        }
    }
}
```

docker-compose.yml

```yaml
services:
  app:
    container_name: javadev
    build:
      context: .
      dockerfile: Dockerfile
    environment:
      POSTGRES_HOSTNAME: postgresdb
      POSTGRES_DB: postgres
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    volumes:
      - ../..:/workspaces:cached
    command: sleep infinity
    network_mode: service:db
  db:
    container_name: postgresdb
    image: postgres:latest
    restart: unless-stopped
    volumes:
      - postgres-data:/var/lib/postgres/data
    environment:
      POSTGRES_DB: postgres
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
volumes:
  postgres-data:
```

Dockerfile

```dockerfile
FROM maven:3.9.9-amazoncorretto-21-debian-bookworm

RUN apt-get update && apt-get install -y \
  postgresql-client \
  git \
  locales \
  && rm -rf /var/lib/apt/lists/*

RUN sed -i -E 's/# (ja_JP.UTF-8)/\1/' /etc/locale.gen \
  && locale-gen \
  && update-locale LANG=ja_JP.UTF-8
ENV LANG=ja_JP.UTF-8 LANGUAGE=ja_JP:ja LC_ALL=ja_JP.UTF-8
```

## 3. Javaコード

pom.xmlを編集して、PostgreSQL JDBC Driverをインストールする

```xml
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.7.5</version>
</dependency>
```

Main.java

```java
package com.example;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class Main {
    public static void main(String[] args) {
        // 環境変数を取得する
        String hostname = System.getenv("POSTGRES_HOSTNAME");
        String db = System.getenv("POSTGRES_DB");
        String user = System.getenv("POSTGRES_USER");
        String password = System.getenv("POSTGRES_PASSWORD");

        String url = String.format(
            "jdbc:postgresql://%s:5432/%s",hostname, db);

        System.out.println("url: " + url);
        System.out.println("user: " + user);
        System.out.println("password: " + password);

        Connection conn = null;
        Statement stmt = null;
        ResultSet rset = null;

        try{
            //PostgreSQLへ接続
            conn = DriverManager.getConnection(url, user, password);

            //SELECT文の実行
            stmt = conn.createStatement();
            String sql = "SELECT name FROM myfriends;";
            rset = stmt.executeQuery(sql);

            //SELECT結果の受け取り
            while(rset.next()){
                String col = rset.getString(1);
                System.out.println(col);
            }
        } catch (SQLException e){
            e.printStackTrace();
        } finally {
            try {
                if(rset != null)rset.close();
                if(stmt != null)stmt.close();
                if(conn != null)conn.close();
            }
            catch (SQLException e){
                e.printStackTrace();
            }
        }
    }
}
```
