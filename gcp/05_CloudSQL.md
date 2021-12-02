# Cloud SQL

作成日 2019/12/05、更新日 2021/12/02

## 01. Cloud SQL とは

フルマネージドの MySQL, PostgreSQL, SQL Server データベースサービス

公式トップ => [https://cloud.google.com/sql/](https://cloud.google.com/sql/)

### Cloud SQL の料金

[https://cloud.google.com/sql/pricing](https://cloud.google.com/sql/pricing)

#### インスタンスの料金

MySQL 第 2 世代 東京リージョン 1 か月あたり

| マシンタイプ     | スペック              | 価格    |
| ---------------- | --------------------- | ------- |
| db-f1-micro      | 共有 vCPU, 0.6GB, 3GB | \$9.96  |
| db-g1-small      | 共有 vCPU, 1.7GB, 3GB | \$33.22 |
| db-n1-standard-1 | 1vCPU, 3.75GB, 30GB   | \$64.10 |

#### ストレージとネットワークの料金

東京リージョン 1 か月あたり

- ストレージ ... \$0.221 /GB
- 上りネットワーク ... 無料
- 下りネットワーク ... 共通

## 02. 注意すべき点

### 新しい場所からの接続を許可する場合

GCP コンソール ＞ ストレージ ＞ SQL ＞ Instances ページが表示される\
＞ 設定したいインスタンスをクリックする\
＞ 左枠 ＞ 接続 ＞ 接続ページ\
＞ パブリック IP ＞ 承認済みネットワークに、新しい場所を追加する

もちろんユーザーアカウントにも、ホスト名を指定して接続場所を指定するが、\
その前に、承認済みネットワークに追加しないと、接続できるようにはならない

### タイムゾーンを日本に変更する

タイムゾーンの設定を確認する

```sql
show variables like '%time_zone%';
-- system_time_zone UTC
-- time_zone SYSTEM
```

これを日本に変更するには、Cloud SQL の管理画面でフラグを立てる必要がある

SQL ＞ インスタンス ＞ 目当てのインスタンスIDをクリックする\
概要ページ ＞ 構成コーナー ＞ 「構成を編集」をクリック\
フラグコーナー ＞ フラグを開く ＞ 「フラグを追加」をクリック\
フラグ名に「default_time_zone」を選ぶ ＞ 値に「+09:00」を入力する\
「完了」ボタン ＞ 「保存」ボタン ＞ 再起動する

タイムゾーンの設定をもう一度確認する

```sql
show variables like '%time_zone%';
-- system_time_zone UTC
-- time_zone +09:00

select now();
-- 2021-02-09 18:18:53
```
