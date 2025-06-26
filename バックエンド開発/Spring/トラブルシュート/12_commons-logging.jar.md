# commons-logging.jar を取り除く

作成日 2025/02/28、更新日 2025/03/04

## 1. デバッグコンソールにメッセージが出る

```text
Standard Commons Logging discovery in action with spring-jcl: please remove commons-logging.jar from classpath in order to avoid potential conflicts
```

## 2. 参考記事を読む

[Standard Commons Logging discovery in action with spring-jcl](https://stackoverflow.com/questions/76706612/standard-commons-logging-discovery-in-action-with-spring-jcl)

To exclude commons-logging in a Maven project, add this to your pom.xml configuration file:

```xml
<dependency>
<groupId>com.mashape.unirest</groupId>
<artifactId>unirest-java</artifactId>
<version>1.4.9</version>
<exclusions>
    <exclusion>
        <groupId>commons-logging</groupId>
        <artifactId>commons-logging</artifactId>
    </exclusion>
</exclusions>
</dependency>
```

## 3. 結果報告

commons-logging を使っているのは、OpenCSV だった

```xml
<dependency>
    <groupId>com.opencsv</groupId>
    <artifactId>opencsv</artifactId>
    <version>5.8</version>
    <exclusions>
        <exclusion>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```
