# DEGCP2 Week1

作成日 2020/01/19、更新日 2020/01/25

## 00. コース概要

タイトル: Modernizing Data Lakes and Data Warehouses with GCP\
位置づけ: Part of a 6-course series, Data Engineering with GCP

## 01. Introduction to Data Engineering

### Explore the role of a data engineer 6min

A data engineer builds data pipelines to enable data-driven decisions

A data lake brings together data from across the enterprise into a single location

Cloud Storage is designed for eleven 9 annual durability\
Quick create buckets with cloud shell\
`gsutil mg gs://your-project-name`

ETL (Extract, Transform, Load) with Cloud DataProc and Cloud Dataflow

Real-time analytics with Cloud Pub/Sub, Cloud Dataflow and\
stream into BigQuery

### Analyze data engineering challenges 8min

Data is often siled in many upstream source systems

### Intro to BigQuery 3min

Blank

### Data Lakes and Data Warehouses 5min

```mermaid
graph LR;
A[Raw Data]-->|Replicate|B(Data Lake)-->|ETL,Pipeline|C(Data Warehouse)
```

BigQuery is a modern data warehouse that changes the conventional mode of data warehouseing

- Automate data delivery
- Make insights accessible
- Build the foundation for AI
- Tee up real-time Insights
- Fully managed
- Simplify dta operations

### Demo: Federated Queries with BigQuery 6min

