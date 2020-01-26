# DEGCP1 Week1

作成日 2020/01/05、更新日 2020/01/12

## Recommending Products using Cloud SQL and Spark

- Apache Spark ML
- Cloud DataProc
- Cloud SQL

Recommendation system = data, model, training infra

ML = Examples, not rules

rating = explicitly or implicitly training

Stream or Bacth

DEMO: Spark, Hadoop on Cloud DataProc

DataProc menu > Cluster, Jobs, Workflow

New Cluster > Standard(1 master, N workers)\
Mater node (4 vCPUs, 15GB, 500GB)\
Nodes 2 (minimum)

Submit Job\
JobType: Spark\
JavaClass: fill\
JarFile: fill\
arguments: blank

SparkML jobs on Hadoop Clusters\
=> オンプレではリソースの配分が難しい\
Job1 50%, Job2 50%, Job3 starved, Job4 starved

Cloud DataProc の特徴

- Hadoop Yarn Metrics
- Store data off cluster
- Preemptive VMs (80% cheaper)

データを Cloud Storage に置くのは簡単\
`hdfs://` => `gs://`

HBase = BigTable

DEMO: Cloud SQL - Spark

- Create Cloud SQL Instance
- Create table with SQL file
- Populate table from CSV file
- Accomodation table, Rating table
- Allow access to Cloud SQL

Create Cluster on Cloud DataProc

Name: rentals\
Zone: us-central1-a\
Total Worker nodes: 2\
rentals-m: 130.211.127.217\
rentals-w-0: 34.67.220.181\
rentals-w-1: 104.154.165.245

## Predict Visitor Purchases with BigQuery ML

Introduction to BigQuery

DEMO

BigQuery Storage Service = Colossus File System

- Cloud DataPrep
- Cloud DataFlow

BigQuery は Array と Struct を持つ\
JOIN させたデータをひとつのテーブルにまとめて保存できる

## Apply machine learning using SQL with BigQuery ML

### Choose the right model type for your structured data use case

```text
------Supervided ML 教師あり学習
    |    |--forecast 予測（数値を予測する）
    |    |    `--Linear Regression 線形回帰
    |    |--classify 分類（文字列を予測する）
    |    |    |--binary
    |    |    |    `--Logistic Regression ロジスティック回帰
    |    |    `--Multi-Class
    |    |        `--Logistic Regression with multi case option
    |    `--recomeend オススメ
    |        `--Matrix Factorization
    |
    `--Unsupervised ML 教師なし学習
        `--Clustering クラスタ分け
```

### Predicting customer LTV with a ML model

LTV ... Life time value

### データサイエンティストや機械学習のプロが使う専門用語

- レコードもしくは行 ... インスタンス, オブザベーション
- Label ... 正しい答え、アウトプット
- Feature Column ... インプットされるデータ

### BigQueryML: Create models with SQL

Build machine learning models in minutes with SQL

Create model with a SQL statement

```sql
CREATE MODEL numbikes.model
OPTIONS
  (model_type='linear_reg', labels=[`num_trips`]) AS WITH bike_data AS
 (
 SELECT
   COUNT(*) as num_trips,
 )
```

Predict

```sql
SELECT
  predicted_num_trips, num_trips, trip_data
FROM
  ml.PREDICT(MODEL `numbikes.model`, (WITH bike_data AS
(
  SELECT
    COUNT(*) as num_trips,
)

  ))
```

#### BigQueryML とは

