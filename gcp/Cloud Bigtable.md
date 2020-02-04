---
tags: gcp
---

# Cloud Bigtable

作成日 2019/12/03

## 01. Cloud Bigtableとは

公式トップ => [https://cloud.google.com/bigtable/](https://cloud.google.com/bigtable/)

### 東京リージョンの料金

機能 | 料金
----|----
ノード | $0.85 node/hr
SSDストレージ | $0.22 GB/Month
HDDストレージ | $0.034 GB/Month
ネットワーク（上り） | 無料
ネットワーク（下り） | $0.14 GB

### 「Cloud Bigtableの概要」を読む

[Cloud Bigtable の概要](https://cloud.google.com/bigtable/docs/overview)

> Cloud Bigtable はスパース（低密度）に格納されるテーブルであり、数十億行、数千列の規模にスケール可能です。\
> これにより、数テラバイト、あるいは数ペタバイトのデータを格納できます。\
> 各行の単一の値がインデックスに登録され、この値が行キーとなります。\
> Cloud Bigtable は、極めて低いレイテンシで単一キーの超大容量データを格納するのに理想的です。 \
> 低レイテンシで高い読み取り / 書き込みスループットを実現できるため、\
> MapReduce オペレーションに理想的なデータソースです。

#### MapReduceとは

[Hadoop MapReduce入門](https://dev.classmethod.jp/hadoop/hadoop-advent-calendar-01-introduction-mapreduce/)

> MapReduceとは、分散処理のフレームワークの一つです。\
> 利用者はMapと呼ばれる処理とReduceと呼ばれる処理のみ実装すれば\
> 分散処理環境でそれを簡単に実行できるというものです。

- Map ... 入力を受け取り、それを変換しkey-valueの形式で出力を行う処理を実装する
- Reduce ... Mapで出力したkey-valueのペアのうち、同じkeyを持つものを集約して処理を行う

分散させて並列で処理可能な仕組みになっている

#### ストレージとデータベースに関するその他のオプション

> Cloud Bigtable はリレーショナル データベースではありません。\
> したがって、SQL クエリや結合、複数行トランザクションは\
> サポートされていません。\
> また、格納するデータが1TB未満の場合に適切なソリューションではありません。

### Apache HBaseとは

公式トップ => [http://hbase.apache.org/](http://hbase.apache.org/)

> Apache HBase is an open-source, distributed, versioned, 
> non-relational database modeled after Google's Bigtable: 
> A Distributed Storage System for Structured Data by Chang et al. 
>
> Just as Bigtable leverages the distributed data storage 
> provided by the Google File System, Apache HBase provides 
> Bigtable-like capabilities on top of Hadoop and HDFS.

Cloud Bigtable は HBase API をサポートしている

GFS + Bigtable = HDFS + HBase

## 02. 「cbtを使用したクイックスタート」を読む

[cbt を使用したクイックスタート  \|  Cloud Bigtable ドキュメント  \|  Google Cloud](https://cloud.google.com/bigtable/docs/quickstart-cbt)

### GCPコンソールを使って、Cloud Bigtableインスタンスを作成する

- インスタンス名 ... Quickstart instance
- インスタンスID ... quickstart-instance
- インスタンスのタイプ ... 開発
- ストレージの種類 ... SSD
- クラスタID ... quickstart-instance-c1
- リージョン ... us-east1
- ゾーン ... us-east1-c
- 作成をクリック

### インスタンスに接続する

```bash=
gcloud components update
gcloud components install cbt

echo project = [PORJECT_ID] > ~/.cbtrc
echo instance = quickstart-instance >> ~/cbtrc
```

### データの読み取りと書き込み

```bash=
cbt ceatetable my-table

cbt ls

cbt createfamily my-table cf1

cbt lst my-table

cbt set my-table r1 cf1:c1=test-value

cbt read my-table

cvt deletetable my-table
```

