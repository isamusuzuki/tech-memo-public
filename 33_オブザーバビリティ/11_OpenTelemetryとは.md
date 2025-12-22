
# Open Telemetryとは

作成日 2025/12/09

## 1. 公式サイト（英語）を読む

[OpenTelemetry](https://opentelemetry.io/)

> High-quality, ubiquitous, and portable telemetry to enable effective observability

高品質で、どこにでも偏在する、軽量なテレメトリーが、効率的なオブザーバビリティを可能にします

> OpenTelemetry is a collection of APIs, SDKs, and tools. Use it to instrument,
> generate, collect, and export telemetry data (metrics, logs, and traces)
> to help you analyze your software’s performance and behavior.

OpenTelemetryは、API,SDK,ツールのコレクションです。テレメトリーデータ（メトリクス, ログ, トレース）の計装・生成・収集・エクスポートに、OpenTelemetryを使い、ソフトウェアのパフォーマンスや動作の分析に役立てましょう

> OpenTelemetry is generally available across several languages and is suitable for production use.

OpenTelemetryは、さまざまなプログラミング言語で一般公開されていて、プロダクションレベルの使用に最適です

## 2. 解説記事を読む

[OpenTelemetryのSpring Boot starterを使ってトレースを見てみる #Java - Qiita](https://qiita.com/a-sawaguchi/items/20b15151548aebab1d3b)

用語解説

- オブザーバビリティ ... 出力データを調べることで、システムの内部状態を把握すること
- テレメトリー ... システムから送出されるデータのこと（トレース、メトリクス、ログ）
- トレース ... マルチサービスアーキテクチャを伝播するリクエストがたどった経路の記録
- メトリクス ... システムエラー率、CPU使用率、あるサービスのリクエスト率など
- ログ ... サービス・コンポーネントが発するタイムスタンプ付きのメッセージ
- 計装 ... システムからテレメトリーが送出されるようにすること

## 3. 公式ドキュメント（日本語）を読む

[ドキュメント | OpenTelemetry](https://opentelemetry.io/ja/docs/)

OTelの略称でも知られるOpenTelemetryは、トレース、メトリクス、ログのようなテレメトリーデータを
計装、生成、収集、エクスポートするためのベンダー非依存なオープンソースのオブザーバビリティフレームワークです。

業界標準として、OpenTelemetryは90以上のオブザーバビリティベンダーによってサポートされ、
多くのライブラリ、サービス、アプリによって統合され、多くのエンドユーザーによって採用されています。

[Vendors | OpenTelemetry](https://opentelemetry.io/ecosystem/vendors/)

- [Axiom](https://axiom.co/)
- [Datadog](https://www.datadoghq.com/ja/)
- [New Relic](https://newrelic.com/jp)
- [Sentry](https://sentry.io/welcome/)
