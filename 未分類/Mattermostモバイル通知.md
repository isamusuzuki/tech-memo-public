---
tags: mattermost
---

# Mattermostモバイル通知

作成日 2019/06/06

インストールしたバージョンは `v5.11.0`

## 1. モバイル通知は動いているのか

![20190606_mattermost.png](https://imgur.com/tLJPhuU.png)

### TPNS Connectionとは

公式ガイド FAQ: push notice free? => [https://docs.mattermost.com/overview/faq.html#are-push-notifications-free](https://docs.mattermost.com/overview/faq.html#are-push-notifications-free)

push notificationsは、以下の二通りで無料

- 自分でpush-proxy serviceをコンパイルする
- Mattermostが提供しているTPNSを使う

### push-proxy serviceをコンパイルするとは

[Push Notification Service](https://developers.mattermost.com/contribute/mobile/push-notifications/service/)

- 最低1GBのメモリを搭載したLinuxサーバー
- Mattermost Push Notification Serviceのコピー
- モバイルアプリをカスタマイズする
- アップルデベロッパーコンソールから入手した秘密鍵と公開鍵
- Firebase Consoleから入手したFirebase Cloud Messaging Serverのキー

### TPNSを使うとは

TPNS = Test Push Notification Service

製品レベルのSLAは提供できないが、トランスポート層の暗号化はされている

MattermostはTPNSをホストしている => `https://push-test.mattermost.com`

### Mattermostの価格プラン

[Mattermost Pricing](https://mattermost.com/pricing/)

- Enterprise E10 ... $3.25 / user / month / annual => 1人1年39ドル
- Enterprise E20 ... $8.50 / user / month / annual => 1人1年102ドル

## 2. とりあえずの方針

この会社はスマホファーストで、Eメールはまったく使われていない

メールアドレスが与えられていないクルーもたくさんいる

モバイル通知は大事だが、とりあえず、TPNSを使ってるところから始めることにする

うまくいかない場合に、あらためて考える

### 参考記事

[MattermostでiOS/Androidクライアントにpush通知 \- Qiita](https://qiita.com/terukizm/items/d34ea4dbc00a9da920b8)
