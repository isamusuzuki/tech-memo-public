# DEGCP1 Week2 前半

作成日 2020/01/13、更新日 2020/01/18

## Create Streaming Data Pipelines with Cloud Pub/Sub and Cloud Dataflow

勉強する目的

- モダンなデータパイプラインが挑戦していること、そして、Cloud Dataflow でそれをどうやって解決しつつスケールしているかを知る
- Apache Beam を使って、ストリームパイプラインをデザインする
- Data Studio を使って、共有可能なリアルタイムダッシュボードを構築する

Video title: Real-time IoT Dashboard with Pub/Sub, Dataflow, and Data Studio

Apache Beam

- Works with both batch and streaming data
- SDK supports data transformations
- You can choose from templates
- Java, Python, and Go lang
- Runner or Engine => Cloud Dataflow

実際のコードの例

```java
Pipeline p = Pipelin.create();
p
    .apply(TextIO.Read.from("gs://..."))
    .apply(ParDo.of(new Filter1()))
    .apply(new Group1())
    .apply(ParDo.of(new Filter2()))
    .apply(new Transform1())
    .apply(TextIO.Write.to("gs://..."));

p.run();
```

Start with provided templates and build from there

[GoogleCloudPlatform/DataflowTemplates: Google\-provided Cloud Dataflow template pipelines for solving simple in\-Cloud data tasks](https://github.com/GoogleCloudPlatform/DataflowTemplates)

Requirements: Java 8, Maven 3

## Lab: Create a streaming data pipeline with Cloud Dataflow

### Setup streaming Taxi topic in Pub/Sub

Connect to a streaming data Topic in Cloud Pub/Sub

GCP Project ID: `qwiklabs-gcp-02-c704f556fe7e`

GCP Console > API & services > + ENABLE APIS AND SERVCICES

> Enable 'Cloud Pub/Sub API' and 'Dataflow API'

[NYC Open Data \-](https://opendata.cityofnewyork.us/)

Open cloudshell

```bash
bq mk taxirides

bq mk --time_partitioning_field timestamp --schema ride_id:string,point_idx:integer,latitude:float,longitude:float,timestamp:timestamp,meter_reading:float,meter_increment:float,ride_status:string,passenger_count:integer -t taxirides.realtime
```

create a cloud storage bucket with your GCP project id

### Create Dataflow job from template

Ingest streaming data with Cloud Dataflow

GCP Console > Dataflow > + CREATE JOB FROM TEMPLATE\
Job name: streaming-taxi-pipeline\
template: Cloud Pub/Sub topic to BigQuery\
Cloud Pub/Sub input topic: `projects/pubsub-public-data/topics/taxirides-realtime`\
BigQuery output table:`qwiklabs-gcp-02-c704f556fe7e:taxirides.realtime`\
Temporary Location: `gs://qwiklabs-gcp-02-c704f556fe7e/tmp/`\
Click Run job button

### Stream and monitor pipeline in BigQuery

Load streaming data into BigQuery

```sql
SELECT * FROM taxirides.realtime LIMIT 10
```

### Visualize key metrics in Data Studio

Analyze and visualize the results

```sql
WITH streaming_data AS (

SELECT
  timestamp,
  TIMESTAMP_TRUNC(timestamp, HOUR, 'UTC') AS hour,
  TIMESTAMP_TRUNC(timestamp, MINUTE, 'UTC') AS minute,
  TIMESTAMP_TRUNC(timestamp, SECOND, 'UTC') AS second,
  ride_id,
  latitude,
  longitude,
  meter_reading,
  ride_status,
  passenger_count
FROM
  taxirides.realtime
WHERE ride_status = 'dropoff'
ORDER BY timestamp DESC
LIMIT 100000

)

# calculate aggregations on stream for reporting:
SELECT
 ROW_NUMBER() OVER() AS dashboard_sort,
 minute,
 COUNT(DISTINCT ride_id) AS total_rides,
 SUM(meter_reading) AS total_revenue,
 SUM(passenger_count) AS total_passengers
FROM streaming_data
GROUP BY minute, timestamp
```

Click Explore with Data Studio

Chart type: column chart\
Data range dimension: dashboard_sort\
Dimension: dashboard_sort, minute\
Drill Down: dashboard_sort\
Metric: SUM() total_rides, SUM() total_passengers, SUM() total_revenue
Sort: dashboard_sort Ascending

Visualizing data at a minute-level granularity is currently not supported in Data Studio as a timestamp. This is why we created our own dashboard_sort dimension.\
分レベルの粒度は現在のところサポートされていない。それゆえ我々は dashboad_sort というディメンションを作成したのである

## Module Resources

[Cloud Pub/Sub ドキュメント  \|  Cloud Pub/Sub ドキュメント  \|  Google Cloud](https://cloud.google.com/pubsub/docs/)

[Cloud Dataflow ドキュメント  \|  Cloud Dataflow  \|  Google Cloud](https://cloud.google.com/dataflow/docs/)

[GitHub \- GoogleCloudPlatform/DataflowTemplates: Google\-provided Cloud Dataflow template pipelines for solving simple in\-Cloud data tasks](https://github.com/GoogleCloudPlatform/DataflowTemplates/)

[データスタジオ  \|  Google Developers](https://developers.google.com/datastudio/)

[Data Portal Report Gallery](https://datastudiogallery.appspot.com/gallery)

## Classify Images with Pre-Built Modedls using Vision API and Cloud AutoML

### Learning Objectives

- ビジネスシーンにおいて、非構造の ML モデルをどのように利用しているか、またモデルがどのように機能しているかを見極める
- 既成とカスタムの間でどのように ML モデルを選択するか
- Cloud AutoML を使って、1 行もコードを書かずに高性能なイメージ分類モデルを構築する

### Machine Learning on Unstructured Datasets

#### Video1: Deriving Insights from Unstructured Data using Machine Learning

=> 非構造データから洞察を導き出す

#### Video2: How does ML on unstructured data work?

Use Deep Learning when you can't explain the labeling rules

#### Video3: Demo ML built into Google Photos

#### Video4: Comparing approaches to ML

Good Machine Learning models require lots of high-quality trainig data

#### Video5: Demo Using ML building blocks

Google Translate ができること

[https://cloud.google.com/translate/](https://cloud.google.com/translate/)

Google Spreadsheet からも使える

```
=googletranslate(B3, "en", "ja")
```

Vison API ができること

- Label Detection
- Face Detection
- OCR
- Explicit Content Detection
- Landmark Detection
- Logo Detection

#### Video6: Using pre-built AI to create a chatbot

Dialogflow <- rebrand of "api.ai" which Google acquired

sentiment analysis

Dialogflow benefits: pre-built agents, one-click integrations

#### Video7: Customizing Pre-built models with AutoML

Vision API add your uploaded image several labels.

```
--AutoML Vision's menu
    |--IMAGES (1000 images needs longer hour than 1 node hour)
    |--TRAIN (1 node hour is free)
    |--EVALUATE
    `--PREDICT`
```

- Precision ... measure of quality 何枚が正しく認識されたか？
- Recall ... measure of quantity 正しいものが何枚認識されたか？

AutoML - codeless model building

Visio API - need invoking APi with a programming language

### Lab: Classify Images with Pre-built ML Models using Cloud Vision API and AutoML

#### 1. Setup AutoML Vision

GCP Console > APIs & Services > Library\
Search "Cloud AutoML API" > Enable

Open Cloud Shell

```bash
export P_ID=$DEVSHELL_PROJECT_ID
export Q_UN=student-03-bde9589eb5a0@qwiklabs.net

# Create Storage bucket
gsutil mb -p $P_ID -c regional -l us-central1 gs://$P_ID-vcm/
# -p ... Specify the project ID
# -c ... default storage class
# -l ... location
```

[https://console.cloud.google.com/vision/datasets?project=qwiklabs-gcp-03-9386b70ec2c7](https://console.cloud.google.com/vision/datasets?project=qwiklabs-gcp-03-9386b70ec2c7)

Click AutoML UI link > choose current project ID\

> AutoML Vision Datasets page

#### 2. Upload training images to Google Cloud Storage

GCP Console > STORAGE > Storage

```bash
export BUCKET=qwiklabs-gcp-03-9386b70ec2c7-vcm

gsutil -m cp -r gs://automl-codelab-clouds/* gs://${BUCKET}
# -m ... run in parallel
# -r ... copy recursively
```

#### 3. Create a dataset

```bash
gsutil cp gs://automl-codelab-metadata/data.csv .
sed -i -e "s/placeholder/${BUCKET}/g" ./data.csv
gsutil cp ./data.csv gs://${BUCKET}
```

AutML Vision Datasets page > "+ NEW DATASET" link\

> dataset name: "clouds"\
> leave "Single-label classification" checked\
> Click "CREATE DATESET" > next screen\
> Choose "Select a CSV file on Cloud Storage"\
> Input `gs://qwiklabs-gcp-03-9386b70ec2c7-vcm/data.csv`
> CONTINUE button
> Click IMAGE link > See "Running: Importing images"
> Wait for 15-25 minutes to finish

#### 4. Inspect images

- cirrus ... シーラス、つる、巻雲
- cumulonimbus ... 積乱雲、入道雲
- cumulus ... 堆積(たいせき)、累積、積雲

#### 5. Train your model

GCP Console > ARTIFICIAL INTELLIGENCE > Vision > Detasets\

> clouds > TRAIN tab > click START TRAINING\
> use default auto-generated name > leave "Cloud hosted"\
> CONTINUE button\
> Budget: 8 > Check "Deploy model to 1 node after training"\
> Click START TRAINING

#### 6. Evaluate your model

Click EVALUATE tab

- Confidence threshold 0.5 ... 調整可能
- Precision 83.33% ... 適合率
- Recall 83.33% ... 再現率
- Confusion Matrix ... 予測結果と真の結果の表

テストアイテムを残してあるので、threshold を動かしたときに\
毎回テストして、その結果を表示することができる

##### Confusion Matrix

このタブは、機械学習における、作成モデルの正確性を評価するための、よくある指標を提供する。これをチェックすることで、トレーニングデータを改善すべき点を判断する。

#### Precison と Recall を勉強する

正と負の 2 クラスの分類問題を考える。\
分類器の予測結果と，真の結果に基づいて以下のように分類する

| 予測結果\真の結果 |  真の結果が「正」   |  真の結果が「負」   |
| ----------------- | :-----------------: | :-----------------: |
| 予測結果が「正」  | TP (True Positive)  | FP (False Positive) |
| 予測結果が「負」  | FN (False Negative) | TN (True Negative)  |

- Precision ... 適合率、正と予測したデータのうち、実際に正であるものの割合
- Recall ... 再現率、実際に正であるもののうち、正であると予測されたものの割合

$$
Precision=\frac{TP}{TP+FP}
$$

$$
Recall=\frac{TP}{TP+FN}
$$

自分なりの理解\
Precision の足りない％は、間違えて含めちゃったものの割合\
Recall の足りない％は、含めそこなっちゃったものの割合\
キツキツにすれば Precision は良くなるが、Recall が悪くなる\
ユルユルにすれば Recall は良くなるが、Precision が悪くなる

#### 7. Generate predictions

Download these images

- image1.jpeg ... cirrus
- image2.jpeg ... cumulonimbus

Click TEST & USE > Click UPLOAD IMAGES

#### 8. Learn more

[Introducing Cloud AutoML \- YouTube](https://www.youtube.com/watch?v=GbLQE2C181U)

[Cloud AutoML: Making AI accessible to every business](https://www.blog.google/topics/google-cloud/cloud-automl-making-ai-accessible-every-business/)

[AutoML Vision ドキュメント  \|  Cloud AutoML Vision  \|  Google Cloud](https://cloud.google.com/vision/automl/docs/)

### Building a Custom Model

#### Video1 - Building a Custom Model

Train and run ML in the familiar BigQuery UI

- creating a machine learning data set
- identifying features and labels

#### Video2 - Demo: Text classification done three ways

solve the same machine learning model, three different ways

- BigQuery ML
- AutoML
- a custom model that written in Keras

go to BigQuery UI

```sql
SELECT
  url, title
FROM
  `bigquery-public-data.hacker_news.stories`
WHERE
  LENGTH(title) > 10
  AND LENGTH(url) > 0
LIMIT 10
```

URL から github, nytimes, techcrunch だけを抜き出して source とし、\
title から最初の 5 文字だけを抜き出して word1-5 とする

##### BigQuery ML

モデルを作成する

```sql
CREATE OR REPLACE MODEL advdata.txtclass
OPTIONS(model_type='logistic_reg', input_label_cols=['source'])
AS
-- オリジナルのクエリ
```

データセットに txtclass というモデルが出来上がる\
開くと、Details, Training, Schema という 3 つのタブがある

モデルを評価する

```sql
SELECT * FROM ML.EVALUATE(MODEL advdata.txtclass)
-- precision, recall, accuracy...
```

モデルを使って予測する

```sql
SELECT * FROM ML.PREDICT(MODEl advdata.txtclass, (
--- word1-5を与えるクエリ
))
```

##### AutoML

GCP Consle > ARTIFICIAL INTELLIGENCE > Natural Language\

> Google Cloud Naural Lnaguage page > Create a custom model box\
> click "Get started with AutoML"\
> 動画はカットされてしまっているが、おそらくここで CSV ファイルを使って、データを読み込んでいるはず\
> github, nytimes, techcrunch というラベルにそれぞれ 3 万前後のデータが入っている

TRAIN タブに行くと、すでにモデルが出来上がっている\
Analyzed 92395 text item, Precision 87.839%, Recall 86.218%

PREDICT タブに行くと、テキストボックスがあって、入力して PREDICT ボタンを押せる

##### Custom model using Keras

GCP Console > ARTIFICIAL INTELLIGENCE > ML Engine > Notebooks\
Notebook instances page > Click "+ NEW INSTANCE" > TesorFlow

インスタンスが出来上がると、一覧の行にある OPEN JUPYTERLAB リンクが使える\
Launcher > Other > Terminal

```bash
git clone https://github.com/GoogleCloudPlatform/training-data-analyst
```

Go to training-data-analyst/courses/machine-learning/deepdive/09_sequence\
Open text_classification.ipynb\

Text Classification using TensolFlow/Keras on Cloud ML Engine

Keras とは

[Home \- Keras Documentation](https://keras.io/ja/)

> Keras は，Python で書かれた，TensorFlow または CNTK，Theano 上で\
> 実行可能な高水準のニューラルネットワークライブラリです．

インスタンス上では、データは、Pandas のデータフレームとして扱われる\
BigQuery からデータを引っ張ってきて、trandf, evaldf の 2 つのデータフレームを作る\

```python
traindf = bg.query(query + "").to_dataframe()
evaldf = bg.query(query + "").to_dataframe()
```

オリジナルのデータのうち 75%をトレーニングに、25%を評価に使うべし

```python
traindf['source'].value_counts()
evaldf['source'].value_counts()
```

データをディスクに保存する

```python
import os, shutil
DATADIR='data/txtcls'
shutil.rmtree(DATADIR, ignore_errors=True)
os.makdir(DATADIR)
traindf.to_csv(os.path.join(DATADIR, 'train.csv', header=False))
evaldf.to_csv(os.path.join(DATADIR, 'eval.csv', header=False))
```

Keras のスクリプトでモデルを作成する

txtclsmodel/trainer/model.py

GCP の ML engine に jobs を登録する

```bash
gcloud ml-eingie jobs submit training $JOBNAME \
--region=$REGION \
--module-name=trainer.task \
--package-path=${PWD}/txtclsmodel/trainer \
--job-dir=$OUTDIR \
--scale-tier=BASIC_GPU \
--runtime-version=TFVERSION \
-- \
--output_dir=$OUTDIR \
--train_data_path=gs://${BUCKET}/txtcls/train.csv \
--eval_data_path=gs://${BUCKET}/txtcls/eval.csv \
--num_epoch=5
```

#### 復習: AutoML Vision と Vision API の違い

[AutoML Vision ドキュメント  \|  Cloud AutoML Vision  \|  Google Cloud](https://cloud.google.com/vision/automl/docs/)

> AutoML Vision を使用すると、独自に定義したラベルに従って画像を分類するよう機械学習モデルをトレーニングできます。

[Vision AI \| ML から画像情報を引き出す  \|  Cloud Vision API  \|  Google Cloud](https://cloud.google.com/vision/#)

> Google Cloud の Vision API は REST API や RPC API を使用して
> 強力な事前トレーニング済みの機械学習モデルを提供します。画像にラベルを
> 割り当てることで、事前定義済みの数百万のカテゴリに画像を高速で
> 分類できます。オブジェクトや顔を検出し、印刷テキストや手書き入力を読み取り、
> 有用なメタデータを画像カタログに作成します。

自分の理解\
Vision API は、画像を与えるだけで、ラベルを割り当ててくれる

このページについているアプリを試してみたが、雲の写真に対して、\
animal や hat といったラベリングをして、まったく雲を認識できなかった\
また、人の顔を検知するとその表情を分析しようとし、その他のことを認識しなかった

## Course Summary

1 Petabit/sec of total bisection bandwidth\
Google's data center network speed enables\
the separation of compute and storage

There are many roles in a data-driven organization

- Data Analyst
- Data Engineer
- Applied ML Engineer (Data Scientist, Statistician, Researcher)

Big Data Challenges

1. Migrating existing data workloads (ex:Hadoop, Spark jobs)
2. Building streaming data pipelines
3. Analyzing large dataset at scale
4. Applying machine learining to your data