ドキュメント => [BigQuery ML の概要](https://cloud.google.com/bigquery-ml/docs/bigqueryml-intro?hl=ja)

サポートされるモデル

- 線形回帰（予測） ... ラベルは実数値
- 2 項ロジスティック回帰（分類） ... ラベルの値は 2 つだけ
- 多孔ロジスティック回帰（分類）... 複数の考えうる値を予測する
- K 平均法クラスタリング（データセグメンテーション） ... K 平均法は教師なし学習
- TensorFlow モデルのインポート ... トレーニング済みの TensorFlow モデルから BigQueryML モデルを作成する

### ML projects phases

1. ETL into BigQuery
2. Preprocess Features
3. "CREATE MODEL name OPTION () AS query"
4. "SELECT column1, column2 ... FROM ML.EVALUATE(MODEL name)"
5. "SELECT \* FROM ML.PREDICT(name)"

#### ETL とは

ETL とは、Extract（抽出）・Transform（変換）・Load（格納）の略で、\
データ統合時に発生する各プロセスの頭文字をとったもの

### BigQuery ML key features

```sql
CREATE OR REPLACE MODEL
  `mydataset.mymodel`
OPTION
(
  model_type='linear_reg',
  input_label_cols='sales',
  ls_init_learn_rate=.15,
  l1_reg=1,
  max_iterations=5
) AS
```

### BigQuery ML Cheatsheet

- Label ... specify column in OPTIONS using input_label_cols
- Feature ... SELECT \* FROM ML.FEATURE_INFO(MODEL `mydataset.mymode`)
- Model ... resides in BigQuery dataset
- ModelType ... Linear Regression, Logistic Regression
- TrainingProgress ... SELECT \* FROM ML.TRAINING_INFO(MODEL `mydataset.mymode`)
- Inspect Weights ... SELECT \* FROM ML.WEIGHTS(Model `mydataset.mymodel`, (query))
- Evaluation ... SELECT \* FROM ML.EVALUATE(model `mydatasets.mymodel`)
- Prediction ... SELECT \* FROM ML.PREDICT(MODEL `mydataset.mymodel`, (query))

## Lab: Predict Visitor Purchases with BigQuery ML

Classify returning customers with BigQuery ML

Visitor and order data has been loaded into BigQuery\
Build a machine learning model to predict whether a visitor will return for more
purchases later.

### Predict Visitor Purchases with a Classification Model with BigQuery ML

Create a classification model (logistic regression) in BigQuery ML

### Setup and requirements

[Google Analytics - BigQuery Export schema](https://support.google.com/analytics/answer/3437719?hl=en)

| Field Name                       | Data Type | Description                                                                                        |
| -------------------------------- | --------- | -------------------------------------------------------------------------------------------------- |
| fullVisitorId                    | STRING    | The unique visitor ID                                                                              |
| totals                           | RECORD    | aggregate values across the session                                                                |
| totals.transactions              | INTEGER   | Total number of ecommerse transactions within the session.                                         |
| totals.newVisit                  | INTEGER   | Total number of new users in session. If this is the first, this values is 1, otherwise it is null |
| totals.bounces                   | INTEGER   | Total bounces. For a bounced session, the value is 1, otherwise it is null                         |
| totals.timeOnSite                | INTEGER   | Total time of the session expressed in seconds                                                     |
| hits                             | RECORD    | populated for any and all types of hits                                                            |
| hits.product.v2ProductName       | STRING    | Product Name                                                                                       |
| hits.product.v2ProductCategory   | STRING    | Product Category                                                                                   |
| hits.product.productQuantity     | INTEGER   | The quantity of the product purchased                                                              |
| hits.product.localProductRevenue | INTEGER   | The revenue of the product in local currency                                                       |

### Explore ecommerce data

Question: Out of the total visitors who visited our website, what % made a purchase?

Question: What are the top 5 selling products?

Question: How many visitors bought on subsequent visits to the website?

### Identify an objective

blank

### Select features and create your training dataset

- feature = input ... bounces, time_on_site
- label = output ... will_buy_on_return_visit

Question: do you think time_on_site and bounces will be a good indicator?

### Create a BigQuery dataset to store models

blank

### Select a BQML model type and specify options

| Model          | Type         | Label         | Example                      |
| -------------- | ------------ | ------------- | ---------------------------- |
| Forecasting    | linear_reg   | Numeric value | sales for next year          |
| Classification | logistic_reg | 0 or 1        | an email as spam or not spam |

create a model and specify model options

```sql
CREATE OR REPLACE MODEL `ecommerce.classification_model`
OPTIONS
(
model_type='logistic_reg',
labels = ['will_buy_on_return_visit']
)
AS

#standardSQL
SELECT
  * EXCEPT(fullVisitorId)
FROM

  # features
  (SELECT
    fullVisitorId,
    IFNULL(totals.bounces, 0) AS bounces,
    IFNULL(totals.timeOnSite, 0) AS time_on_site
  FROM
    `data-to-insights.ecommerce.web_analytics`
  WHERE
    totals.newVisits = 1
    AND date BETWEEN '20160801' AND '20170430') # train on first 9 months
  JOIN
  (SELECT
    fullvisitorid,
    IF(COUNTIF(totals.transactions > 0 AND totals.newVisits IS NULL) > 0, 1, 0) AS will_buy_on_return_visit
  FROM
      `data-to-insights.ecommerce.web_analytics`
  GROUP BY fullvisitorid)
  USING (fullVisitorId)
;
```

### Evaluate classification model performance

minimize the False Positive Rate\
maximize the True Positive Rate

ROC (Receiver Operating Charastristic) curve\
ROC 曲線とは、FPR を横軸、TPR を縦軸にプロットしたもの\
理想的な予測ができていると、(FPR, TPR) = (0, 1) 左上の点を通る\
分類の性能が悪いと、ROC 曲線は直線に近づいていく

AUC ... Area under the curve

ROC_AUC ... Area under an ROC curve ROC 曲線下の面積（評価指標）\
理想的な分類がでできるモデルの ROC_AUC スコアは 1.0\
ROC_AUC スコアが 0.5 を下回る場合は、予測スコアを反転すると 0.5 を上回るようになる\
そのため、最低スコアは 0.5 である

[scikit\-learn で ROC 曲線とその AUC を算出 \| note\.nkmk\.me](https://note.nkmk.me/python-sklearn-roc-curve-auc-score/)

Evaluate query

```sql
SELECT
  roc_auc,
  CASE
    WHEN roc_auc > .9 THEN 'good'
    WHEN roc_auc > .8 THEN 'fair'
    WHEN roc_auc > .7 THEN 'not great'
  ELSE 'poor' END AS model_quality
FROM
  ML.EVALUATE(MODEL ecommerce.classification_model,  (

SELECT
  * EXCEPT(fullVisitorId)
FROM

  # features
  (SELECT
    fullVisitorId,
    IFNULL(totals.bounces, 0) AS bounces,
    IFNULL(totals.timeOnSite, 0) AS time_on_site
  FROM
    `data-to-insights.ecommerce.web_analytics`
  WHERE
    totals.newVisits = 1
    AND date BETWEEN '20170501' AND '20170630') # eval on 2 months
  JOIN
  (SELECT
    fullvisitorid,
    IF(COUNTIF(totals.transactions > 0 AND totals.newVisits IS NULL) > 0, 1, 0) AS will_buy_on_return_visit
  FROM
      `data-to-insights.ecommerce.web_analytics`
  GROUP BY fullvisitorid)
  USING (fullVisitorId)

));
```

=> 0.72 not great という結果になった

### Improve model performance with Feature Engineering

改良版のモデルを作成する

```sql
CREATE OR REPLACE MODEL `ecommerce.classification_model_2`
OPTIONS
  (model_type='logistic_reg', labels = ['will_buy_on_return_visit']) AS

WITH all_visitor_stats AS (
SELECT
  fullvisitorid,
  IF(COUNTIF(totals.transactions > 0 AND totals.newVisits IS NULL) > 0, 1, 0) AS will_buy_on_return_visit
  FROM `data-to-insights.ecommerce.web_analytics`
  GROUP BY fullvisitorid
)

# add in new features
SELECT * EXCEPT(unique_session_id) FROM (

  SELECT
      CONCAT(fullvisitorid, CAST(visitId AS STRING)) AS unique_session_id,

      # labels
      will_buy_on_return_visit,

      MAX(CAST(h.eCommerceAction.action_type AS INT64)) AS latest_ecommerce_progress,

      # behavior on the site
      IFNULL(totals.bounces, 0) AS bounces,
      IFNULL(totals.timeOnSite, 0) AS time_on_site,
      totals.pageviews,

      # where the visitor came from
      trafficSource.source,
      trafficSource.medium,
      channelGrouping,

      # mobile or desktop
      device.deviceCategory,

      # geographic
      IFNULL(geoNetwork.country, "") AS country

  FROM `data-to-insights.ecommerce.web_analytics`,
     UNNEST(hits) AS h

    JOIN all_visitor_stats USING(fullvisitorid)

  WHERE 1=1
    # only predict for new visits
    AND totals.newVisits = 1
    AND date BETWEEN '20160801' AND '20170430' # train 9 months

  GROUP BY
  unique_session_id,
  will_buy_on_return_visit,
  bounces,
  time_on_site,
  totals.pageviews,
  trafficSource.source,
  trafficSource.medium,
  channelGrouping,
  device.deviceCategory,
  country
);
```

=> 改良版の roc_auc は、0.91 になった

### Predict which new visitors will come back and purchase

予想する

```sql
SELECT
*
FROM
  ml.PREDICT(MODEL `ecommerce.classification_model_2`,
   (

WITH all_visitor_stats AS (
SELECT
  fullvisitorid,
  IF(COUNTIF(totals.transactions > 0 AND totals.newVisits IS NULL) > 0, 1, 0) AS will_buy_on_return_visit
  FROM `data-to-insights.ecommerce.web_analytics`
  GROUP BY fullvisitorid
)

  SELECT
      CONCAT(fullvisitorid, '-',CAST(visitId AS STRING)) AS unique_session_id,

      # labels
      will_buy_on_return_visit,

      MAX(CAST(h.eCommerceAction.action_type AS INT64)) AS latest_ecommerce_progress,

      # behavior on the site
      IFNULL(totals.bounces, 0) AS bounces,
      IFNULL(totals.timeOnSite, 0) AS time_on_site,
      totals.pageviews,

      # where the visitor came from
      trafficSource.source,
      trafficSource.medium,
      channelGrouping,

      # mobile or desktop
      device.deviceCategory,

      # geographic
      IFNULL(geoNetwork.country, "") AS country

  FROM `data-to-insights.ecommerce.web_analytics`,
     UNNEST(hits) AS h

    JOIN all_visitor_stats USING(fullvisitorid)

  WHERE
    # only predict for new visits
    totals.newVisits = 1
    AND date BETWEEN '20170701' AND '20170801' # test 1 month

  GROUP BY
  unique_session_id,
  will_buy_on_return_visit,
  bounces,
  time_on_site,
  totals.pageviews,
  trafficSource.source,
  trafficSource.medium,
  channelGrouping,
  device.deviceCategory,
  country
)

)

ORDER BY
  predicted_will_buy_on_return_visit DESC;
```

- predicted_will_buy_on_return_visit ... whether the visitor will buy later (1=yes)
- predicted_will_buy_on_return_visit_probs.label ... binary classifier for yes/no
- predicted_will_buy_on_return_visit.prob ... confidence in it's prediction (1 = 100%)

label が事象、すなわち 1 と 0。prob は、それぞれの事象の確かさ。prob は合計すると 1 になる。

[Precision and recall \- Wikipedia](https://en.wikipedia.org/wiki/Precision_and_recall)

### おまけ

Google Sheets は、BigQuery から直接読み込めるらしい

[Connecting BigQuery and Google Sheets to help with hefty data analysis \| Google Cloud Blog](https://cloud.google.com/blog/products/g-suite/connecting-bigquery-and-google-sheets-to-help-with-hefty-data-analysis)

> With the Sheets data connector for BigQuery, you can analyze and share large datasets from BigQuery right from within your spreadsheet. Connect up to 10,000 rows of data from BigQuery into Sheets (with a simple SQL statement that you can get from a data analyst), and analyze it using the Explore feature, or by creating charts or pivot tables, in your spreadsheet.
