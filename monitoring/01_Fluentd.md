# Fluentd

作成日 2019/12/11

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
