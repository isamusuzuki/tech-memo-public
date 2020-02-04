---
tags: gcp
---

# Cloud DataProc

作成日 2019/12/16、更新日 2020/01/06

## 01. Cloud DataProcとは

Apache SparkとApache Hadoopのマネージドサービス\
Spark と Hadoop のツール、ライブラリ、ドキュメントを使用できる\
データ分析カテゴリのひとつ

公式トップ => [https://cloud.google.com/dataproc/](https://cloud.google.com/dataproc/)

## 02. Apache Hadoopとは

公式トップ => [https://hadoop.apache.org/](https://hadoop.apache.org/)

### Apache Hadoopを構成する技術群

- HDFS(Hadoop Distributed File System) ... 分散ファイルシステム
- MapReduce ... 分散計算フレームワーク
- HBase ... 列指向のデータベース、BigTableがモデルで、Javaで書かれている

### 『Hadoopファーストガイド』を読む

#### P.2 Hadoop登場

2004年、グーグルは社内で大量のデータを処理するためにMapReduceと呼ばれる独自の分散処理フレームワークを
利用していることを論文で発表した。グーグルは、2004年8月の1ヶ月で、3ペタバイト以上のデータをMapReduceを
使って実際に処理していた

MapReduceは、大きな処理を細かい処理に分割して、その細かい処理を別々のサーバが同時並行でこなす、
スケールアウトの考え方

これに触発されたのが、オープンソースの全文検索ライブラリであるApache Luceneを実装していたDoug Cutting氏で、
独自に、GFS(Google File System)やMapReduceの仕組みをApache Nutchに組み込んだ。2006年には分散処理の
実装部分がApache Hadoopとして公開された

#### P.21 グーグルの基盤技術と対応するオープンソース実装

グーグル | オープンソース | 説明
--------|-------------|-----
GFS | HDFS | 分散処理ファイルシステム
MapReduce | Hadoop MapReduce | 分散処理フレームワーク
BigTable | HBase | 分散データベース

#### P.22 HDFSの設計思想

- ディスクのブロックサイズを、通常の数KB程度から、64MB以上に大きく設定し、ディスクの読み込みコストを最小限に抑える
- データの読み込みコスト = シーク時間（ここを1%に抑える） + データの転送時間
- データの複製を複数のサーバーに格納して、データの冗長性を確保する。RAIDは使わない
- HDFSのデータ書き込みは常にファイルの末尾に行う。ファイル中の任意の位置の修正はサポートしていない
- ファイルのメタデータ（位置情報）は、ネームノードのメモリ以上に持つ

クラスタは、束になったサーバ群のこと\
ノードは、個々のサーバーのこと\
ネームノードは、全体を管理するマスター\
データノードは、スレーブ

ネームノードは、HDFSの単一障害点(Single Point Of Failure)になる

#### P.28 Hadoop MapReduceの設計思想

データは、MapフェーズとReduceフェーズを通る\
その間にSuffleフェーズもあるが、そこは自動で処理が行われる

ジョブ = 入力データ、Mapper、Reducer、設定情報\
ジョブはタスクに分割される

- ジョブトラッカー ... マスター
- タスクトラッカー ... スレーブ

#### P.32 HBase

HBaseはHDFS上に構築された分散データベース\
リレーショナルではなく、SQLもサポートしていない

#### 書籍について

『Hadoopファーストガイド』佐々木達也 著\
秀和システム 2012年 発行

## 03. Apache Sparkとは

公式トップ => [https://spark.apache.org/](https://spark.apache.org/)

ドキュメント => [http://spark.apache.org/documentation.html](http://spark.apache.org/documentation.html)

紹介記事 => [Apache Sparkとは何か――使い方や基礎知識を徹底解説 \(1/3\)](https://www.atmarkit.co.jp/ait/articles/1608/24/news014.html)

> 巨大なデータに対して高速に分散処理を行うオープンソースのフレームワーク\
> JavaやScala、Pythonなどいろいろな言語のAPIが用意されている\
> 分散処理のややこしい部分をうまく抽象化しているので、簡潔なコードで、\
> 何百台ものコンピュータで、同時並行に計算を実行させることができる

pysparkというパッケージがあった => [https://pypi.org/project/pyspark/](https://pypi.org/project/pyspark/)

紹介記事 => [pandasの処理をPysparkで \- Qiita](https://qiita.com/niship2/items/5d50b534697549878dea)

> pandasがApache Arrowを経由してsparkというものにつながっている\
>
> 調べてみるとarrrowはいちいちcsvなどでデータ処理ツール間を経由させずに\
> 高速にデータを動かすことができる中間体みたいなものを提供し、\
> sparkは分散処理に強いツールということでした。







