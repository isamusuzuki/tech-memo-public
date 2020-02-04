---
tags: gcp
---

# Cloud Pub/Sub

作成日 2019/12/17

## 01. Cloud Pub/Subとは

公式トップ => [https://cloud.google.com/pubsub/](https://cloud.google.com/pubsub/)

> - グローバルメッセージングとイベント取り込みが簡単になります
> - Cloud Pub/Subは、大規模および小規模のパブリッシュ/サブスクライブパターンをサポートする、スケーラブルで耐久性のあるイベント取り込みおよび配信システムです
> - Cloud Pub/Subは、イベントデータのパブリッシャとサブスクライバを切り離すことによって、システムをより堅牢にします。
>
> Cloud Pub/Sub にイベントがパブリッシュされると、次のように処理されます
>
> - Pushサブスクリプションが、Cloud FunctionsまたはApp Engineで稼働するサーバーレス アプリにイベントを配信します
> - Pullサブスクリプションが、Google Kubernetes EngineまたはCloud Dataflowで稼働する複雑なステートフルサービスに対してイベントを利用可能にします
> - Cloud Pub/Subのグローバルな性質のおかげで、マルチリージョン環境がシームレスに動作します

ドキュメント => [https://cloud.google.com/pubsub/docs/](https://cloud.google.com/pubsub/docs/)

### Cloud Pub/Subの基本コンセプト

- トピック ... パブリッシャーがメッセージを送信する名前付きのリソース
- サブスクリプション ... メッセージストリームを表す名前付きのリソース。このストリームはサブスクライバーに配信される
- メッセージ ... パブリッシャーがトピックに送信し、最終的にはサブスクライバーに配信されるデータ
- メッセージ属性 ... パブリッシャーがメッセージに設定できる Key-Value ペア

### パブリッシャーとサブスクライバーの関係

- パブリッシャーによってメッセージが作成され、トピックに送信される
- サブスクライバーによって、トピックに対するサブスクリプションが作成され、それからメッセージが受け取られる
- 通信は 1対多、多対1、多対多にできる

## 02. Cloud Pub/Subの料金

料金ページ => [https://cloud.google.com/pubsub/pricing?hl=ja](https://cloud.google.com/pubsub/pricing?hl=ja)

データ容量は、pull, push, publish の各オペレーションのメッセージデータと属性データを使用して計算される

1ヶ月あたりのデータ容量 => 最初の10GBは、$0.00/TiB。それ以降は、$40/TiB。TiBは約 1.01 兆バイト

リクエストごとに請求対象となるデータ最小量は、1KBである

### 料金計算例

アプリケーションで2つのサブスクリプションがあるトピックに、1,024KBのメッセージを、1MiB/秒でパブリッシュする

サブスクライバーが常時稼働し、Cloud Pub/Sub が 1MiB/秒で取り込みを行い、2MiB/秒で配信を行う

合計のデータ伝送速度は 3MiB/秒になる

30日間で計算すると、7.416TiBとなる

無料枠(10GB)を取り除いて、7.406TiBとなり、40 x 7.406 = 296.24となる

### プロジェクトをまたいだ請求

複数のプロジェクトでCloud Pub/Subを使用している場合、料金はリクエストされたリソースを含むプロジェクトに請求される

プロジェクトAのサービスアカウントに、プロジェクトBのサブスクリプションに対するサブスクライバーアクセスが付与されている場合、請求先アカウントBには、サービスアカウントAがサブスクリプションから取得するデータの料金が課金される

サブスクリプションを持っているプロジェクトが、そのサブスクリプションから取り出したデータ分だけ請求される。


## 03. gcloud pubsubコマンドを使ってみる

参考記事 => [gcloud ツールで Pub/Sub を行う \- Qiita](https://qiita.com/ekzemplaro/items/2c47beb962bee54b4609)

```bash=
# トピックを作成する
gcloud pubsub topics create taro_topic_1
# => Created topic [projects/taro1/topics/taro_topic_1].

# サブスクリプションを作成する
gcloud pubsub subscriptions create --topic taro_topic_1 taro_subscription_1
# => Created subscription [projects/taro1/subscriptions/taro_subscription_1].

# メッセージを送信する
gcloud pubsub topics publish taro_topic_1 --message "Okamoto!"
# messageIds:
# - '641703973332605'

# メッセージを取得する（ackするとデータは消える）
gcloud pubsub subscriptions pull --auto-ack taro_subscription_1
#+-----------+-----------------+------------+
#|    DATA   |    MESSAGE_ID   | ATTRIBUTES |
#+-----------+-----------------+------------+
#| Okamoto!  | 641703973332605 |            |
#+-----------+-----------------+------------+

gcloud pubsub subscriptions pull --auto-ack taro_subscription_1
# Listed 0 items.

# 一覧表示系
gcloud pubsub topics list
# ---
# name: projects/taro1/topics/taro_topic_1

gcloud pubsub topics list-subscriptions taro_topic_1
# ---
#  projects/taro1/subscriptions/taro_subscription_1

gcloud pubsub subscriptions list
# ---
# ackDeadlineSeconds: 10
# expirationPolicy:
#   ttl: 2678400s
# messageRetentionDuration: 604800s
# name: projects/taro1/subscriptions/taro_subscription_1
# pushConfig: {}
# topic: projects/taro1/topics/taro_topic_1
```
