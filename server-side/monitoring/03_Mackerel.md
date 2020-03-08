# Mackerel

作成日 2019/10/18

## 01. Mackerel とは

はてなが運営するサーバー死活監視サービス

公式トップ => [https://mackerel.io/ja/](https://mackerel.io/ja/)

## 02. アカウントを作成する

1. [サインアップページ](https://mackerel.io/signup)
2. 自分のメールアドレスを入力（パスワードは求められず）
3. オーガニゼーション名を入力
4. 自分宛てに届いたメールに書いてあるリンクをクリック
5. パスワードを設定

## 03. エージェントをインストールする

Mackerel にホストを登録するには、\
登録したいホストに mackerel-agent をインストールする必要がある

[新規ホストの登録](https://mackerel.io/orgs/everglow/instruction-agent) => 後で必要になる API キーは、このページに表示されている

### Windows の場合

`mackerel-agent-latest.msi` (30.3MB) をダウンロードする\
インストールしている途中で API キーを入力する画面あり

### Ubuntu の場合

```bash
# インストールする
wget -q -O - https://mackerel.io/file/script/setup-all-apt-v2.sh | MACKEREL_APIKEY='<api-key>' sh
# => 途中で、sudo 実行するためのパスワードを求められる

# 動作確認
sudo journalctl -u mackerel-agent.service
```

## 04. ダッシュボードページの構成

左枠

```text
オーガニゼーションの選択
  |--Dashboards ... カスタムダッシュボードを追加するとここに表示される
  |   `--Overview ... オーガニゼーションのダッシュボード
  |--Hosts ... 監視されているホストの一覧
  |--Services ... ホストはサービスとロールで分類される
  |--Monitors ... 設定した監視ルールの一覧
  |--Channels ... チャンネルとは通知する手段のこと。Slack投稿もここで設定する
  `--Alerts ... 監視ルールで設定された閾値を超えたときに出るアラートの一覧
```
