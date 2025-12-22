# Spring Bootに組み込む

作成日 2025/12/09、更新日 2025/12/10

## 1. 公式ブログ（英語）を読む

[OpenTelemetry with Spring Boot](https://spring.io/blog/2025/11/18/opentelemetry-with-spring-boot)

Open TelemetryをSpring Bootに統合するには、いくつか選択肢がある

Spring Boot 4.0より、`spring-boot-starter-opentelemetry`が提供され、`start.spring.io`の依存関係の選択肢に"OpenTelemetry"が登場するようになった

## 2. 解説記事を読む

[SpringBoot4のオブザーバビリティ機能を試してみる #Java - Qiita](https://qiita.com/a-sawaguchi/items/f74e27940d4684b07fc8)

2025年11月にSpringBoot4.0がリリースされ、OpenTelemetryに関する新しい機能が追加された

- `spring-boot-starter-opentelemetry` ... Springがリリース
- `opentelemetry-spring-boot-starter` ... OpenTelemetryがリリース

collectorはOpenTelemetryのコレクタのことです。
jaeger、prometheus、loki、grafanaは、テレメトリデータを可視化するためのツールです。
今回はトレース、メトリクス、ログのすべてを可視化できるGrafanaをメインで使っていきます。

src/main/resources/application.yml

```yaml
management:
  otlp:
    metrics:
      export:
        url: http://localhost:4318/v1/metrics
  opentelemetry:
    tracing:
      export:
        url: http://localhost:4318/v1/traces
      sampling:
        probability: 1.0
    logging:
      export:
        otlp:
          endpoint: http://localhost:4318/v1/logs
  observations:
    annotations:
      enabled: true
```

## 3. 公式のサンプルアプリ

[mhalbritter/spring-boot-and-opentelemetry: Showcase of Spring Boot using OpenTelemetry](https://github.com/mhalbritter/spring-boot-and-opentelemetry)

READMEはない。説明は、すべて元記事のほうにある

[Let's see it in action](https://spring.io/blog/2025/11/18/opentelemetry-with-spring-boot#lets-see-it-in-action)

### Docker OpenTelemetry LGTM

[Docker OpenTelemetry LGTM | OpenTelemetry documentation](https://grafana.com/docs/opentelemetry/docker-lgtm/)

`grafana/otel-lgtm`イメージは、OpenTelemetryバックエンドを最も簡単にセットアップする方法である

OpenTelemetry Collector, Grafana, Loki, Mimir, Tempoが含まれていて、開発・デモ・テスト環境に適している

[grafana/otel-lgtm - Docker Image](https://hub.docker.com/r/grafana/otel-lgtm)

- OpenTelemetry Collector ... receiving Open Telemetry data
- Prometheus ... metrics database
- Tempo ... trace database
- Loki ... logs database
- Grafana ... for visualization
