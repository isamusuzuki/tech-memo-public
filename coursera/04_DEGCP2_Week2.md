# DEGCP2 Week1

作成日 2020/01/26

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

TODO
