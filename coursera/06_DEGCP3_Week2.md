# DEGCP3 Week1

作成日 2020/02/11、更新日 2020/02/11

## 01. Manage Data Pipelines with Cloud Data Fusion and Cloud Composer

### Cloud Data Fusion

#### Introduction - 7min

Cloud Data Fusion is a fully-managed, cloud native, enterprise data integration service for quickly building and managing data pipelines.\
Cloud Data Fusion uses Cloud Dataproc cluster to perform all transforms in the pipeline.

Data Fusion creates ephemeral execution environments to run pipelines

Build data pipelines with a friendly UI w/ 100+ plugins, 1000+ transforms

Extensible 拡張性が高い

#### Components of Data Fusion - 2min

Cloud Data Fusion - UI Overview

- Control Center ... ダッシュボードみたいな画面
  - Application
  - Artifact
  - Dataset
- Pipelines ... フロー設計みたいな画面
  - Developer Studio
  - Preview
  - Export
  - Schedule
  - Connector and function pallete
  - Navigation
- Wrangler ... スプレッドシートみたいな画面
  - Connections
  - Transforms
  - Data Quality
  - Insights
  - Functions
- Metadata ... テキストエディタみたいな画面
  - Search
  - Tags & Properties
  - Lineage - Field and Data
- Hub　... アプリストアみたいな画面
  - Plugins
  - Use cases
  - Pre-build pipelines
- Entities ... ダッシュボードみたいな画面
  - Pipeline
  - Application
  - Plugin
  - Driver
  - Library
  - Directive
- Administration
  - Management ... ダッシュボードみたいな画面
  - Configuration ... 設定画面みたいな画面

主に使うところは、PipelineとWranglerの2つである

#### Building a Pipeline - 6min

Data Pipline = DAG (Directed Acyclic Graph)

Studio is the UI where you create new pipelines

Use Preview and Moniter

You can schedule batch pipelines

#### Exploring Data using Wrangler - 1min

Data analysts can explore datasets in the Wrangler

Connection, Live browsing, transform

#### Lab: Building and Executing a Pipeline Graph in Data Fusion

ラボの目的

- Cloud Data Fusionをいくつかのデータソースに接続する
- 基本的な変換を適用する
- ふたつのデータソースを結びつける
- データをシンクに書き込む

Task1: Creating a Cloud Data Fusion instance

Enable Cloud Data Fusion API first\
GCP Console > Data Fusion\
Click "Create an Instance"\
Copy Service Account\
GCP Console > IAM\
Add service account and grant "service agent" role

Task2: Loading the data

```bash
export BUCKET=$GOOGLE_CLOUD_PROJECT
gsutil mb gs://$BUCKET
gsutil cp gs://cloud-training/OCBL017/ny-taxi-2018-sample.csv gs://$BUCKET
gsutil mb gs://$BUCKET-temp
```

Task3: Cleaning the data

Task4: Creating the pipeline

Task5: Adding a data source

Task6: Joining tow sources

Task7: Storing the output to BigQuery

Task8: Deploying and running the pipeline

Task9: Viewing the results

**問題発生** Cloud Data Fusionのページにアクセスできない。ラボは後回しにすることにした

### Cloud Composer

#### Orchestrating work between GCP services with Cloud Composer - 1min

Cloud Composer orchestrates automatic workflows\
Cloud Composer is managed by Apache Airflow\
Use Apache Airflow DAGs to orchestrate GCP services

#### Apache Airflow Environment - 1min

Cloud Composer creates managed Apache Airflow environments\
Each Airflow environment has a separate webserver and folder\
in GCP for pipeline DAGs\
The DAGs folder is simply a GCS bucket where you will load\
your pipeline code

#### DAGs and Operators - 12min

