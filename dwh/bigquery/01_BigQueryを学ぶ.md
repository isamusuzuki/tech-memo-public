# BigQuery を学ぶ

作成日 2019/12/09

## 01. BigQuery とは

サーバーレスで、スケーラビリティに優れた、企業向けデータウェアハウス\
データベースの操作には SQL を使用できる

公式トップ => [https://cloud.google.com/bigquery/?hl=ja](https://cloud.google.com/bigquery/?hl=ja)

ドキュメント　=> [https://cloud.google.com/bigquery/docs/?hl=ja](https://cloud.google.com/bigquery/docs/?hl=ja)

> BigQuery は、ギガバイトからペタバイト規模のデータに対して SQL クエリを超高速で実行します。
>
> BigQuery では Tableu、MicroStrategy、Looker、データポータルなどの一般的な BI ツールがサポートされているので、誰でも簡単に洗練されたレポートやダッシュボードを作成できます。

### BigQuery の料金

- ストレージ ... \$0.02 GB/月
- ストリーミングインサート ... \$0.01 200MB あたり
- データの読み込み、コピー、エクスポート ... 無料
- クエリ ... \$5 TB あたり（毎月 1TB まで無料）

## 02. スタートガイド（GCP コンソール編）を写経する

[https://cloud.google.com/bigquery/docs/quickstarts/quickstart-web-ui?hl=ja](https://cloud.google.com/bigquery/docs/quickstarts/quickstart-web-ui?hl=ja)

### 一般公開データセットに対してクエリを実行する

GCP コンソール ＞ ナビゲーションメニュー ＞ ビッグデータ ＞ BigQuery をクリック\
＞ BigQuery のページ ＞ クエリエディタが開いていればオーケー\
＞ 開いてなければ、右上「＋クエリを新規作成」をクリック\
＞ 「エディタ」エリアに以下のクエリをコピペする ＞ 実行ボタンをクリックする

```sql
SELECT
  name, gender,
  SUM(number) AS total
FROM
  `bigquery-public-data.usa_names.usa_1910_2013`
GROUP BY
  name, gender
ORDER BY
  total DESC
LIMIT
  10
```

＞ 「クエリ結果」エリアに 10 行のテーブルが登場する\
＞ クエリ完了（経過時間 2.4 秒、処理されたバイト数 99.9MB）

### テーブルにデータを読み込む

米国社会保障局から提供された、人気のある赤ちゃんの名前に関するデータを\
ダウンロードする ＞ names.zip (6.86MB) ＞ 自分のマシンで解凍する\
＞ 1 個の PDF ファイルと 139 個のテキストファイル\
＞ yob2014.txt を開く\
＞ 「名前、性別(M or F)、子供の数」の 3 列の CSV データ、ヘッダーなし

BigQuery のページ ＞ 左枠 ＞ リソース ＞ プロジェクト名をクリック\
＞ 右枠 ＞ 「データセットを作成」をクリック ＞ 右からスライダーが登場\
＞ データセット ID: babynames ＞ データのロケーション: 米国 (US)\
＞ 有効期限: 無期限 ＞ 暗号化: Google が管理する鍵 ＞ 「データセットを作成」ボタン

BigQuery のページ ＞ 左枠 ＞ リソース ＞ babynames データセットをクリック\
＞ 右枠 ＞ 「テーブルを作成」をクリック ＞ 右からスライダーが登場\
＞ ソース ＞ テーブルの作成元: アップロード\
＞ ファイルを選択: yob2014.txt ＞ ファイル形式: CSV\
＞ 送信先 ＞ テーブル名: names_2014\
＞ スキーマ ＞ 「テキストとして編集」をチェックオン\
＞ `name:string,gender:string,count:integer`\
＞ 「テーブルを作成」ボタンをクリック

BigQuery のページ ＞ 左枠 ＞ リソース ＞ names_2014 をクリック\
＞ 右枠 ＞ プレビュー ＞ データを確認\

右上の「＋クエリを新規作成」をクリック\
＞ 「エディタ」エリアに以下のクエリをコピペする ＞ 実行ボタンをクリックする

```sql
SELECT
  name,
  count
FROM
  `babynames.names_2014`
WHERE
  gender = 'M'
ORDER BY
  count DESC
LIMIT
  5
```

### クリーンアップ

BigQuery のページ ＞ 左枠 ＞ リソース ＞ babynames データセットをクリック\
＞ 右枠 ＞ 「データセットを削除」をクリック

## 03. 事前定義されたロールが持つ権限を確認する

[事前定義された役割と権限  \|  BigQuery  \|  Google Cloud](https://cloud.google.com/bigquery/docs/access-control?hl=ja)

- BigQuery 管理者 ... bigquery.\*
- BigQuery データ編集者 ... bigquery.tables.getData, bigquery.tables.updateData
- BigQuery データオーナー ... bigquery.tables.\*
- BigQuery データ閲覧者 ... bigquery.tables.getData
- BigQuery ジョブユーザー ... bigquery.jobs.create
- BigQuery メタデータ閲覧者 ... bigquery.tables.get
- BigQuery ユーザー ... bigquery.tables.list

Client クラスの query()メソッドは、job.QueryJob クラスを返す。\
ジョブを作成する権限 => `bigquery.jobs.create`権限が必要

事前定義された役割の中では「BigQuery ジョブユーザー」が、\
ほぼ`bigquery.jobs.create`権限だけを持つ最低限のものである。\
これをサービスアカウントに与える

さらにテーブルのデータを取得するには、`bigquery.tables.getData`権限が必要\
また、テーブルにデータを挿入するには、`bigquery.tables.updateData`権限が必要

「BigQuery データ閲覧者」は、getData を持っているが、updateData がない。\
「BigQuery データ編集者」ならば、両方を持つ。これをサービスアカウントに与える

### 権限の範囲を確認する

[データセットへのアクセスの制御  \|  BigQuery  \|  Google Cloud](https://cloud.google.com/bigquery/docs/dataset-access-controls?hl=ja)

> 現時点では、テーブル、ビュー、列、行に対する権限を付与することはできません。\
> データセットは、BigQuery でのアクセス制御をサポートする最下位レベルのリソースです。
>
> データセットの作成時にアクセス制御を適用するには、datasets.insert API メソッドを呼び出します。\
> データセットの作成後は、datasets.patch API メソッドを呼び出すことでデータセットにアクセス制御を適用できます。
