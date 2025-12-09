# Lombokをログ出力に使う

作成日 2024/12/24、更新日 2025/12/10

## 1. 解説記事を読む

[Spring Boot で SLF4J+logback+Lombok を使いログ出力を行う](https://www.aruse.net/entry/2022/07/09/220510#SLF4J-%E3%81%A8%E3%81%AF)

> - SLF4J単体では動作せず、logbackなどのロギングフレームワークが必要になる
> - log4jの開発者が、log4jの後継として開発したのが、logbackとSLF4J
> - SLF4Jとlogbackだけだと、全てのクラスでLoggerの宣言をする必要がある
> - Lombokを使用すると、`@Slf4j`のアノテーションを付けるだけでログを出力できるようになる

```java
@Slf4j
public class LogSampleClass {
  public void log() {
    log.info("infoログを出力しました");
  }
}
```
