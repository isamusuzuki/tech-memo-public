---
tags: gcp
---

# Cloud Run

作成日 2019/12/02

## 01. Cloud Runとは

公式トップ => [https://cloud.google.com/run/](https://cloud.google.com/run/)

> Cloud Run は、マネージド型のコンピューティング プラットフォームで、HTTP リクエスト経由で呼び出し可能なステートレス コンテナを実行できます。
> 
> Cloud Run はサーバーレスです。インフラストラクチャ管理が一切不要のため、最も重要な作業であるアプリケーション構築に集中できます。
> 
> Knative から作成されているため、Cloud Run を使用してフルマネージド型でコンテナを実行するか、Cloud Run with GKE を使用して Google Kubernetes Engine クラスタ内でコンテナを実行するかを選択できます。
> 

### Knativeとは

公式トップ => [https://knative.dev/](https://knative.dev/)

## 02. 解説記事を読む

[GKE と Cloud Run、どう使い分けるべきか \| Google Cloud Blog](https://cloud.google.com/blog/ja/products/containers-kubernetes/when-to-use-google-kubernetes-engine-vs-cloud-run-for-containers)

> フルマネージド Cloud Run は、Kubernetes の名前空間、ポッドでのコンテナ共存（サイドカー）、ノードの割り当てや管理といった機能を必要としないコンテナ化されたステートレス マイクロサービスにうってつけのサーバーレス プラットフォームです。
> 

[Googleの「Cloud Run」が正式サービスに。KnativeベースでDockerコンテナをサーバレスとして実行 － Publickey](https://www.publickey1.jp/blog/19/googlecloud_runknativedocker.html)

> Cloud RunはHTTPでアクセス可能なステートレスなサービスを提供するコンテナを、サーバレス環境で実行可能なサービスです。
>
> すなわち、負荷がない場合にはサービスはまったく起動されず、負荷に応じて自動的にスケール。Dockerコンテナであれば、どんな言語で作られたサービスであっても関係なく利用できます。
>
> 課金もおよそ100ミリ秒ごとに、起動しているサービス数などによって計算されます。
>
> また、Cloud RunはKubernetes上でサーバレスコンピューティング環境を実現するフレームワークとしてGoogleがオープンソースとして開発しているKnativeをベースにしています。
> 

