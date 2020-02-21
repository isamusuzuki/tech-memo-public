# Cloud Firestore を学ぶ

作成日 2019/11/13、更新日 2019/12/06

## 01. Cloud Firestore とは

公式サイト => [https://cloud.google.com/firestore/](https://cloud.google.com/firestore/)

- Cloud Datastore の次世代版
- スケーラビリティの高い NoSQL データベース
- RESTful インターフェイスを持つ
- スキーマレスなデータベース
- 複数のアクセス方法
- フルマネージド

## 02. Cloud Firestore の料金体系

### 無料枠

[Quotas and limits  \|  Cloud Firestore  \|  Google Cloud](https://cloud.google.com/firestore/quotas)

> Cloud Firestore offers free quota that allows you to get started at no cost.
> The free quota amounts are listed below. If you need more quota, you must enable billing for your Cloud Platform project.
>
> Quotas are applied daily and reset around midnight Pacific time.

| Free tier        | Quota            |
| ---------------- | ---------------- |
| Stored data      | 1 GiB            |
| Document reads   | 50,000 per day   |
| Document writes  | 20,000 per day   |
| Document deletes | 20,000 per day   |
| Network egress   | 10 GiB per month |

=> 1 ギガバイトまでは無料ということを理解した

### 東京リージョンの料金

[料金  \|  Cloud Firestore  \|  Google Cloud](https://cloud.google.com/firestore/pricing?hl=ja)

=> 1 日あたりの無料割り当てについては、東京リージョンも変わらないことを理解した

無料割り当て超過分の料金（単価）

| Element                | Price   | Unit                          |
| ---------------------- | ------- | ----------------------------- |
| ドキュメントの読み取り | \$0.038 | ドキュメント 100,000 点あたり |
| ドキュメントの書き込み | \$0.115 | ドキュメント 100,000 点あたり |
| ドキュメントの削除     | \$0.013 | ドキュメント 100,000 点あたり |
| 保存データ             | \$0.115 | GB/月                         |

## 03. Cloud Firestore を使う前にデータベースの作成が必要

GCP コンソール ＞ 該当プロジェクト\
＞ 左枠 ＞ Datastore ＞ エンティティ ＞ 開始ページ

- Cloud Firestore は、次世代の Cloud Datastore
- Cloud Firestore は、ネイティブモードまたは DataStore モードで使用できる
- ネイティブモードは、Firestore API を使う（Python2.7 のサポートなし）
- Datastore モードは、Datastore API を使う

ネイティブモードを選択 ＞ ロケーションを選択\
＞ asia-northeast1 (Tokyo) ＞ 「データベースを作成」ボタン
