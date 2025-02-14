# Python から BigQuery を使う

作成日 2019/12/12

## 01. Python ライブラリの情報のありか

パッケージ => [google\-cloud\-bigquery · PyPI](https://pypi.org/project/google-cloud-bigquery/)

リポジトリ => [googleapis/google\-cloud\-python: Google Cloud Client Library for Python](https://github.com/googleapis/google-cloud-python)

ドキュメント(Python 特化) => [Python Client for Google BigQuery — google\-cloud\-bigquery 0\.1\.0 documentation](https://googleapis.dev/python/bigquery/latest/index.html)

ドキュメント(包括的) => [BigQuery クライアント ライブラリ  \|  BigQuery  \|  Google Cloud](https://cloud.google.com/bigquery/docs/reference/libraries?hl=ja)

## 02. クイックスタートを写経する

[クイックスタート: クライアント ライブラリの使用  \|  BigQuery  \|  Google Cloud](https://cloud.google.com/bigquery/docs/quickstarts/quickstart-client-libraries?hl=ja)

### 始める前に

1. GCP プロジェクトを選択または作成する
2. BigQuery API を有効にする
3. サービスアカウントキーを作成する

### クライアントライブラリのインストール

```bash
pip install google-cloud-bigquery
```

2019/12/12 現在のライブラリバージョン

```text
google-cloud-bigquery==1.23.0
pandas==0.25.3
pyarrow==0.15.1
```

### Python のサンプルコード

```python
from google.cloud import bigquery

def query_stackoverflow():
    client = bigquery.Client()
    query_job = client.query("""
        SELECT
          CONCAT(
            'https://stackoverflow.com/questions/',
            CAST(id as STRING)) as url,
          view_count
        FROM `bigquery-public-data.stackoverflow.posts_questions`
        WHERE tags like '%google-bigquery%'
        ORDER BY view_count DESC
        LIMIT 10""")

    results = query_job.result()  # Waits for job to complete.

    for row in results:
        print("{} : {} views".format(row.url, row.view_count))

if __name__ == '__main__':
    query_stackoverflow()
```

### サンプルコードが動くようになるまでに行ったこと

#### ImportError 発生

```text
ImportError: cannot import name 'collections_abc' from 'six.moves' (unknown location)
```

six ライブラリを 1.12 から 1.13 に更新した

#### Access Denied 発生

```text
google.api_core.exceptions.Forbidden:
403 POST https://bigquery.googleapis.com/bigquery/v2/projects/PROJECT-NAME/jobs:
Access Denied:
Project PROJECT-NAME:
User does not have bigquery.jobs.create permission in project PROJECT-NAME.
```

bigquery-public-data は、デフォルトのプロジェクトにおける役割をチェックする。\
デフォルトのプロジェクトで、該当するサービスアカウントに、\
「BigQuery ユーザー」という役割を与えた

## 03. 自分で試す

一般公開データではなく、自分が実際に作成したテーブルにアクセスしてみる

```python
from google.cloud import bigquery


def query_test_top20():
    client = bigquery.Client()
    sql = (
        'SELECT'
        ' sku_code,master_item_name'
        ', sum(amount) count, sum(amount_price) price'
        ' FROM `project-id.dataset-name.table-name`'
        ' where is_cancel = false'
        ' group by sku_code, master_item_name'
        ' order by count desc'
        ' limit 20'
    )
    query_job = client.query(sql)
    results = query_job.result()

    for row in results:
        print(
            f'{row.sku_code} {row.master_item_name}:'
            f' {row.count}, {row.price}'
        )


if __name__ == "__main__":
    query_test_top20()
```

### API リファレンスを見ながら、サンプルコードを読み解く

[API Reference — google\-cloud\-bigquery 0\.1\.0 documentation](https://googleapis.dev/python/bigquery/latest/reference.html)

1. bigquery.client.Client クラスは、API アクセスに必要な認証情報をバンドルする
1. Client.query(query)メソッドは、QueryJob インスタンスを生成する
1. bigquery.job.QueryJob クラスは、テーブルへのクエリーを実行する非同期ジョブである
1. QueryJob.result()メソッドは、ジョブをスタートし、完了まで待ち、bigquery.table.RowIterator クラスを生成する
1. RowIterator クラスをイテレーターとして使うと、bigquery.table.Row クラスを順番に得られる
1. Row クラスには、辞書としてもしくはプロパティとしてキーを与えることで、値にアクセスできる
1. RowIterator クラスには to_dataframe()メソッドがあり、すべての結果を pandas の DataFrame に変換する

## 04. 解説記事を読む

[Python から BigQuery を読み書きするクラス \- Qiita](https://qiita.com/r2en/items/f0e86642eb5e6a40703a)

BigQuery のデータを、pandas の DataFrame に変換することも、\
pandas の DataFrame を BigQuery にあげることもできるようだ

```python
from google.cloud import bigquery

client = bigquery.Client

dataframe = client.query(QUERY, project=PROJECT).to_dataframe()

dataframe.to_gbq('database.table', project_id=PROJECT)
```

### Pandas の Dataframe に to_gbq()メソッドがある

[https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.to_gbq.html](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.to_gbq.html)

> Write a DataFrame to a Google BigQuery table.
> This functions requires the pandas-gbq package.

## 05. pandas-gbq とは

サードパーティの BigQuery クライアントライブラリ

[https://pandas-gbq.readthedocs.io/en/latest/](https://pandas-gbq.readthedocs.io/en/latest/)

### pandas-gbq を使わなければならないシーンはあるのか

結論：ない

[pandas\-gbq からの移行  \|  BigQuery  \|  Google Cloud](https://cloud.google.com/bigquery/docs/pandas-gbq-migration?hl=ja)

### 標準 SQL 構文を使ってデータのクエリ

すでに、google-cloud-bigquery ライブラリに\
to_dataframe()メソッドがあることを知っているので、\
pandas-gbq ライブラリは必要ない

```python
from google.cloud import bigquery


client = bigquery.Client()
sql = """
    SELECT name
    FROM `bigquery-public-data.usa_names.usa_1910_current`
    WHERE state = 'TX'
    LIMIT 100
"""

# 明示的にプロジェクトIDを指定
project_id = 'your-project-id'
df = client.query(sql, project=project_id).to_dataframe()
```

### 構成を含むクエリーの実行

```python
from google.cloud import bigquery

client = bigquery.Client()
sql = """
    SELECT name
    FROM `bigquery-public-data.usa_names.usa_1910_current`
    WHERE state = @state
    LIMIT @limit
"""
query_config = bigquery.QueryJobConfig(
    query_parameters=[
        bigquery.ScalarQueryParameter('state', 'STRING', 'TX'),
        bigquery.ScalarQueryParameter('limit', 'INTEGER', 100)
    ]
)

df = client.query(sql, job_config=query_config).to_dataframe()
```

## 06. pandas データを BigQuery テーブルに送信する

DataFrame を Parquet 形式に変換してから BigQuery API に送信する\
Parquet は、ネストした値と配列値をサポートする\
pyarrow（Parquet エンジン）を、インストールする必要がある

```python
from google.cloud import bigquery
import pandas

df = pandas.DataFrame(
    {
        'my_string': ['a', 'b', 'c'],
        'my_int64': [1, 2, 3],
        'my_float64': [4.0, 5.0, 6.0],
    }
)
client = bigquery.Client()
table_id = 'my_dataset.new_table'
# Since string columns use the "object" dtype, pass in a (partial) schema
# to ensure the correct BigQuery data type.
job_config = bigquery.LoadJobConfig(schema=[
    bigquery.SchemaField("my_string", "STRING"),
])

job = client.load_table_from_dataframe(
    df, table_id, job_config=job_config
)

# Wait for the load job to complete.
job.result()
```

### client.load_table_from_dataframe()メソッド

- 説明 ... upload the contents of a table from a pandas DataFrame
- 戻り値 ... bigquery.job.LoadJob クラスの新しいインスタンスを生成

[google\.cloud\.bigquery\.client\.Client — google\-cloud\-bigquery 0\.1\.0 documentation](https://googleapis.dev/python/bigquery/latest/generated/google.cloud.bigquery.client.Client.html)

```python
job = client.load_table_from_dataframe(
    dataframe,
    destination,
    num_retries=6,
    job_id=None,
    job_id_prefix=None,
    location=None,
    project=None,
    job_config=None,
    parquest_compression='snappy'
)
```

### bigquery.schema.SchemaField クラス

[google\.cloud\.bigquery\.schema\.SchemaField — google\-cloud\-bigquery 0\.1\.0 documentation](https://googleapis.dev/python/bigquery/latest/generated/google.cloud.bigquery.schema.SchemaField.html)

```python
schema = [
    bigquery.SchemaField(
        name,
        field_type,
        mode='NULLABLE',
        description=None,
        fields=()
    )
]
```

使用可能な field_type

- STRING
- BYTES
- INTEGER, INT64
- FLOAT, FLOAT64
- BOOLEAN, BOOL
- TIMESTAMP
- DATE
- TIME
- DATETIME
- RECORD, STRUCT (a nested schema)

使用可能な mode

- NULLABLE (default)
- REQUIRED
- REPEATED

## 07. pyarrow とは

Python Library for Apache Arrow

パッケージ => [pyarrow · PyPI](https://pypi.org/project/pyarrow/)

インストール => `pip install pyarrow`

[Python: Apache Parquet フォーマットを扱ってみる \- CUBE SUGAR CONTAINER](https://blog.amedama.jp/entry/2017/10/10/135331)

> pyarrow は Apache Arrow プロジェクトの Python 実装という位置づけ。\
> Apache Arrow というのは、データエンジニアリングにおいてプログラミング言語などに\
> 依存しないメモリ上での共通のオブジェクト表現を実現するためのプロジェクト。\
> Apache Arrow のオブジェクトを永続化するために Apache Parquet フォーマットが使える。

```python
import pyarrow as pa
from pyarrow import parquet as pq

table = pa.Table.from_pandas(df)
pq.write_table(table, 'users.parquet', compression='qzip')

table2 = pq.read_table('users.parquet')
table2.to_pandas()
```

Apache Parquet 形式は、圧縮できるのが強み

### Apache Parquet 形式とは

公式トップ => [Apache Parquet](http://parquet.apache.org/)

> Apache Parquet is a columnar storage format available to any project in the Hadoop ecosystem

[Python で csv ファイルを Parquet 形式に変換 \- Qiita](https://qiita.com/TaigoKuriyama/items/cedcc9436f4456191601)

> Apache Parquet とは csv などの行志向のデータフォーマットと違い、\
> 列志向のフォーマットで、列単位でデータを取り出す分析用途に向いています。

[カラムナフォーマットのきほん 〜データウェアハウスを支える技術〜 \- Retty Tech Blog](https://engineer.retty.me/entry/columnar-storage-format)

[カラムナフォーマットのきほん \- Speaker Deck](https://speakerdeck.com/chie8842/karamunahuomatutofalsekihon-2)

> - テキストフォーマット ... CSV, JSON
> - 行指向フォーマット ... AVRO
> - 列指向（カラムナ）フォーマット ... Parquet, orc
>
> 列指向フォーマットの特徴
>
> - 列方向に連続してデータを保持する
> - 特定の列のみ取得する分析用途に有利
> - 列のカーディナリティが低い場合、圧縮率が高くなる
> - 一般的に分析用途のビッグデータ基盤はこちらが向いている
