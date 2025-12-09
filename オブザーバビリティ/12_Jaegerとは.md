# Jaegerとは

作成日 2025/12/09

## 1. 解説記事を読む

[Jaeger超入門 #Go - Qiita](https://qiita.com/tamura__246/items/7e22fcc17a09b1c129a2)

「イエーガー」と読む。ドイツ語で「猟師」を意味する。開発元はUber

分散トレーシング（マイクロサービスアーキテクチャでリクエストのフローを追跡）を実現するOSS

複数のサービスで構成されたアプリケーションの遅延の原因を特定するようなケースで役に立つ

用語解説

- スパン ... 処理の開始から終了まで
- トレース ... 複数のスパンで形成される処理の全体

アーキテクチャ

- Open Telemetry SDKを組み込んだアプリケーションがトレースデータを取得
- Jaeger Collectorがトレースデータを収集して、DBに保存
- Jaeger UIがわかりやすく可視化
- デフォルトのストレージはメモリだが、推奨はOpenSearchまたはElasticSearch

## 2. 公式サイト（英語）を読む

[Jaeger](https://www.jaegertracing.io/)

Get startedをクリック

Jaegerを動かすもっとも簡単な方法は、コンテナで始めることである

```bash
docker run --rm --name jaeger \
  -p 16686:16686 \
  -p 4317:4317 \
  -p 4318:4318 \
  -p 5778:5778 \
  -p 9411:9411 \
  cr.jaegertracing.io/jaegertracing/jaeger:2.13.0
```

## 3. HotRODデモを試す

`https://github.com/jaegertracing/jaeger/blob/v2.13.0/examples/hotrod/README.md`

- Download docker-compose.yml <= `https://github.com/jaegertracing/jaeger/blob/main/examples/hotrod/docker-compose.yml`
- `JAEGER_VERSION=2.13 docker compose -f docker-compose.yml up`
- Jaeger UI => `http://localhost:16686`
- HotROD app => `http://localhost:8080`
- `docker compose -f docker-compose.yml down`

PowerShellで動かす

```bash
# 環境変数を設定
$env:JAEGER_VERSION="2.13.0"
$env:HOTROD_VERSION="1.76.0"
docker compose -f docker-compose.yml up
docker compose -f docker-compose.yml down
```

docker-compose.yml

```yaml
# To run a specific version of Jaeger, use environment variable, e.g.:
#     JAEGER_VERSION=2.0.0 HOTROD_VERSION=1.63.0 docker compose up

services:
  jaeger:
    image: ${REGISTRY:-}jaegertracing/jaeger:${JAEGER_VERSION:-latest}
    ports:
      - "16686:16686"
      - "4317:4317"
      - "4318:4318"
    environment:
      - LOG_LEVEL=debug
    networks:
      - jaeger-example

  hotrod:
    image: ${REGISTRY:-}jaegertracing/example-hotrod:${HOTROD_VERSION:-latest}
    # To run the latest trunk build, find the tag at Docker Hub and use the line below
    # https://hub.docker.com/r/jaegertracing/example-hotrod-snapshot/tags
    #image: jaegertracing/example-hotrod-snapshot:0ab8f2fcb12ff0d10830c1ee3bb52b745522db6c
    ports:
      - "8080:8080"
      - "8083:8083"
    command: ["all"]
    environment:
      - OTEL_EXPORTER_OTLP_ENDPOINT=http://jaeger:4318
    networks:
      - jaeger-example
    depends_on:
      - jaeger

networks:
  jaeger-example:
```
