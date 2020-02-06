# DEGCP3 Week1

作成日 2020/02/01、更新日 2020/02/06

## 00. コース概要

タイトル: Building Batch Data Pipelines on GCP\
位置づけ: Part of a 6-course series, Data Engineering with GCP

### Building batch data pipelines

1. EL, ETL, ELT
1. Executing Spark on Dataproc
1. Levaraging GCP in pipelines
1. Serverless data processing with Dataflow
1. Summary

## 01. Introduction to Batch Data Pipelines

### EL, ELT, ETL - 4min

EL Architecture

- Extract data from files on Google Cloud Storage
- Load it into BigQuery's native storage
- You can trigger this from Cloud Composer, Cloud Functions, or scheduled queries

When would you use EL?

- Batch load of historical data
- Scheduled periodic loads of log files (e.g. once a day)
- But only if the data are already clean and correct!

ELT Architechture

- Extract data from files in Google Cloud Storage into BigQuery
- Transform the data on the fly using BigQuery views, or store into the new tables

When would you use ELT?

- Experimental datasets where you are not yet sure what kinds of transformations are needed to make the data usable
- Any production dataset where the transformation can be expressed in SQL

### Quality considerations - 1min

BigQuery can fix many data quality issues using SQL and Views

### How to carry out operations in BigQuery - 3min

1. Validity ... Filter to identify and isolate valid data

- `WHERE(constion)` ... Filter rows
- `HAVING(conditions)` ... Filter aggregations
- `WHERE field IS NOT NULL` ... Filters NULLs but leave blanks
- `WHERE field IS NOT NULL AND field <> ""` ... Filters NULLs and blanks

2. Consistency .. Detect duplication, enforce uniqueness for consistency

- `COUNT(DISTINCT field)`, `COUNT(field)` ... A difference means duplications
- `COUNT(field) GROUP BY (field)` ... more than 1 indicates duplications

3. Accuracy ... Test data against known good values for accuracy

- `JOIN`, `(quantity_ordered * item_price) as sub_total`

4. Completeness ... Identify and fill in mssing values for completeness

- `NULLIF()`, `IFNULL()`, `COALESCE()` 

5. Uniformity ... Make data types and formats explicit for uniformity

- `FORMAT()`, `CAST()`

### Shortcomings - 3min

What if the transformations cannot be expressed in SQL? Or are too complex to do in SQL?

ETL Architecture

- Extract data from Pub/Sub, Google Cloud Storage, Cloud Spanner, Cloud SQL, etc.
- Transform the tata using Cloud Dataflow
- Have Dataflow pipeline write to BigQuery

When would you use ETL?

- When the raw data needs to be quality-controlled, transformed, or enriched before being loaded into BigQuery.
- When the data loading has to happen continuously
- When you want to integrate with CI/CD systems and perform unit testing on all components

### ETL to solve data quality issues - 4min

- Dataflow to BigQuery ... Latency, throughput
- Cloud Dataproc ... Reusing Spark pipelines
- Cloud Data Fusion ... Need for visual pipline building

Cloud Dataproc is a managed service for batch processing, querying, streamin, and ML

Cloud Data Fusion is a fully-managed, cloud native, enterprice data instegration service for quickly building and managing data pipelines

Tracking lineage in ETL pipelines can be important

Labels on datasets, tables, and views can help track lineage

Data Catalog is a fully managed and highly scalable data discovery and metadata management service

## 02. Executing Spark on Cloud Dataproc

### The Hadoop ecosystem - 8min

The Hadoop ecosystem developed because of a need to analyze large datasets

Apache Spark is a popular, flexible, powerful way to process large datasets

Turning and optimizing the on-prem cluster can be time-consuming and difficult

On-premises Hadoop clusters have a number of limitations

- Not elastic
- Hard to scale fast
- Have capacity limit
- Have no separation between storage and compute resources

Cloud Dataproc simplifies Hadoop workloads on GCP

- Built-in support for Hadoop
- Managed hardware and configuration
- Simplified version management
- Flexible job configuration

### Running Hadoop on Cloud Dataproc - 10min

Cloud Dataproc is a managed service for running Hadoop and Spark data processing workloads

Use initialization actions to add other software to cluster at startup

```bash
gcloud dataproc clusters create <CLUSTER_NAME> \
  --initialization-actions gs://$MY_BUCKET/hbase/hbase.sh \
  --num-masters 3 --num-workers 2
```

Dataproc cluster can read/write to GCP storage products

- HDFS connector (`htfs://`) ... Cloud Storage (`gs://`)
- HBase connector ... Cloud Bigtable
- BigQuery connector ... BigQuery

### GCS instead of HDFS - 5min

HDFS in the Cloud is a sub-par solution

- Compute and storage are not independnet, adding to costs
- If you use persistent disks, then data locality no longer holds
- Lost of data replication makes this expensive

Petabit bandwidth is a game-changer for big data

Separation of computer and storage enables better options

Cloud storage is a drop-in replacement for HDFS

### Optimizing Dataproc - 5min

Hadoop and Spark performance questions for all cluster architectures, Cloud Dataproc included

### Optimizing Dataproc Storage - 9min

- Persistent clusters are expensive
- Your open-source-based tools may be insufficient
- Persistent clusters are difficult to manager

Cluster Scheduled Deletion ... Min 10 min, Max 14 days,  Granularity 1 second

Persistent clusters <=> Ephemeral（儚い） clusters

### Optimizing Dataproc Templates and Autoscaling - 4min

```bash
gcloud dataproc workflow-templates

gcloud dataproc workflow-templates add-job
```

### Optimizing Dataproc Monitoring - 3min

Use Stackdriver logging and performance monitoring
