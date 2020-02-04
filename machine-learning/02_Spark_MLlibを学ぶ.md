# Spark MLlib を学ぶ

作成日 2020/01/09

## 01. DataProc の勉強中に登場した pyspark のコードを観察する

```python
import os
import sys
import pickle
import itertools
from math import sqrt
from operator import add
from os.path import join, isfile, dirname
from pyspark import SparkContext, SparkConf, SQLContext
from pyspark.mllib.recommendation import ALS, MatrixFactorizationModel, Rating
from pyspark.sql.types import StructType, StructField, StringType, FloatType

# MAKE EDITS HERE
CLOUDSQL_INSTANCE_IP = ''   # <---- CHANGE (database server IP)
CLOUDSQL_DB_NAME = 'recommendation_spark' # <--- leave as-is
CLOUDSQL_USER = 'root'  # <--- leave as-is
CLOUDSQL_PWD  = ''  # <---- CHANGE

# DO NOT MAKE EDITS BELOW
conf = SparkConf().setAppName("train_model")
sc = SparkContext(conf=conf)
sqlContext = SQLContext(sc)

jdbcDriver = 'com.mysql.jdbc.Driver'
jdbcUrl    = 'jdbc:mysql://%s:3306/%s?user=%s&password=%s' % (CLOUDSQL_INSTANCE_IP, CLOUDSQL_DB_NAME, CLOUDSQL_USER, CLOUDSQL_PWD)

# checkpointing helps prevent stack overflow errors
sc.setCheckpointDir('checkpoint/')

# Read the ratings and accommodations data from Cloud SQL
dfRates = sqlContext.read.format('jdbc').options(driver=jdbcDriver, url=jdbcUrl, dbtable='Rating', useSSL='false').load()
dfAccos = sqlContext.read.format('jdbc').options(driver=jdbcDriver, url=jdbcUrl, dbtable='Accommodation', useSSL='false').load()
print("read ...")

# train the model
model = ALS.train(dfRates.rdd, 20, 20) # you could tune these numbers, but these are reasonable choices
print("trained ...")

# use this model to predict what the user would rate accommodations that she has not rated
allPredictions = None
for USER_ID in range(0, 100):
  dfUserRatings = dfRates.filter(dfRates.userId == USER_ID).rdd.map(lambda r: r.accoId).collect()
  rddPotential  = dfAccos.rdd.filter(lambda x: x[0] not in dfUserRatings)
  pairsPotential = rddPotential.map(lambda x: (USER_ID, x[0]))
  predictions = model.predictAll(pairsPotential).map(lambda p: (str(p[0]), str(p[1]), float(p[2])))
  predictions = predictions.takeOrdered(5, key=lambda x: -x[2]) # top 5
  print("predicted for user={0}".format(USER_ID))
  if (allPredictions == None):
    allPredictions = predictions
  else:
    allPredictions.extend(predictions)

# write them
schema = StructType([StructField("userId", StringType(), True), StructField("accoId", StringType(), True), StructField("prediction", FloatType(), True)])
dfToSave = sqlContext.createDataFrame(allPredictions, schema)
dfToSave.write.jdbc(url=jdbcUrl, table='Recommendation', mode='overwrite')
```

## 02. pyspark とは

[pyspark · PyPI](https://pypi.org/project/pyspark/)

> (Spark) also supports a rich set of higher-level tools including Spark SQL for SQL and DataFrames, MLlib for machine learning, GraphX for graph processing, and Spark Streaming for stream processing.

## 03. MLlib とは

[MLlib: Main Guide \- Spark 2\.4\.4 Documentation](http://spark.apache.org/docs/latest/ml-guide.html)

[【機械学習】Spark MLlib を Python で動かしてレコメンデーションしてみる \- Qiita](https://qiita.com/kenmatsu4/items/42fa2f17865f7914688d)

> 今度は MLlib を使って協調フィルタリングを用いたレコメンデーションの実装を行います。
>
> ここからが本題。Spark に付属の MLlib にある ALS(Alternating Least Squares)という手法でレコメンデーションを行います。これは協調フィルタリングという手法で、あるユーザと嗜好(ここでは映画の rating)の類似した他のユーザの情報を用いて推論を行う方法です。映画のコンテンツはある意味無視して、ユーザーの行動から推論するところが特徴の一つです。

## 04. 協調フィルタリングとは

[協調フィルタリング \- Wikipedia](https://ja.wikipedia.org/wiki/%E5%8D%94%E8%AA%BF%E3%83%95%E3%82%A3%E3%83%AB%E3%82%BF%E3%83%AA%E3%83%B3%E3%82%B0)

> 協調フィルタリング（きょうちょうフィルタリング、Collaborative Filtering、CF）は、多くのユーザの嗜好情報を蓄積し、あるユーザと嗜好の類似した他のユーザの情報を用いて自動的に推論を行う方法論である。

[Collaborative Filtering \- RDD\-based API \- Spark 2\.4\.4 Documentation](http://spark.apache.org/docs/latest/mllib-collaborative-filtering.html)

[PySpark で協調フィルタリング \- Qiita](https://qiita.com/best_not_best/items/b3e843b912548695c027)

> 学習データを取得する
>
> データベース等から学習データを取得し、「user」「item」「rating」の 3 カラムを持つ CSV ファイルを作成します。例えば「過去 1 ヶ月分の商品購入データ」を用いる場合、「ユーザー ID」「商品 ID」「その商品を購入したかどうか（未購入:0/購入:1）」といったデータになります。以下、CSV ファイルの例です。
>
> rating の名前の通り、「ユーザーがその商品にどれだけ評価値を付けたかどうか」が本来の使い方になりますが、上記の通り「商品を購入したかどうか」、または「ページにアクセスしたかどうか」といったデータでも実装は可能です。前者の場合は「ユーザーがその商品を購入するスコアはどのくらいか」、後者は「ユーザーがそのページにアクセスするどのくらいか」を予測するモデルになります。
