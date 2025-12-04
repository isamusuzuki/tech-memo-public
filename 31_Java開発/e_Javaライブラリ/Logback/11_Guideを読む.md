# [A Guide To Logback](https://www.baeldung.com/logback)を読む

作成日 2025/01/27、更新日 2025/12/04

## 1. セットアップ

Maven Dependency

- LogbackはSLF4J(Simple Logging Facade for Java)をネイティブ・インターフェイスとして利用する
- ロギングを開始するには、LogbackとSLF4Jの2つをpom.xmlに追加する必要がある

```xml
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-core</artifactId>
    <version>1.5.18</version>
</dependency>

<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>2.1.0-alpha1</version>
    <scope>test</scope>
</dependency>
```

Classpath

- Logbackは、logback-classic.jarもランタイムのクラスパスに追加する必要がある

```xml
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.5.18</version>
</dependency>
```

## 2. 基本例

logback.xml

```xml
<configuration>
  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
      <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
    </encoder>
  </appender>

  <root level="debug">
    <appender-ref ref="STDOUT" />
  </root>
</configuration>
```

Example.java

```java
public class Example {

    private static final Logger logger
      = LoggerFactory.getLogger(Example.class);

    public static void main(String[] args) {
        logger.info("Example log from {}", Example.class.getSimpleName());
    }
}
```

ログの出力結果

```text
20:34:22.136 [main] INFO Example - Example log from Example
```

## 3. ファイル出力

```xml
<configuration>
    <property name="LOG_DIR" value="/var/log/application" />
    <appender name="FILE" class="ch.qos.logback.core.FileAppender">
        <file>${LOG_DIR}/tests.log</file>
        <append>true</append>
        <encoder>
            <pattern>%-4relative [%thread] %-5level %logger{35} - %msg%n</pattern>
        </encoder>
    </appender>
</configuration>
```
