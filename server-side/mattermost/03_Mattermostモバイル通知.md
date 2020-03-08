# Mattermost モバイル通知

作成日 2019/06/06

インストールしたバージョンは `v5.11.0`

## 01. モバイル通知は動いているのか

![20190606_mattermost.png](https://imgur.com/tLJPhuU.png)

### TPNS Connection とは

公式ガイド FAQ: push notice free? => [https://docs.mattermost.com/overview/faq.html#are-push-notifications-free](https://docs.mattermost.com/overview/faq.html#are-push-notifications-free)

push notifications は、以下の二通りで無料

-   自分で push-proxy service をコンパイルする
-   Mattermost が提供している TPNS を使う

### push-proxy service をコンパイルするとは

[Push Notification Service](https://developers.mattermost.com/contribute/mobile/push-notifications/service/)

-   最低 1GB のメモリを搭載した Linux サーバー
-   Mattermost Push Notification Service のコピー
-   モバイルアプリをカスタマイズする
-   アップルデベロッパーコンソールから入手した秘密鍵と公開鍵
-   Firebase Console から入手した Firebase Cloud Messaging Server のキー

### TPNS を使うとは

TPNS = Test Push Notification Service

製品レベルの SLA は提供できないが、トランスポート層の暗号化はされている

Mattermost は TPNS をホストしている => `https://push-test.mattermost.com`

### Mattermost の価格プラン

[Mattermost Pricing](https://mattermost.com/pricing/)

-   Enterprise E10 ... \$3.25 / user / month / annual => 1 人 1 年 39 ドル
-   Enterprise E20 ... \$8.50 / user / month / annual => 1 人 1 年 102 ドル

## 02. とりあえずの方針

この会社はスマホファーストで、E メールはまったく使われていない

メールアドレスが与えられていないクルーもたくさんいる

モバイル通知は大事だが、とりあえず、TPNS を使ってるところから始めることにする

うまくいかない場合に、あらためて考える

### 参考記事

[Mattermost で iOS/Android クライアントに push 通知 \- Qiita](https://qiita.com/terukizm/items/d34ea4dbc00a9da920b8)
