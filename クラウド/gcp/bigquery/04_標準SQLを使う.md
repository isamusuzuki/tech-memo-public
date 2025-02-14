# 標準 SQL を使う

作成日 2020/01/16、更新日 2020/01/22

## 01. DELETE ステートメント

[データ操作言語の構文  \|  BigQuery  \|  Google Cloud](https://cloud.google.com/bigquery/docs/reference/standard-sql/dml-syntax?hl=ja)

```sql
DELETE [FROM] target_name [alias]
WHERE condition
```

- DELETE ステートメントを作成するごとに、WHERE キーワードと条件を指定する必要があります。
- WHERE キーワードは、すべての DELETE ステートメントに必須です。
  - 1 つのテーブル内のすべての行を削除するには、WHERE キーワードに true を指定

```sql
DELETE dataset.DetailedInventory
WHERE true

DELETE dataset.Inventory
WHERE quantity = 0

DELETE dataset.Inventory i
WHERE i.product NOT IN (SELECT product from dataset.NewArrivals)

DELETE dataset.Inventory
WHERE NOT EXISTS
  (SELECT * from dataset.NewArrivals
   WHERE Inventory.product = NewArrivals.product)
```

### DML ステートメントに関する上限

[割り当てと上限  \|  BigQuery  \|  Google Cloud](https://cloud.google.com/bigquery/quotas)

> テーブルごとの INSERT、UPDATE、DELETE、MERGE の各ステートメントを合わせた 1 日あたりの最大数 - 1,000

実行する行ではなく、実行する数だから問題ない

#### DML とは

DML = Data Manipulation Language データ操作言語

[データ操作言語  \|  BigQuery  \|  Google Cloud](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-manipulation-language?hl=ja)

> BigQuery のデータ操作言語（DML）を使用すると、BigQuery テーブルからデータを更新、挿入、および削除できます。
>
> DML ステートメントは、次の条件が満たされている場合、SELECT ステートメントと同じように実行できます。
>
> - 標準 SQL を使用する必要があること。
> - 宛先テーブルを指定できないこと。
>
> 各 DML ステートメントは、暗黙のトランザクションを開始します。つまり、成功した各 DML ステートメントの終了時に、ステートメントによる変更が自動的にコミットされます。複数ステートメントのトランザクションはサポートされていません。

### サンプルコード

SQL クエリー

```sql=
DELETE FROM `test-project.test-dataset.test_table`
where
  created_at >= '2019-12-14 00:00:00+09'
  and created_at < '2019-12-15 00:00:00+09'
```

Python コード

```python
with open(f'./delete.sql', mode='r', encoding='utf-8') as f:
    sql = f.read()
client = bigquery.Client()
row = client.query(sql).result()
print(row)
# => <google.cloud.bigquery.table._EmptyRowIterator>
```

## 02. WITH 句

[標準 SQL クエリ構文](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax?hl=ja)

- WITH 句には、1 つ以上の名前付きサブクエリが含まれる
- サブクエリは、後続の SELECT ステートメントから参照される度に実行される
- サブクエリの結果は実体化されない

```sql
WITH
    subQ1 AS (SELECT SchoolID FROM Roster),
    subQ2 AS (SELECT OpponentID FROM PlayerStats)
SELECT * FROM subQ1
UNION ALL
SELECT * FROM subQ2
```

サブクエリの中での WITH 句の使用も可能

```sql
WITH q1 AS (my_query)
SELECT *
FROM
    (WITH q2 AS (SELECT * FROM q1) SELECT * FROM q2)
```

**自分なりの感想**\
WITH 句を使うと、クエリー文を上から下に読めるようになるので\
（本来当たり前のこと）、積極的に利用していこうと思う
