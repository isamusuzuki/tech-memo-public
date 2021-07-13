# Stackdriver

作成日 2021/07/12

## Stackdriver とは

解析記事 => [GCP for Stackdriver 設定マニュアル \- Qiita](https://qiita.com/kengomkt/items/f41fe1d08a06a3676c2f)

> Googleが2014年に買収したクラウド監視サービス。
>
> Stackdriver Monitoringではインスタンスのリソース使用率監視やUptimeCheck(URLへの死活監視や、GCEへのポート死活監視)、GCE上のプロセス稼働数などで監視を実現します。
>
> Stackdriver LoggingではGCP/OS/MWのログを監視し、特定の文言でフィルターをかけた指標とよばれるメトリックを作成します。そのメトリックをStackdriver Monitoringでアラート化し監視を実現します。

### Stackdriver Monitor Agent

> インストールすることにより、GCEのリソース使用率をより詳細に取得することができる。
>
> さらにサードパーティ製アプリケーション(Apache web serverやnginx,memcachedやMySQLなど)のモニタリングが可能となる。

インストール方法

```bash
curl -sSO https://dl.google.com/cloudagents/install-monitoring-agent.sh
sudo bash install-monitoring-agent.sh
```

> サードパーティ製アプリケーションのモニタリングをするには、各アプリケーションのプラグインのインストールが必要となる。

[サードパーティ製アプリケーションのモニタリング  \|  Cloud Monitoring  \|  Google Cloud](https://cloud.google.com/monitoring/agent/plugins/?hl=ja)
