# DEGCP2 Week1

作成日 2020/01/26、更新日 2020/01/28

## Building a data warehouse

Learning Objectives

- Discuss requirements of a modern warehouse
- Understand why BigQuery is the scalable data warehousing solution on GCP
- Undersntad core concepts of BigQuery and review options of loading data into BigQuery

### The Modern Data Warehouse 3min

blank

### Intro to BigQuery 1min

blank

### Demo: Querying TB of Data in seconds 7min

[training\-data\-analyst/bigquery_scale\.md at master · GoogleCloudPlatform/training\-data\-analyst](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/data-engineering/demos/bigquery_scale.md)

Query 10 billion rows of Wikipedia data in BigQuery

```sql
SELECT
  language,
  title,
  SUM(views) AS views
FROM
  `bigquery-samples.wikipedia_benchmark.Wiki10B`
WHERE
  title LIKE '%Google%'
GROUP BY
  language,
  title
ORDER BY
  views DESC
```

Query results には、タブが 4 つあって、結果を表示しているのは Results タブ\
Execution details タブを見ると、Elasped time（現実にかかった時間）と\
Slot time consumed（全体でかかった時間）がわかる

SQL はケースセンシティブなので、もし大文字小文字含めて検索したいならば\
`WHERE UPPER(title) LIKE '%GOOGLE%`とするべし

### Getting Started 9min

BigQuery organizes data tables into units called datasets\
BigQuery datasets belong to a project\
Access control to run a query is via Cloud IAM\
BigQuery datasets can be regional or multi-regional\
The table schema provides structure to the data

You can separate cost of storage and cost of queries\
Project A pays for queries against data stored in project B\
Project B pays for storage and queries from Prject B

With the BigQuery Data Transfer Service, you can copy large datasets\
from different projects to yours in seconds

### Loading Data 11min

Batch load supports different file formats\
CSV, NEWLINE_DELIMITED_JSON, AVRO, DATASTORE_BACKUP, PARQUET, ORC

DTS (Data Transfer Service) provides SaaS connectors\
(Cloud Storage, S3, YouTube, Google Ads, Salesforce)

Modify table data with standard DML statements\
INSERT,UPDATE,DELETE,MERGE records into tables

```sql
UPDATE table_A
SET
  y = table_B.y,
  z = table_B.z +1
FROM
  table_B
WHERE table_A.x = table_B.x
  AND table_A.y IS NULL;

INSERT INTO table VALUES (1,2,3),(4,5,6),(7,8,9);

DELETE FROM table WHERE TRUE;
```

Note: You are limited to 1,000 DML updates per table per day.

[データ定義言語ステートメントの使用  \|  BigQuery  \|  Google Cloud](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language?hl=ja)

- CREATE TABLE ... 新しいテーブルを作成します
- CREATE TABLE IF NOT EXISTS ... 指定されたデータセットにテーブルが存在しない場合にのみ新しいテーブルを作成します
- CREATE OR REPLACE TABLE ... テーブルを作成し、既存のテーブルを指定されたデータセット内の同じ名前に置き換えます

BigQueryは外部のMySQLに接続できる\
左枠 ＞ Recources ＞ +ADD DATA ＞ Create connection\
BigQueryとエクスターナルデータの間のJOINも可能

```sql
FROM
  EXTERNAL_QUERY('bigquery-federation-text.vs.demo_mysql_connection',
  'SELECT customer_id, SUM(amount) as current_spend FROM orders GROPU BY customer_id') AS extable
```

UDF = User Defined Functions

