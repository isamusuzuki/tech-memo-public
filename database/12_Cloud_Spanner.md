# Cloud Spanner

作成日 2019/11/13

## 01. Cloud Spanner とは

2017 年リリースの分散データベース

出だしの記事 => [株式会社メルペイ：Cloud Spanner と Google Kubernetes Engine でスマホ決済サービスを構築](https://cloud.google.com/blog/ja/topics/customers/merpay-cloud-spanner-google-kubernetes-engine)

リリース時の記事 => [Google がグローバルな分散データベース Cloud Spanner をローンチ、SQL と NoSQL の‘良いとこ取り’を実装 \| TechCrunch Japan](https://jp.techcrunch.com/2017/02/15/20170214google-launches-cloud-spanner-a-new-globally-distributed-database-service/)

公式トップ => [Cloud Spanner \| 大規模な自動シャーディングとトランザクションの整合性  \|  Cloud Spanner  \|  Google Cloud](https://cloud.google.com/spanner/?hl=ja)

### Cloud Spanner の料金

[料金  \|  Cloud Spanner のドキュメント  \|  Google Cloud](https://cloud.google.com/spanner/pricing?hl=ja)

東京リージョンの場合

-   1 時間あたりのノードごとの費用 ... \$1.17
-   ストレージ（GB 単位/月） ... \$0.39

## 02. RDB, NoSQL の歴史

[RDB の限界と NoSQL の登場 \- Qiita](https://qiita.com/1amageek/items/3dbbc3112493a73880d0)

最後の章で、Cloud Spanner と Cloud Firestore が登場する
