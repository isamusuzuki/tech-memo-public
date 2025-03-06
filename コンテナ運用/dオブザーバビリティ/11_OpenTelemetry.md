# OpenTelemetry

作成日 2025/03/06

## 1. オープンテレメトリーとは

公式サイト（日本語） => [OpenTelemetry](https://opentelemetry.io/ja/)

公式ドキュメント（日本語） => [ドキュメント | OpenTelemetry](https://opentelemetry.io/ja/docs/)

> OTelの略称でも知られるOpenTelemetryは、トレース、メトリクス、ログのようなテレメトリーデータを\
> 計装、生成、収集、エクスポートするためのベンダー非依存なオープンソースのオブザーバビリティフレームワークです。

参照サイト => [オープンテレメトリーとは？](https://qiita.com/keiji_nonoda/items/5ca2e5dcd356e66a2a56)

> ホスト内のオープンテレメトリーcollectorでデータを収集し、\
> バックエンドとしてDatadogやNew Relicを指定してデータを送信するイメージ

## 2. Spring Boot Starter

ドキュメント（英語） => [Spring Boot starter](https://opentelemetry.io/docs/zero-code/java/spring-boot-starter/)

Add the dependency given below to enable the OpenTelemetry starter.

```xml
<dependency>
    <groupId>io.opentelemetry.instrumentation</groupId>
    <artifactId>opentelemetry-spring-boot-starter</artifactId>
</dependency>
```