[GoogleCloudPlatform/training\-data\-analyst: Labs and demos for courses for GCP Training \(http://cloud\.google\.com/training\)\.](https://github.com/GoogleCloudPlatform/training-data-analyst)

[training\-data\-analyst/simple_external_data_query\.md at master · GoogleCloudPlatform/training\-data\-analyst](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/data-engineering/demos/simple_external_data_query.md)

> An external data source is a data source that you can query directly even though the data is not stored in BigQuery. Instead of loading or streaming the data, you create a table that references the external data source.

[外部データソースの概要  \|  BigQuery  \|  Google Cloud](https://cloud.google.com/bigquery/external-data-sources)

> 外部データソースは、データが BigQuery に格納されていない場合でも直接クエリできるデータソースです。データの読み込みまたはストリーミングの代わりに、外部データソースを参照するテーブルを作成します。\
> BigQuery では、次のデータに対して直接クエリを実行できます。
>
> - Cloud Bigtable
> - Cloud Storage
> - Google Drive

BigQuery は、Google Spreadsheet を外部データソースとして使える。\
いったんテーブルを作成した後は、更新作業は不要で、クエリーを投げるごとに新しいデータが使われる

### Transactional Databases vs Data Warehouses 4min

Cloud SQL is fully managed SQL Server, Postgres, or MySQL for your Relational Database (transactional RDBMS)

RDBMS are optimized for data from a signle source and high-throughput writes vs. high-read data warehouses

### Partner effectively with other data teams 6min

- Machine Learning Engineer needs to capture new features in a stable pipeline
- Data Analyst relys on showcase the latest insights
- Other Data Engineer relys on timely and error free pipline

### Manage data access and governance 2min

Cloud Data Catalog is guarding PII

PII = Personally Identifiable Information

DLP API = Data Loss Prevention API

### Demo: Finding PII in your dataset with DLP API

training-data analyst / courses / data-engineering / demos / cloud_dlp.md

[training\-data\-analyst/cloud_dlp\.md at master · GoogleCloudPlatform/training\-data\-analyst](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/data-engineering/demos/cloud_dlp.md)

[Data Loss Prevention Demo](https://cloud.google.com/dlp/demo/#!/)

### Build production-ready pipelines 2min

Cloud Composer (managed Apache Airflow) is used to orchestrate production workflows

Cloud Composer は、GCP コンソールのビッグデータの一番上にある。API を有効にしてからでないと使えない

[Google Cloud Composer  \|  Google Cloud](https://cloud.google.com/composer/?hl=ja)

- Apache Airflow で構成されている
- Python を採用
- フルマネージド
- 有向非巡回グラフ (DAG) = Workflow

[Apache Airflow](https://airflow.apache.org/)

[Integration — Airflow Documentation](https://airflow.apache.org/docs/stable/integration.html)

### Review GCP customer case study 3min

Blank

### Recap 1min

[gregsramblings/google\-cloud\-4\-words: The Google Cloud Developer's Cheat Sheet](https://github.com/gregsramblings/google-cloud-4-words)

### Lab: Using BigQuery to do Analysis

analyze 2 different public datasets

- weather data from the NOAA
- bicycle rental data from New York City

#### Explore bicycle rental data

GCP Consle > BIG DATA > BigQuery > Left pane > "+ADD DATA"\

> Explore public datasets > find 'NYC bike'\
> bigguery-public-data > new_york_citibike > citbike_trips > Preview

```sql
SELECT
  MIN(start_station_name) AS start_station_name,
  MIN(end_station_name) AS end_station_name,
  APPROX_QUANTILES(tripduration, 10)[OFFSET (5)] AS typical_duration,
  COUNT(tripduration) AS num_trips
FROM
  `bigquery-public-data.new_york_citibike.citibike_trips`
WHERE
  start_station_id != end_station_id
GROUP BY
  start_station_id,
  end_station_id
ORDER BY
  num_trips DESC
LIMIT
  10
```

```sql
WITH
  trip_distance AS (
SELECT
  bikeid,
  ST_Distance(ST_GeogPoint(s.longitude,
      s.latitude),
    ST_GeogPoint(e.longitude,
      e.latitude)) AS distance
FROM
  `bigquery-public-data.new_york_citibike.citibike_trips`,
  `bigquery-public-data.new_york_citibike.citibike_stations` as s,
  `bigquery-public-data.new_york_citibike.citibike_stations` as e
WHERE
  start_station_id = s.station_id
  AND end_station_id = e.station_id )
SELECT
  bikeid,
  SUM(distance)/1000 AS total_distance
FROM
  trip_distance
GROUP BY
  bikeid
ORDER BY
  total_distance DESC
LIMIT
  5
```

#### Explore the weather dataset

bigguery-public-data > ghcn_d > ghcnd_2015

rainfall (in mm) for all days in 2015 from a weather station in New York

```sql
SELECT
  wx.date,
  wx.value/10.0 AS prcp
FROM
  `bigquery-public-data.ghcn_d.ghcnd_2015` AS wx
WHERE
  id = 'USW00094728'
  AND qflag IS NULL
  AND element = 'PRCP'
ORDER BY
  wx.date
```

#### Find colrelation between rain and bicycle rentals

```sql
WITH bicycle_rentals AS (
  SELECT
    COUNT(starttime) as num_trips,
    EXTRACT(DATE from starttime) as trip_date
  FROM `bigquery-public-data.new_york_citibike.citibike_trips`
  GROUP BY trip_date
),

rainy_days AS
(
SELECT
  date,
  (MAX(prcp) > 5) AS rainy
FROM (
  SELECT
    wx.date AS date,
    IF (wx.element = 'PRCP', wx.value/10, NULL) AS prcp
  FROM
    `bigquery-public-data.ghcn_d.ghcnd_2015` AS wx
  WHERE
    wx.id = 'USW00094728'
)
GROUP BY
  date
)

SELECT
  ROUND(AVG(bk.num_trips)) AS num_trips,
  wx.rainy
FROM bicycle_rentals AS bk
JOIN rainy_days AS wx
ON wx.date = bk.trip_date
GROUP BY wx.rainy
```

New Yorkers ride the bicycle 47% fewer times when it rains.

## 02. Building a Data Lake

- Learn how to use Google Cloud Storage as you data lake on GCP
- Use Cloud SQL for a relational data lake

### Introduction to Data Lakes 10min

Data Lake vs. Data Warehouse

### Data Storage and ETL options on GCP 4min

- EL ... Extract and Load for mini-size
- ELT ... Extract, Load, and Transform for mid-size
- ETL ... Extract, Transform, and Load for large-size

### Building a Data Lake using Cloud Storage 10min

Bucket properties depend on your requirements

- Location
- Frequency of Access (Regional/Multi-Regional, Newline, Coldline)
- High Availablity (single-region, multi-region, dual-region)

[バケットのロケーション  \|  Cloud Storage  \|  Google Cloud](https://cloud.google.com/storage/docs/locations)

[ストレージ クラス  \|  Cloud Storage  \|  Google Cloud](https://cloud.google.com/storage/docs/storage-classes)

Cloud Storage simulates a file system

### Demo: Optimizing cost with Google Cloud Sotrage classes and Cloud Functions 7min

Blank

### Securing Cloud Storage 5min

Data encryption options

- GMEK ... Google-managed encryption keys
- CMEK ... Customer-managed encyption keys
- CSEK ... Customer-supplied encryption keys

### Storing All Sorts of Data Types 5min

Transactional vs. Analytical

Transactional systems are 80% writes and 20% reads

Data engineers build the pipeliness between the sytems

### Demo: Running federated queries on Parquet and ORC files in BigQuery 4min

Google Cloud Storage にあがっている、3.5 テラバイトの、Parquet 形式の、Web クロールデータを BigQuery に移行する。サブディレクトリが "key=value" の構造になっている

BigQuery managed storage という新機能は。データをコピーしつつ、きちっと列をチェックしている

### Storing Relational Data in the Cloud 1min

Cloud Spanner は分散 RDB。地理的に離れた場所で使う時やギガバイトを超える量があるときはこちらを選択

分析ワークロードには、BigQuery が一番だが、1 秒間で百万行のようなハイスループットのインサートや、ミリセカンドの低遅延が求められるときは、BigTable を選択

### Cloud SQL as a relational Data Lake

OLTP = Online Transaction Processing workloads

Advantages of Cloud SQL

- Google grade security
- Managed backups（各インスタンスが 7 バックアップを保持する）
- Vertical scaling (read and write)
- Horizontal scaling (read, suports 3 replica scinarios)
- Automatic replication in different zone (failover replica)

### Lab: Loading Taxi Data into Cloud SQL 27sec

Objectives

- Create Cloud SQL instance
- Create a Cloud SQL database
- Import text data into Cloud SQL
- Check the data for integrity

Cloud Shell を使う

```bash
gcloud auth list # => credential Accountsを表示
gcloud config list project # => Project名を表示

export P_ID=$(gcloud info --format='value(config.project)')
echo $P_ID

export BUCKET=${P_ID}-ml
echo $BUCKET

# Create a Cloud SQL instance
gcloud sql instances create taxi --tier=db-n1-standard-1 --activation-policy=ALWAYS

# Set a root password
gcloud sql users set-password root --host % --instance taxi --password Passw0rd

# 自分のIPアドレスを取得
export ADDRESS=$(wget -qO - http://ipecho.net/plain)/32

# SQLインスタンスのホワイトリストに自分を登録
gcloud sql instances patch taxi --authorized-networks $ADDRESS

# SQLインスタンスのIPアドレスを取得する
MYSQLIP=$(gcloud sql instances describe taxi --format='value(ipAddresses.ipAddress)')

# ログイン
mysql --host=$MYSQLIP --user=root --password
```

以下は mysql コマンドラインの中で行う

```sql
create database if not exists bts;

use bts;

drop table if exists trips;

create table trips (
  vendor_id VARCHAR(16),
  pickup_datetime DATETIME,
  dropoff_datetime DATETIME,
  passenger_count INT,
  trip_distance FLOAT,
  rate_code VARCHAR(16),
  store_and_fwd_flag VARCHAR(16),
  payment_type VARCHAR(16),
  fare_amount FLOAT,
  extra FLOAT,
  mta_tax FLOAT,
  tip_amount FLOAT,
  tolls_amount FLOAT,
  imp_surcharge FLOAT,
  total_amount FLOAT,
  pickup_location_id VARCHAR(16),
  dropoff_location_id VARCHAR(16)
);

describe trips;

select distinct(pickup_location_id) from trips;
```

Add data to Cloud SQl instance

```bash
gsutil cp gs://cloud-training/OCBL013/nyc_tlc_yellow_trips_2018_subset_1.csv trips.csv-1
gsutil cp gs://cloud-training/OCBL013/nyc_tlc_yellow_trips_2018_subset_2.csv trips.csv-2
# => 870KBのファイルが2つ、Cloud Shellのローカルにコピーされた

mysqlimport --local --host=$MYSQLIP --user=root --password --ignore-lines=1 --fields-terminated-by=',' bts trips.csv-*

# ログイン
mysql --host=$MYSQLIP --user=root --password
```
