# Stackdriver をインストールする

作成日 2021/07/12

## 参考記事

[Stackdriver を利用した Google Compute Engine の監視設定方法 \| Tech ブログ \| JIG\-SAW OPS](https://ops.jig-saw.com/tech-cate/stackdriver_computeengine_monitoring)

[単一の VM に Cloud Monitoring エージェントをインストールする  \|  Google Cloud](https://cloud.google.com/monitoring/agent/monitoring/installation)

## エージェントのインストール

GCE からデータを収集するためには、エージェントのインストールが必要

```bash
# サーバーにログイン後
curl -sSO https://dl.google.com/cloudagents/add-monitoring-agent-repo.sh
sudo bash add-monitoring-agent-repo.sh --also-install
rm add-monitoring-agent-repo.sh

# 稼働状況を確認する
systemctl status stackdriver-agent
```

## リソースデータの監視設定

GCP マネジメントコンソール ＞ ナビゲーションメニュー ＞ オペレーション ＞ Monitoring ＞ アラート

「＋ CREATE POLICY」 ＞ What do you want to track? ＞「ADD CONDITION」ボタン ＞ 右スライド

Target

- Resource type: VM Instance
- Metric: CPU utilization (OS Reported)

※ 表示は後になっているが、先にメトリックのほうを検索する

Filter

- cpu_state = "user"

Group By

- system_labels.name

Aggregator

- mean

Period

- 5 minutes

Configuration

- Condition triggers if: Andy time series violates
- Condition: is above
- Threshold: 80 %
- For: 1 minute

「Add」ボタン ＞ 「次へ」ボタン

Who should be notified? ＞ Notification Channels

表示された選択肢にチェックを入れる、選択肢を追加したい場合は「通知チャンネルを管理」リンクをクリック ＞ 「OK」ボタン ＞ 「NEXT」ボタン

What are the steps to fix the issue?

アラート名を入力して ＞ 「SAVE」ボタン