Airflow workflows are written in Python\
The python file creates a DAG (Webページで見るとグラフが出来ている）\

```text
Name: GcsToBigQueryTriggered 

Items:
process-delimited-and-push
    |-->success-move-to-completion
    `-->failure-move-to-completion
``` 

Airflow uses operators in your DAG to orchestrate other GCP services\
Apache Airflow is open source and cross-platform for hybrid pipelines

Example: Common Machine Learning workflow DAG Operators

```python
# update training data
t1 = BigQueryOperator()
# BigQuery training data export to GCS
t2 = BigQueryToCloudStrorageOperator()
# AI Platform training job
t3 = MLEngineTrainingOperator()
# App Engine deploy new version
t4 = AppEngineVersionOperator()

# DAG dependencies
t2.set_upstream(t1)
t3.set_upstream(t2)
t4.set_upstream(t3)
```

Pythonの固定値をAirflowのマクロを使って書き換えることができる

```python
max_query_date = '2018-02-01'  # {{ macros.ds_add(ds, -7) }}
min_query_date = '2018-01-01'  # {{ macros.ds_add(ds, -1) }}
```

#### Workflow scheduling - 6min

Two scheduling options for Cloud Composer workflows:

- Periodic
- Event-driven

Use Cloud Functions to create an event-based push architecture\
Creating a Cloud Function GCS trigger and Airflow DAG

WebサーバーのURL、DAGの名前、CLIENT_ID, WEBSERVER_IDがあれば、\
Cloud FunctionsからAirflowのDAGをキックすることができる

#### Monitoring and Logging - 4min

WebサーバーのDAGsの一覧テーブルに、DAG Runsというカラムがある\
これで成功した数、失敗した数がわかる\
DAG Runsは、ブラウズメニューからも行けるようだ\
DAGの詳細ページには、Log by attemptsという最近のログを表示するコーナーがある\
Stackdriverからもログを見ることができる

#### Lab: An Introduction to Cloud Composer

Create Cloud Composer environment

Navigation menu > Composer\
Click "CREATE ENVIRONMENT"

```bash
export BUCKET=$GOOGLE_CLOUD_PROJECT
gsutil mb gs://$BUCKET
```

Airflow and core concepts

- DAG ... a collection of all the tasks you want to run
- Operator ... The description of a single task
- Task ... A parameterised instance of an Operator; a node in the DAG
- Task Instance ... A specific run of a task

Defining the workflow

`hadoop_tutorial.py`

```python
"""Example Airflow DAG that creates a Cloud Dataproc cluster, runs the Hadoop
wordcount example, and deletes the cluster.

This DAG relies on three Airflow variables
https://airflow.apache.org/concepts.html#variables
* gcp_project - Google Cloud Project to use for the Cloud Dataproc cluster.
* gce_zone - Google Compute Engine zone where Cloud Dataproc cluster should be
  created.
* gcs_bucket - Google Cloud Storage bucket to used as output for the Hadoop jobs from Dataproc.
  See https://cloud.google.com/storage/docs/creating-buckets for creating a
  bucket.
"""

import datetime
import os

from airflow import models
from airflow.contrib.operators import dataproc_operator
from airflow.utils import trigger_rule

# Output file for Cloud Dataproc job.
output_file = os.path.join(
    models.Variable.get('gcs_bucket'), 'wordcount',
    datetime.datetime.now().strftime('%Y%m%d-%H%M%S')) + os.sep
# Path to Hadoop wordcount example available on every Dataproc cluster.
WORDCOUNT_JAR = (
    'file:///usr/lib/hadoop-mapreduce/hadoop-mapreduce-examples.jar'
)
# Arguments to pass to Cloud Dataproc job.
wordcount_args = ['wordcount', 'gs://pub/shakespeare/rose.txt', output_file]

yesterday = datetime.datetime.combine(
    datetime.datetime.today() - datetime.timedelta(1),
    datetime.datetime.min.time())

default_dag_args = {
    # Setting start date as yesterday starts the DAG immediately when it is
    # detected in the Cloud Storage bucket.
    'start_date': yesterday,
    # To email on failure or retry set 'email' arg to your email and enable
    # emailing here.
    'email_on_failure': False,
    'email_on_retry': False,
    # If a task fails, retry it once after waiting at least 5 minutes
    'retries': 1,
    'retry_delay': datetime.timedelta(minutes=5),
    'project_id': models.Variable.get('gcp_project')
}

with models.DAG(
        'composer_sample_quickstart',
        # Continue to run DAG once per day
        schedule_interval=datetime.timedelta(days=1),
        default_args=default_dag_args) as dag:

    # Create a Cloud Dataproc cluster.
    create_dataproc_cluster = dataproc_operator.DataprocClusterCreateOperator(
        task_id='create_dataproc_cluster',
        # Give the cluster a unique name by appending the date scheduled.
        # See https://airflow.apache.org/code.html#default-variables
        cluster_name='quickstart-cluster-{{ ds_nodash }}',
        num_workers=2,
        zone=models.Variable.get('gce_zone'),
        master_machine_type='n1-standard-1',
        worker_machine_type='n1-standard-1')

    # Run the Hadoop wordcount example installed on the Cloud Dataproc cluster
    # master node.
    run_dataproc_hadoop = dataproc_operator.DataProcHadoopOperator(
        task_id='run_dataproc_hadoop',
        main_jar=WORDCOUNT_JAR,
        cluster_name='quickstart-cluster-{{ ds_nodash }}',
        arguments=wordcount_args)

    # Delete Cloud Dataproc cluster.
    delete_dataproc_cluster = dataproc_operator.DataprocClusterDeleteOperator(
        task_id='delete_dataproc_cluster',
        cluster_name='quickstart-cluster-{{ ds_nodash }}',
        # Setting trigger_rule to ALL_DONE causes the cluster to be deleted
        # even if the Dataproc job fails.
        trigger_rule=trigger_rule.TriggerRule.ALL_DONE)

    # Define DAG dependencies.
    create_dataproc_cluster >> run_dataproc_hadoop >> delete_dataproc_cluster
```

Viewing environment information

Using the Airflow UI

Setting Airflow variables

Airflow web interface > Admin > Variables > Click "Create"

Uploading the DAG to Cloud Storage

```bash
gsutil cp hadoop_tutorial.py <DAGs_folder_path>
```
DAGs folderは、Composerの一覧テーブルの名前をクリックすると表示される

実行開始日が昨日に設定されているので、ファイルをコピーするやいなや実行が開始される

DAG一覧テーブルの名前をクリックすると、DAG詳細ページが表示される\
Graph View ... タスクにマウスオーバーするとステータスが表示される\
タスクをクリックするとダイアログが表示され、直接操作できる

## 02. Serverless Data Processing with Cloud Dataflow
