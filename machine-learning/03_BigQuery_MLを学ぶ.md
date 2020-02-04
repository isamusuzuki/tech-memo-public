# BigQuery ML を学ぶ

作成日 2020/01/15

## BigQuery ML とは

[BigQuery ML の概要  \|  BigQuery ML  \|  Google Cloud](https://cloud.google.com/bigquery-ml/docs/bigqueryml-intro?hl=ja)

BigQuery で、標準 SQL クエリを使用して、機械学習モデルを作成して実行できる\
BigQuery ML は、BigQuery のいち機能として提供されているので、データを移動する必要がない

### BigQuery ML でサポートされるモデル

- 線形回帰（予測） ... 特定の日の商品アイテムの売上、ラベルは実数値
- 2 項ロジスティック回帰（分類） ... お客様が購入するかどうか、ラベルの値は 2 つだけ
- 多項ロジスティック回帰（分類） ... 交差エントロピー損失関数をを持つ多項式分類を使う
- K 平均法クラスタリング（データセグメンテーション） ... 教師なし学習にあたる
- TensorFlow モデルのインポート ... トレーニング済みの TensorFlow モデルから BigQuery ML モデルを作成して、予測を行える

BigQuery ML モデルは、テーブルやビューなどと同様に、BigQuery データセットに格納される\
モデルのトレーニングで使用したデータ量とデータに実行したクエリに基づいて料金が発生する
