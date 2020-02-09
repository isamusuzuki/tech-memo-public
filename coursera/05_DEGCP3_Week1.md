# DEGCP3 Week1

作成日 2020/02/01、更新日 2020/02/09

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

### Lab: Running Apache Spark jobs on Cloud Dataproc - 1h30

#### Objectives

- Migrating existing Spark jobs to Cloud Dataproc
- Modify Spark jobs to use Cloud Storage instead of HDFS
- Optimize Spark jobs to run on Job specific clusters

#### Lab setup

```bash
gcloud auth list
gcloud config list project
```

#### Part1: Lift and Shift

GCP Consle > Left pane > BIG DATA > Dataproc > Clusters\
Click "Create Cluster" Button\
Name: sparktodp\
Component gateway: Enalble "access to the web interface ..."\
Advanced Options > Change Image > Select "1.4"\
Optional components > Click "Select component"\
Select "Anaconda" and "Jupyter Notebook"\
Click "Select" to close dialog\
Click "Create"

in Cloud Shell

```bash
git -C ~ clone https://github.com/GoogleCloudPlatform/training-data-analyst

export DP_STORAGE="gs://$(gcloud dataproc clusters describe sparktodp --region=us-central1 --format=json | jq -r '.config.configBucket')"

gsutil -m cp ~/training-data-analyst/quests/sparktobq/*.ipynb $DP_STORAGE/notebooks/jupyter
```

Login to the Jupyter Notebook

Dataproc Cluster page > click your cluster > Cluster detail page\
Click "Web Interfaces" > Click the jupyter link\
Click "01_spark.ipynb" to open\
Click Cell and run all

```python
# In [1]
!wget http://kdd.ics.uci.edu/databases/kddcup99/kddcup.data_10_percent.gz

# In [2]
!hadoop fs -put kddcup* /

# In [3]
!hadoop fs -ls /
```

Reading in data

```python
# In[4]
from pyspark.sql import SparkSession, SQLContext, Row

spark = SparkSession.builder.appName("kdd").getOrCreate()
sc = spark.sparkContext
data_file = "hdfs:///kddcup.data_10_percent.gz"
raw_rdd = sc.textFile(data_file).cache()
raw_rdd.take(5)

# In[5]
csv_rdd = raw_rdd.map(lambda row: row.split(","))
parsed_rdd = csv_rdd.map(lambda r: Row(
    duration=int(r[0]),
    protocol_type=r[1],
    service=r[2],
    flag=r[3],
    src_bytes=int(r[4]),
    dst_bytes=int(r[5]),
    wrong_fragment=int(r[7]),
    urgent=int(r[8]),
    hot=int(r[9]),
    num_failed_logins=int(r[10]),
    num_compromised=int(r[12]),
    su_attempted=r[14],
    num_root=int(r[15]),
    num_file_creations=int(r[16]),
    label=r[-1]
    )
)
parsed_rdd.take(5)
```

Spark analysis

```python
# In[6]
# One Way to analyze data in Spark is to call methods on a dataframe.
sqlContext = SQLContext(sc)
df = sqlContext.createDataFrame(parsed_rdd)
connections_by_protocol = df.groupBy('protocol_type').count().orderBy('count', ascending=False)
connections_by_protocol.show()

# In[7]
# Another way is to use Spark SQL
df.registerTempTable("connections")
attack_stats = sqlContext.sql("""
    SELECT
      protocol_type,
      CASE label
        WHEN 'normal.' THEN 'no attack'
        ELSE 'attack'
      END AS state,
      COUNT(*) as total_freq,
      ROUND(AVG(src_bytes), 2) as mean_src_bytes,
      ROUND(AVG(dst_bytes), 2) as mean_dst_bytes,
      ROUND(AVG(duration), 2) as mean_duration,
      SUM(num_failed_logins) as total_failed_logins,
      SUM(num_compromised) as total_compromised,
      SUM(num_file_creations) as total_file_creations,
      SUM(su_attempted) as total_root_attempts,
      SUM(num_root) as total_root_acceses
    FROM connections
    GROUP BY protocol_type, state
    ORDER BY 3 DESC
    """)
attack_stats.show()

# In[8]
%matplotlib inline
ax = attack_stats.toPandas().plot.bar(
  x='protocol_type', subplots=True, figsize=(10,25))
```

#### Part2: Separate Compute and Storage

In Cloud Shell

```bash
export PROJECT_ID=$(gcloud info --format='value(config.project)')
gsutil mb gs://$PROJECT_ID

wget http://kdd.ics.uci.edu/databases/kddcup99/kddcup.data_10_percent.gz
gsutil cp kddcup.data_10_percent.gz gs://$PROJECT_ID/
```

switch back to `01_spark` jupyter notebook tab\
Click File menu and Make a copy\
Change title `01_spark-Copy1` to `De-couple-storage`\
Save/Checkpoint and Close/Halt `01_spark`

switch back to `De-couple-storage` jupyter notebook tab\
Delete first three code cells\

```python
# Replace In[4]
from pyspark.sql import SparkSession, SQLContext, Row

gcs_bucket='[Your-Bucket-Name]'
spark = SparkSession.builder.appName("kdd").getOrCreate()
sc = spark.sparkContext
data_file = "gs://"+gcs_bucket+"//kddcup.data_10_percent.gz"
raw_rdd = sc.textFile(data_file).cache()
raw_rdd.take(5)
```

#### Part3: Deploy Spark Jobs

> You now create a standalone Python file, \
> that can be deployed as a Cloud Dataproc Job,\
> that will perform the same functions as this notebook. 

Click File menu and Make a copy\
Change title `De-couple-storage-Copy1` to `PySpark-analysis-file`\
Save/Checkpoint and Close/Halt `De-couple-storage`

switch back to `PySpark-analysis-file` jupyter notebook tab\
Click first cell and click Insert menu > Insert Cell Above

```python
%%writefile spark_analysis.py

import matplotlib
matplotlib.use('agg')

import argparse
parser = argparse.ArgumentParser()
parser.add_argument("--bucket", help="bucket for input and output")
args = parser.parse_args()

BUCKET = args.bucket
```

For the remaining cells insert `%%writefile -a spark_analysis.py` at the start of each code cell.

この先の作業をまとめる\
要所要所をちょこちょこと変更する（変数、Matplotlibの吐き出し場所など）\
コードセルをひとつのファイルにまとめていき、そのコードをテストで走らせる\
うまくいったらそのPythonコードをCloud Storageに保存する\
Cloud StorageからさらにCloud Shellにコピー\
gcloudコマンドを使って、cloudprocのジョブとして登録する\
ジョブは登録されるとすぐに動作するので、少しの間待つ\
完了したら、その結果を確認する