[bigquery\-utils/udfs/community at master · GoogleCloudPlatform/bigquery\-utils](https://github.com/GoogleCloudPlatform/bigquery-utils/tree/master/udfs/community)

### Lab Loading Data into BigQuery

ラボの目的

- 各種ソースからBigQueryにデータをロードする
- CLIとコンソールを使ってBigQueryにデータをロードする
- DDLを使って、テーブルを作成する

DDL (Data Definition Language) = データ定義言語\
create文、drop文、alter文がDDLに相当する

DML (Data Manipulation Language) = データ操作言語\
select文、insert文、update文、delete文がDMLに相当する

DCL (Data Control Language) = データ制御言語\
grant文、revoke文がDCLに相当する

ラボのタイトルは「Loading Data into BigQuery」

#### Create a new dataset to store our tables

GCP Console ＞ BIG DATA > BigQuery\
Left pane > project name > Right pane > Click "+ CREATE DATASET"\
Input "nyctaxi" as Dataset ID > Click "Create dataset" button

#### Ingest a new Dataset from a CSV

```sql
#standardSQL
SELECT
  *
FROM
  nyctaxi.2018trips
ORDER BY
  fare_amount DESC
LIMIT  5
```

#### Ingest a new Dataset from Google Cloud Storage

```bash
bq load \
--source_format=CSV \
--autodetect \
--noreplace  \
nyctaxi.2018trips \
gs://cloud-training/OCBL013/nyc_tlc_yellow_trips_2018_subset_2.csv
```

#### Create tables from other tables with DDL

```sql
#standardSQL
CREATE TABLE
  nyctaxi.january_trips AS
SELECT
  *
FROM
  nyctaxi.2018trips
WHERE
  EXTRACT(Month
  FROM
    pickup_datetime)=1;
```

----
以下はちょっと先のラボの話

ラボのタイトルは「Working with JSON and Array data in BigQuery」

#### Create a new dataset to store our tables

GCP Console ＞ BIG DATA > BigQuery\
Left pane > project name > Right pane > Click "+ CREATE DATASET"\
Input "fruit_store" as Dataset ID > Click "Create dataset" button

#### Practice working with Array in SQL

配列に慣れよ

```sql
SELECT
  ['raspberry',
  'blackberry',
  'strawberry',
  'cherry'] AS fruit_array
```

Query resultsをJSONで表示させることもできる

```json
[
  {
    "fruit_array": [
      "raspberry",
      "blackberry",
      "strawberry",
      "cherry"
    ]
  }
]
```

テーブルを作成したときに、ModeがREPEATEDになったものは、Arrayを意味する

Recap

- BigQuery natively supports arrays
- Array values must share a data type
- Arrays are called REPEATED fields in BigQuery

### Creating your own arrays with ARRAY_AGG()

変更前

```sql
SELECT
  fullVisitorId,
  date,
  v2ProductName,
  pageTitle
  FROM `data-to-insights.ecommerce.all_sessions`
WHERE visitId = 1501570398
ORDER BY date
```

変更後

```sql
SELECT
  fullVisitorId,
  date,
  ARRAY_AGG(v2ProductName) AS products_viewed,
  ARRAY_AGG(pageTitle) AS pages_viewed
  FROM `data-to-insights.ecommerce.all_sessions`
WHERE visitId = 1501570398
GROUP BY fullVisitorId, date
ORDER BY date
```

ARRAY_LENGTH()を使ったクエリー

```sql
SELECT
  fullVisitorId,
  date,
  ARRAY_AGG(v2ProductName) AS products_viewed,
  ARRAY_LENGTH(ARRAY_AGG(v2ProductName)) AS num_products_viewed,
  ARRAY_AGG(pageTitle) AS pages_viewed,
  ARRAY_LENGTH(ARRAY_AGG(pageTitle)) AS num_pages_viewed
  FROM `data-to-insights.ecommerce.all_sessions`
WHERE visitId = 1501570398
GROUP BY fullVisitorId, date
ORDER BY date
```

DISTINCTを入れる

```sql
SELECT
  fullVisitorId,
  date,
  ARRAY_AGG(DISTINCT v2ProductName) AS products_viewed,
  ARRAY_LENGTH(ARRAY_AGG(DISTINCT v2ProductName)) AS distinct_products_viewed,
  ARRAY_AGG(DISTINCT pageTitle) AS pages_viewed,
  ARRAY_LENGTH(ARRAY_AGG(DISTINCT pageTitle)) AS distinct_pages_viewed
  FROM `data-to-insights.ecommerce.all_sessions`
WHERE visitId = 1501570398
GROUP BY fullVisitorId, date
ORDER BY date
```

Recap

- `ARRAY_LENGTH(<array>)` ... 要素の数を表示する
- `ARRAY_AGG(DISTINCT <field>)` ... 重複を排除して要素を表示する
- `ARRAY_AGG(<field> ORDER BY <field>)` ... 順番に表示する
- `ARRAY_AGG(<field> LIMI 5)` ... 表示を制限する

de-duplication 重複排除

### Querying datasets that already have ARRAYs

```sql
SELECT
  *
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_20170801`
WHERE visitId = 1501570398
```

エラーが起こるクエリー

```sql
SELECT
  visitId,
  hits.page.pageTitle
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_20170801`
WHERE visitId = 1501570398
```

=> `Cannot access field page on a value with type ARRAY<STRUCT<hitNumber INT64, time INT64, hour INT64, ...>> at [3:8]`

正しく動作するクエリー

```sql
SELECT
  visitId,
  h.page.pageTitle
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_20170801`,
  UNNEST(hits) AS h
WHERE
  visitId = 1501570398
LIMIT
  10
```

Recap

- 配列を`UNNEST()`することで、配列要素を行に戻す
- `UNNEST()`は、FROM句の中で使う

#### Introduction to STRUCTs

STRUCTはジョイン済みの別テーブル

- STRUCTは、TypeがRECORDになる
- ARRAYは、ModeがREPEATEDになる
- STRUCTを使うといは、SELECT文の中でCROSS JOINさせる

[標準 SQL での配列の操作  \|  BigQuery  \|  Google Cloud](https://cloud.google.com/bigquery/docs/reference/standard-sql/arrays#flattening-arrays)
