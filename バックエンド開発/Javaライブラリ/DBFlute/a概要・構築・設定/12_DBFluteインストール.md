# DBFlute を自分のプロジェクトにインストールする

作成日 2024/12/24、更新日 2025/02/18

参照サイト => [Maven によるセットアップ](https://dbflute.seasar.org/ja/environment/setup/maven.html)

## 1. 手動作業

- Spring Boot + Maven のプロジェクトが、すでに用意されているものとする
- `pom.xml`を開く
- dependencies タグに以下を追加する

```xml
<dependency>
    <groupId>org.dbflute</groupId>
    <artifactId>dbflute-runtime</artifactId>
    <version>1.2.8</version>
</dependency>
```

- plugins タグに以下を追加する

```xml
<plugin>
    <groupId>org.dbflute</groupId>
    <artifactId>dbflute-maven-plugin</artifactId>
    <version>1.1.0</version>
    <configuration>
        <clientProject>demo</clientProject>
        <packageBase>com.example.demo.dao</packageBase>
    </configuration>
</plugin>
```

## 2. DBFlute エンジンのダウンロード

- ターミナルを開いて、`./mvnw -e dbflute:download`を実行する
- `demo/mydbflute`フォルダが作成された

## 3. DBFlute クライアントの作成

- ターミナルを開いて、`./mvnw -e dbflute:create-client`を実行する
- `demo/dbflute-demo`フォルダが作成された
- フォルダの中身は以下の通り

```text
--dbflute-demo/
    |--dfprop/
    |--extlib/
    |--log/
    |--output/doc/
    |--playsql/
    `--schema/
```

## 4. DBFlute プロパティの基本設定

`basicInfoMap.dfprop`

```text
map: {
    ; database = postgresql
    ; targetContainer = spring
}
```

`databaseInfoMap.dfprop`

```text
map: {
    ; driver = org.postgresql.Driver
    ; url = jdbc:postgresql://localhost:5432/YourDatabase
    ; schema = yourSchema
    ; user = yourName
    ; password = yourPassword
}
```

## 5. クラスの自動生成

- `dbflute_demo/manage.bat`を叩くと、メニューが登場する
- テーブル＆データはすでにデータベース上に作成したものとする。したがって、`0:Replace Schema`はスキップする
- `21:jdbc`を実行する。`dbflute_demo/schema/project-schema-demo.xml`が生成された
- `22:doc`を実行する。`dbflute_demo/output/doc/schema-demo.html`が生成された
- `23:generate`を実行する。`src/main/java/com/example/demo/dao`フォルダが生成された（中身は以下の通り）

```text
--src/main/java/example/demo/dao/
    |--allcommon/
    |--bsbhv/
    |--bsentity/
    |--cbean/
    |--exbhv/
    `--exentity/
```

## 5. ここでエラーが発生

`allcommon/DBFluteCOnfig.java`ファイルにて、次の import 行が解決されない

```java
import org.springframework.jdbc.datasource.DataSourceUtils;
```

DBFlute は PostgresSQL ドライバーを内包しているので、DB 接続情報を入力するだけだったが、\
自分のプロジェクトに、まだ JDBC ドライバーがインストールされていなかった

- `pom.xml`を開く
- dependencies タグに以下を追加する

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <scope>runtime</scope>
</dependency>
```

- Visual Studio Code が「ビルドファイルが更新された。Java のクラスパス/設定と同期させたいか？」と尋ねてくるので、イエスと答える
- 問題解決

## 6. 自分のプロジェクトに DB 接続情報を書き込む

`src/main/resources/application.properties`

```bash
spring.jpa.database=POSTGRESQL # これは不要では？
# これが抜けていたのでは？
# spring.datasource.driver-class-name=org.postgresql.Driver
spring.datasource.url=jdbc:postgresql://localhost:5432/YourDatabase?currentSchema=yourSchema
# URLのパラメーターの代わりにこれを使えばいいのでは？
# spring.datasource.schema=yourSchema
spring.datasource.username=yourName
spring.datasource.password=yourPassword
```
