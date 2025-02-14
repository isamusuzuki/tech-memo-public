# Cloud Scheduler を使う

作成日 2019/10/29

## 01. Cloud Scheduler とは

フルマネージド cron ジョブサービス

公式トップ => [https://cloud.google.com/scheduler/?hl=ja](https://cloud.google.com/scheduler/?hl=ja)

### Cloud Scheduler の料金体系

- 無料枠 ... ジョブ 3 つまで（アカウントレベルで、プロジェクトレベルにあらず）
- 月額 \$0.10 ... ジョブ 1 つにつき

## 02. 公式ドキュメントを読む

### Cloud Scheduler の概要

[https://cloud.google.com/scheduler/docs/?hl=ja](https://cloud.google.com/scheduler/docs/?hl=ja)

> Cloud Scheduler を使用して作成された各 cron ジョブは、\
> 指定のスケジュールに従ってターゲットに送信されます。\
> ターゲットは、次のいずれかのタイプでなければなりません。
>
> - HTTP/S エンドポイント
> - Cloud Pub/Sub トピック
> - App Engine アプリケーション

### cron ジョブの作成と構成

[https://cloud.google.com/scheduler/docs/creating](https://cloud.google.com/scheduler/docs/creating)

```bash
# HTTPターゲット
gcloud scheduler jobs create http JOB\
  --schedule=SCHEDULE\
  --uri=URI

# Pub/Subターゲット
cloud beta scheduler jobs create pubsub JOB\
  --schedule=SCHEDULE\
  --topic=TOPIC\
  --message-body=MESSAGE_BODY

# App Engineターゲット
gcloud beta scheduler jobs create app-engine JOB\
  --schedule=SCHEDULE

# ジョブを削除する
gcloud scheduler jobs delete JOB
```

### スケジュールの構成

[https://cloud.google.com/scheduler/docs/configuring/cron-job-schedules?hl=ja](https://cloud.google.com/scheduler/docs/configuring/cron-job-schedules?hl=ja)

unix-cron 形式（5 桁の数字）で表す

| No  | field        | available |
| :-: | ------------ | :-------: |
|  1  | min          |   0-59    |
|  2  | hour         |   0-23    |
|  3  | day of month |   1-31    |
|  4  | month        |   1-12    |
|  5  | day of week  |    0-6    |

スケジュールの例

- `* * * * *` ... 1 分ごと
- `0 */3 * * *` ... 3 時間ごと
- `0 3 * * *` ... 毎日朝 3 時
- `0 9 * * 1` ... 毎週月曜日の 9 時

## 03. unix-cron 形式について追加で勉強する

[クーロン\(cron\)をさわってみるお \- Qiita](https://qiita.com/katsukii/items/d5f90a6e4592d1414f99)

- `*/10 * * * *` ... 10 分おきに実行
- `0 */1 * * *` ... 毎時 0 分に 1 時間おきに実行
- `0 * * * *` ... 毎時 0 分に 1 時間おきに実行

## 04. 使えるようになるまで手順があった

自分のアカウントに`roles/cloudscheduler.admin`が与えられているのに、\
トップページを表示できなかった\
App Engine を有効にし、課金アカウントと結びつけたら使えるようになった

## 05. 実際にデプロイする

[https://cloud.google.com/sdk/gcloud/reference/scheduler/jobs/create/](https://cloud.google.com/sdk/gcloud/reference/scheduler/jobs/create/)

```bash
gcloud scheduler jobs create http JOB-NAME \
  --schedule "0 3 * * *" \
  --uri "https://www.exmaple.com/api" \
  --http-method POST \
  --time-zone "Asia/Tokyo"

gcloud scheduler jobs list
```
