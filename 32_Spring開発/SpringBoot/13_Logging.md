# Logging

作成日 2025/12/04

## 1. 公式ガイド（英語）を読む

[Logging :: Spring Boot](https://docs.spring.io/spring-boot/how-to/logging.html)

Spring Bootには、ロギングについて必須の依存関係はない。Web アプリケーションの場合は、spring-boot-starter-webが必要になるだけである

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

Logbackを設定するには、logback-spring.xmlファイルを使うことができる

Spring Bootは、includedで使える、多くのLogback設定を用意している

`org/springframework/boot/logging/logback/`の下にある設定ファイル

- `defaults.xml`
- `console-appender.xml`
- `structured-console-appender.xml`
- `file-appender.xml`
- `structured-file-appender.xml`

典型的な設定ファイルは以下のようになる

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <include resource="org/springframework/boot/logging/logback/defaults.xml"/>
    <include resource="org/springframework/boot/logging/logback/console-appender.xml" />
    <root level="INFO">
        <appender-ref ref="CONSOLE" />
    </root>
    <logger name="org.springframework.web" level="DEBUG"/>
</configuration>
```
