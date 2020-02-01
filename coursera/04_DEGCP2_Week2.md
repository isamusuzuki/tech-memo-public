# DEGCP2 Week1

作成日 2020/01/26、更新日 2020/02/01

## 01. Building a data warehouse

Learning Objectives

-   Discuss requirements of a modern warehouse
-   Understand why BigQuery is the scalable data warehousing solution on GCP
-   Undersntad core concepts of BigQuery and review options of loading data into BigQuery

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

-   CREATE TABLE ... 新しいテーブルを作成します
-   CREATE TABLE IF NOT EXISTS ... 指定されたデータセットにテーブルが存在しない場合にのみ新しいテーブルを作成します
-   CREATE OR REPLACE TABLE ... テーブルを作成し、既存のテーブルを指定されたデータセット内の同じ名前に置き換えます

BigQuery は外部の MySQL に接続できる\
左枠 ＞ Recources ＞ +ADD DATA ＞ Create connection\
BigQuery とエクスターナルデータの間の JOIN も可能

```sql
FROM
  EXTERNAL_QUERY('bigquery-federation-text.vs.demo_mysql_connection',
  'SELECT customer_id, SUM(amount) as current_spend FROM orders GROPU BY customer_id') AS extable
```

UDF = User Defined Functions

[bigquery\-utils/udfs/community at master · GoogleCloudPlatform/bigquery\-utils](https://github.com/GoogleCloudPlatform/bigquery-utils/tree/master/udfs/community)

### Lab: Loading Data into BigQuery

ラボの目的

-   各種ソースから BigQuery にデータをロードする
-   CLI とコンソールを使って BigQuery にデータをロードする
-   DDL を使って、テーブルを作成する

DDL (Data Definition Language) = データ定義言語\
create 文、drop 文、alter 文が DDL に相当する

DML (Data Manipulation Language) = データ操作言語\
select 文、insert 文、update 文、delete 文が DML に相当する

DCL (Data Control Language) = データ制御言語\
grant 文、revoke 文が DCL に相当する

#### Create a new dataset to store our tables

GCP Console ＞ BIG DATA > BigQuery\
Left pane > project name > Right pane > Click "+ CREATE DATASET"\
Input "nyctaxi" as Dataset ID > Click "Create dataset" button

#### Ingest a new Dataset from a CSV

```sql
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

## 02. BigQuery as a data warehousing solution

### Exploring Schemas 24sec

Let's explore BigQuery Public Dataset schemas

[BigQuery public datasets  \|  Google Cloud](https://cloud.google.com/bigquery/public-data)

### Demo: Exploring Schemas 10min

[training\-data\-analyst/information_schema\.md at master · GoogleCloudPlatform/training\-data\-analyst](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/data-engineering/demos/information_schema.md)

`bigquery-public-data.baseball.__TABLES__` => これでテーブルのメタデータ（サイズ、更新日など）を得ることができる

`bigquery-public-data.basebasll.INFORMATION_SCHEMA.COLUMNS` => これでカラムについてのメタデータを得ることができる

### Schema Design 3min

Denormalize before loading into a data warehouse

Normalized data => Denormalized flattened data

Netsted and repeated columns improve the efficieny of BiqQuery\
with relational source data

### Nested and Repeated Fields 8min

Reporting Approach: Should we normalize of denormalize?

-   normalize => Many tables => JOINs are costly
-   denormalize => One big table => Data is repeated

Nested and Repeated Fields allow you to have\
multiple levels of data granularity

テーブルの Schema を見て、Type が RECORD であるならば、それは STRUCTS である\
RECORD に続くカラムは、Field name が、「Record と同じ名前.」で始まる\
STRUCTS は JOIN したテーブルそのものである\
ひとつのテーブルにいくつ STRUCTS があってもかまわない

テーブルの Schema を見て、Mode が REPEATED であるならば、それは ARRAYS である\
ARRAYS は、通常のフィールドかもしくは STRUCTS の一部となりうる

BigQuery stores repeated, nested fields in a columner format

### Demo: Nested and Repeated Fields 15min

[training\-data\-analyst/nested\.md at master · GoogleCloudPlatform/training\-data\-analyst](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/data-engineering/demos/nested.md)

```sql
SELECT
  block_id,
  MAX(i.input_sequence_number) AS max_seq_number,
  COUNT(t.transaction_id) as num_transactions_in_block
FROM `bigquery-public-data.bitcoin_blockchain.blocks` AS b
  -- Use the nested STRUCT within BLOCKS table for transactions instead of a separate JOIN
  , b.transactions AS t
  , t.inputs as i
GROUP BY block_id;
```

-   FROM 句の中の","は、"CROSS JOIN" の省略形である
-   Nested または Repeated されたフィールドを使うときは、自分に CROSS JOIN させてから使う

```sql
SELECT DISTINCT
  block_id,
  TIMESTAMP_MILLIS(timestamp) AS timestamp,
  t.transaction_id,
  t_outputs.output_satoshis AS satoshi_value,
  t_outputs.output_satoshis * 0.00000001 AS btc_value
FROM `bigquery-public-data.bitcoin_blockchain.blocks` AS b
  , b.transactions AS t
  , t.inputs as i
  , UNNEST(t.outputs) AS t_outputs
ORDER BY btc_value DESC
LIMIT 10;
```

-   配列となっているフィールドは、使う前に分解する必要がある
-   UNNEST()メソッドで囲ってから、自分に CROSS JOIN させる

#### 自習: ネストされた列と繰り返し列を、もう一度、おさらしておく

[ネストされた列と繰り返し列の指定  \|  BigQuery  \|  Google Cloud](https://cloud.google.com/bigquery/docs/nested-repeated?hl=ja#limitations)

BigQuery は、データを非正規化して、ネストされた列と繰り返し列を利用します。\
ネストされた列と繰り返し列は、リレーショナル（正規化）スキーマを維持することによる\
パフォーマンスへの影響なしで、リレーションシップを維持できます。\
ネストされた列と繰り返し列を指定するには、`RECORD` (`STRUCT`) データ型を使用します。

### Lab: Working with JSON and Array Data in BigQuery

目的

-   半構造の JSON データを BigQuery にロードする
-   Arrays を作成してクエリーする
-   Structs を作成してクエリーする
-   ネストされて繰り返しているフィールドをクエリーする

#### Create a new dataset to store our tables

GCP Console ＞ BIG DATA > BigQuery\
Left pane > Resources > Click project name\
Right pane > Click "+ CREATE DATASET"\
Input "fruit_store" as Dataset ID > Click "Create dataset" button

#### Practice working with Array in SQL

正規化 normalization = going from one table to many\
MySQL のようなデータベースではごく普通のアプローチ

非正規化 denormalization = bring many separate tables\
into one large table\
データアナリストが行う逆方向の作業

```sql
SELECT
  ['raspberry',
  'blackberry',
  'strawberry',
  'cherry'] AS fruit_array
```

Query results を JSON で表示させることもできる

```json
[
    {
        "fruit_array": ["raspberry", "blackberry", "strawberry", "cherry"]
    }
]
```

テーブルを作成したときに、Mode が REPEATED になったものは、Array を意味する

まとめ

-   BigQuery natively supports arrays
-   Array values must share a data type
-   Arrays are called REPEATED fields in BigQuery

### Creating your own arrays with ARRAY_AGG()

ARRAY_AGG()を使って、クエリー結果に配列をつくる

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

ARRAY_LENGTH()を使って配列の数を数える

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

DISTINCT を使って、重複をのぞいた配列をつくる

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

まとめ

-   `ARRAY_LENGTH(<array>)` ... 配列の数を数える
-   `ARRAY_AGG(DISTINCT <field>)` ... 重複を排除して配列をつくる
-   `ARRAY_AGG(<field> ORDER BY <field>)` ... 順番に並べて配列をつくる
-   `ARRAY_AGG(<field> LIMI 5)` ... 数を制限して配列をつくる

de-duplication = 重複排除

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

まとめ

-   配列を`UNNEST()`することで、配列要素を行に戻す
-   `UNNEST()`は、FROM 句の中で使う
-   `UNNEST()`の前の`,`が大事で、`CROSS JOIN`を意味している

#### Introduction to STRUCTs

STRUCT はジョイン済みの別テーブル

-   STRUCT は、Type が RECORD になる
-   ARRAY は、Mode が REPEATED になる
-   STRUCT を使うといは、SELECT 文の中で CROSS JOIN させる

[標準 SQL での配列の操作  \|  BigQuery  \|  Google Cloud](https://cloud.google.com/bigquery/docs/reference/standard-sql/arrays#flattening-arrays)

#### Practice with STRUCTS and ARRAYs

-   Type が RECORD の列は STRUCT
-   Mode が REPEATED の列は ARRAY

正しく動くクエリー

```sql
SELECT race, participants.name
FROM racing.race_results
CROSS JOIN
race_results.participants
```

よりスマートな書き方

```sql
SELECT race, participants.name
FROM racing.race_results AS r, r.participants
```

UNNEST を使ったクエリーの例

```sql
SELECT COUNT(p.name) AS racer_count
FROM racing.race_results as r,
UNNEST(r.participants) as p;

SELECT
  p.name,
  SUM(split_times) as total_race_time
FROM racing.race_results AS r
, UNNEST(r.participants) AS p
, UNNEST(p.splits) AS split_times
WHERE p.name LIKE 'R%'
GROUP BY p.name
ORDER BY total_race_time ASC;

SELECT
  p.name,
  split_time
FROM racing.race_results AS r
, r.participants AS p
, p.splits AS split_time
WHERE split_time = 23.2;
```

## 03. Partitioning and Clustering in BigQuery

### Optimizing with Partitioning and Clustering 5min

BigQuery supports three ways of partitioning tables

- Ingestion time
- Any column that is of type DATE or TIMESTAMP
- integer-typed column

BigQuery automatically sorts the data based on values in the clustering columns

### Demo: Creating Partitioned Tables 7min

[training\-data\-analyst/partition\.md at master · GoogleCloudPlatform/training\-data\-analyst](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/data-engineering/demos/partition.md)

### Demo: Partitioning and Clustering 6min

[training\-data\-analyst/clustering\.md at master · GoogleCloudPlatform/training\-data\-analyst](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/data-engineering/demos/clustering.md)

### Preview: Transforming Batch and Streaming Data 2min

endless streaming data => Cloud Pub/Sub, Cloud Dataflow and Cloud Bigtable

BigQueryにはstreaming insertsという方法がある

