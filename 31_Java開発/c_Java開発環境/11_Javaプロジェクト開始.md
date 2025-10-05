# Java プロジェクトを開始する

作成日 2025/01/20、更新日 2025/01/27

参照サイト（英語） => [Getting Started with Java in VS Code](https://code.visualstudio.com/docs/java/java-tutorial)

## 1. 機能拡張をインストールする

### [Extension Pack for Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack) ... `vscjava.vscode-java-pack`

以下の機能拡張がインストールされる

- Language Support for Java by Red Hat
- Debugger for Java
- Test Runnner for Java
- Maven for Java
- Project Manager for Java
- Visual Studio IntelliCode

### [Spring Boot Extension Pack](https://marketplace.visualstudio.com/items?itemName=vmware.vscode-boot-dev-pack) ... `vmware.vscode-boot-dev-pack`

以下の機能拡張がインストールされる

- Spring Boot Tools
- Spring Initializr Java Support
- Spring Boot Dashboard

## 2. JDK をインストールする

[Amazon Corretto](https://aws.amazon.com/jp/corretto/)

> Amazon Corretto は、マルチプラットフォームで本番環境に対応した、
> 無料の Open Java Development Kit (OpenJDK) ディストリビューションです。

Corretto 21 を選ぶ

## 3. Java プロジェクトを作成する

- `Ctrl+Shift+P`をタイプしてコマンドパレットを呼び出す
- `java`とタイプして候補を探す
- `Java: Create Java Project...`コマンドを実行する
- build tools を選ぶ => Maven
- archtype を選ぶ => No Archtype
- group id を入力する => com.example
- artifact id を入力する => demo
- フォルダを選択する => 指定したフォルダにサブフォルダと pom.xml が作成される

### pom.xml に dependency を追加する

- `Ctrl+Shift+P`をタイプしてコマンドパレットを呼び出す
- `maven`とタイプして候補を探す
- `Maven: Add a dependency...`コマンドを実行する
- キーワードを入れて ENTER キーを押す => 候補一覧が登場する

## 4. `.gitignore`ファイルを設定する

[Java .gitignore template](https://gist.github.com/JonathanGawrych/32afe529fd519a7312df)

```text
# Package Files
*.jar
*.war
*.ear

# Maven
log/
target/

# Compiled Files
*.class
```

## 5. デバッグのセットアップ

- Visual Studio Code 左端の「実行とデバッグ」アイコンをクリックする
- 左枠にある「launch.json ファイルを作成します」リンクをクリックする
- デバッガーの選択は Java を選ぶ
- launch.json ファイルが生成され、その上にポップアップメニューが登場する
- ESC キーでポップアップメニューをキャンセルする
- 左枠上にあるドロップダウンメニューで Main を選んで実行ボタン（緑三角）をクリックする
- F5 キーでも実行可能

## 6. ロガーのセットアップ

pom.xml に dependency を追加する

```xml
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>2.0.16</version>
</dependency>
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-core</artifactId>
    <version>1.5.16</version>
</dependency>
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.5.16</version>
</dependency>
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.36</version>
    <scope>provided</scope>
</dependency>
```

resources フォルダに logback.xml を置く

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%date{hh:mm:ss} %level [%thread] %logger{10}\t%msg%n</pattern>
        </encoder>
    </appender>

    <root level="info">
        <appender-ref ref="STDOUT" />
    </root>
</configuration>
```

Main.java を修正する

```java
package com.example;

import lombok.extern.slf4j.Slf4j;

@Slf4j
public class Main {

    public static void main(String[] args) {
        log.info("Hello world!");
    }
}
```
