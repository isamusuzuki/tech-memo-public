# DEGCP2 Week1

作成日 2020/01/26、更新日 2020/01/27

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
