---
tags: gcp
---

# Cloud SQL

作成日 2019/12/05、更新日 2020/01/24

## 01. Cloud SQLとは

フルマネージドのMySQL, PostgreSQL, SQL Serverデータベースサービス

公式トップ => [https://cloud.google.com/sql/](https://cloud.google.com/sql/)

### Cloud SQLの料金

[https://cloud.google.com/sql/pricing](https://cloud.google.com/sql/pricing)

#### インスタンスの料金

MySQL第2世代 東京リージョン 1か月あたり

マシンタイプ | スペック | 価格
-----------|---------|----
db-f1-micro | 共有vCPU, 0.6GB, 3GB | $9.96
db-g1-small | 共有vCPU, 1.7GB, 3GB | $33.22
db-n1-standard-1 | 1vCPU, 3.75GB, 30GB | $64.10

#### ストレージとネットワークの料金

東京リージョン 1か月あたり

- ストレージ ... $0.221 /GB
- 上りネットワーク ... 無料
- 下りネットワーク ... 共通

## 02. 注意すべき点

### 新しい場所からの接続を許可する場合

GCPコンソール ＞ ストレージ ＞ SQL ＞ Instancesページが表示される\
＞ 設定したいインスタンスをクリックする\
＞ 左枠 ＞ 接続 ＞ 接続ページ\
＞ パブリックIP ＞ 承認済みネットワークに、新しい場所を追加する

もちろんユーザーアカウントにも、ホスト名を指定して接続場所を指定するが、\
その前に、承認済みネットワークに追加しないと、接続できるようにはならない




