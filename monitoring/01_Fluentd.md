# Fluentd

作成日 2019/12/02、更新日 2019/12/11

## 01. Fluentd とは

公式トップ => [Fluentd \| Open Source Data Collector \| Unified Logging Layer](http://www.fluentd.org/)

> Apache logs => Filter/Buffer/Routing => Elasticsearch, MongoDB, AWS S3

[Quickstart Guide \| Fluentd](http://docs.fluentd.org/v0.12/articles/quickstart)

> fluentd はログ収集システム、ログを JSON データとして扱う

[柔軟なログ収集を可能にする「fluentd」入門 \- さくらのナレッジ](http://knowledge.sakura.ad.jp/tech/1336/)

## 02. Amazon Linux 設定手順

[Amazon Linux に Fluentd をインストールして S3 と MongoDB 連携する ｜ Developers\.IO](http://dev.classmethod.jp/cloud/amazon-linux-fluentd-setup-plugin/)

リポジトリを追加する

```bash
sudo vi /etc/yum.repos.d/td.repo
```

`td.repo`ファイルの中身

```text
[treasuredata]
name=TresureData
baseurl=http://packages.treasuredata.com/redhat/$basearch
gpgcheck=0
```

Fluentd をインストールする

```bash
# yumでインストールする
sudo yum install td-agent -y
sudo service td-agent start
sudo chkconfig td-agent on

# ログ収集のために権限を設定する
sudo chgrp td-agent /var/log/httpd/
sudo chmod g+rx /var/log/httpd/

# プラグインをインストールする
sudo /usr/lib64/fluent/ruby/bin/fluent-gem update
sudo /usr/lib64/fluent/ruby/bin/fluent-gem install fluent-plugin-s3
sudo /usr/lib64/fluent/ruby/bin/fluent-gem install fluent-plugin-forest
sudo /usr/lib64/fluent/ruby/bin/fluent-gem install fluent-plugin-config-expander

# 設定ファイルを編集する
sudo vi /etc/td-agent/td-agent.conf
```

## 03. 「データ分析基盤構築入門」を読む

[データ分析基盤構築入門［Fluentd，Elasticsearch，Kibana によるログ収集と可視化］](https://gihyo.jp/dp/ebook/2017/978-4-7741-9270-3)

> 本書は，ログデータを効率的に収集する Fluentd をはじめ，データストアとして注目を集めている Elasticsearch，可視化ツールの Kibana を解説します。本書を通して，ログ収集，データストア，可視化の役割を理解しながらデータ分析基盤を構築できます。

- Fluentd のバージョン ... v0.12.39
- Elasticsearch のバージョン ... v5.4.3
- Kibana のバージョン ... v5.4.3

### 本書の構成

第 1 部 データ分析基盤入門 1 章～ 4 章 ... ログを分析する背景や実際の流れ

第 2 部 ログ収集入門 5 章～ 8 章 ... Fluentd について説明

第 3 部 Elasticsearch 入門 9 章～ 14 章 ... Elasticsearch について解説

第 4 部 Kibana 入門 15 章～ 20 章 ... Kibana（Elasticsearch のフロントエンド）について説明

Appendix ... Fluentd プラグイン事典、Embulk&Digdag 入門、Embulk プラグイン事典、Kibana の便利機能

※ Embulk は、バッチ型バルクデータローダー

### 第 5 章 ログ収集ミドルウェアの紹介

#### AARRR（アー）モデル

サービス利用におけるユーザ行動のフェーズを 5 つの要素に分解し、フェーズごとに指標を取り、改善施策を立てやすくするためのフレームワーク

- Acquisition ユーザ獲得（どれだけ初回訪問を獲得できたか）
- Activation 活性化（はじめて利用したユーザが、どれだけ活性化したか）
- Retention 継続（どれだけのアクティブユーザを獲得できたか）
- Referral 紹介（既存ユーザが別の誰かにサービスを紹介し、どれだけ新規ユーザを獲得できたか）
- Revenue 収益化（ユーザがサービスの中でどれほど収益に貢献したか、どれだけの課金が成立したか）

### 第 6 章 はじめてみよう Fluentd

#### Fluentd のデータ構造

`[tag, time, record]`という 3 つの要素で構成される

- tag ... レコードのルーティングに使う文字列
- time ... UNIX タイムスタンプ
- record ... オブジェクト型、Key-Value 形式の連想配列

## 04. Elastic Stack とは

公式サイト => [Elastic のプロダクト：検索、分析、ロギング、セキュリティ \| Elastic](https://www.elastic.co/jp/products/)

| Key           | Value                          |
| ------------- | ------------------------------ |
| Elasticsearch | リアルタイム検索・分析エンジン |
| Kibana        | データ可視化ツール             |
| Beats         | 軽量なデータシッパー           |
| Logstash      | パイプラインデータ加工処理     |
