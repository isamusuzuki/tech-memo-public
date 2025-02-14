# Cloud Datalab を使う

作成日 2019/12/26

## 01. Cloud Datalab とは

公式トップ => [https://cloud.google.com/datalab/?hl=ja](https://cloud.google.com/datalab/?hl=ja)

> Cloud Datalab は、... 高度なインタラクティブ ツールです。\
> Compute Engine 上で動作し、さまざまなクラウド サービスに簡単に接続できる...\
> Cloud Datalab は、... Jupyter（旧 IPython）上に構築されています。\
> Python、SQL、JavaScript（BigQuery のユーザー定義関数用）を使った BigQuery、Cloud Machine Learning Engine、Compute Engine、Cloud Storage のデータ分析が可能になります。

[Cloud Datalab の料金  \|  Cloud Datalab ドキュメント  \|  Google Cloud](https://cloud.google.com/datalab/docs/resources/pricing?hl=ja)

> Google Cloud Datalab の使用に料金はかかりません。ただし、Cloud Datalab で使用する Google Cloud Platform リソースには課金されます。\
> コンピューティング リソース: Cloud Datalab VM インスタンスの作成時から削除時までコストが発生します。Cloud Datalab VM のデフォルトのマシンタイプは n1-standard-1 です...\
> また、ブートディスクとして使用される 20 GB の標準 Persistent Disk（永続ディスク）と、ユーザーのノートブックが格納される 200 GB の標準 Persistent Disk の料金も課金されます

ドキュメント => [https://cloud.google.com/datalab/docs/?hl=ja](https://cloud.google.com/datalab/docs/?hl=ja)

Cloud Datalab が登場するブログ記事 => [Google Cloud Platform Japan 公式ブログ: GHCN の日次気象データを BigQuery で分析する](https://cloudplatform-jp.googleblog.com/2016/10/ghcn-bigquery.html)

> Google Cloud Datalab で観測所の位置をプロットすると、観測所の密度は北米やヨーロッパ、日本では非常に良好で、アジアの大半の地域ではまずまずの状態だということがわかります。
>
> 完全な BigQuery クエリと Python のプロット コマンドについては、 GitHub 上の Datalab の完全なノートブックを参照してください。
>
> この結果を Pandas DataFrame にキャストすると、Datalab で簡単にグラフ化することができます

## 02. GCP コンソールで見つからないのななぜか

管理画面がないから。用意されたイメージをつかってインスタンスを立てるだけだから

[ビッグデータ分析プロダクト  \|  Data Analytics Products  \|  Google Cloud](https://cloud.google.com/products/big-data/?hl=ja)
